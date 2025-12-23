When a new GPP is created, and xml file is created in the SYSVOL share, which can be used to
- map drives (drives.xml)
- create local users
- create printer config files (printers.xml)
- creating and updating services (services.xml)
- creating scheduled tasks (scheduledtasks.xml)
- changing local admin passwords
  
The cpassword attribute is encrypted with AES-256, but Microsoft published the AES private key on MSDN
- patched in 2014 (MS14-025)
    
    - patching this does not remove existing Groups.xml files
    
[https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be?redirectedfrom=MSDN](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be?redirectedfrom=MSDN)
![image 375.png](../../assets/Group%20Policy%20Preferences%20(GPP)%20Passwords/image%20375.png)
  
can use metasploit to search for GPP files as well in the Post Module
# Decrypting Password with gpp-decrypt
```JavaScript
gpp-decrypt <cpasswd>
```
  
# Locating GPP with CME
![image 1 279.png](../../assets/Group%20Policy%20Preferences%20(GPP)%20Passwords/image%201%20279.png)
```JavaScript
crackmapexec smb -L | grep gpp
```
  
Also possible to find Registry.xml file
- can be used to auto log in on boot
- can hunt for this with crackmapexec using the gpp_autologin module, or using the Get-GPPAutologon.ps1 script from PowerSploit
```JavaScript
crackmapexec smb <IP> -u <user> -p <pass> -M gpp_autologin
```
![image 2 237.png](../../assets/Group%20Policy%20Preferences%20(GPP)%20Passwords/image%202%20237.png)
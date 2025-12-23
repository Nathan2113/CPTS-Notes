# Overview
Kerberos tickets grant you access to a specific resource
- usually when authenticated with NTLM hash, the hash is stored in memory so we are able to move laterally with it, but with Kerberos that doesn’t happen
- using mimikatz after initiating a WinRM shell through kerberos means that our password wouldn’t show up since it’s not stored in memory
  
This is a problem when we want to move laterally by using tools such as WinRM to authenticate over two or more sessions
  
![[../assets/Kerberos Double Hop/image 203.png|image 203.png]]
if we connect to Dev01 using a tool like evil-winrm, we connect with network authentication so the creds are not stored in memory
- so when we load a tool like PowerView and attempt to query the DC for information, Kerberos has no way of telling the DC that our user can access those resources → user has no way to verify their identity
- if unconstrained delegation is enabled on the server, the double hop problem most likely won’t be an issue
    
    - when a user sends their TGS to the target, the TGT will be sent along with it, meaning it can be used for further lateral movement
    
  
to verify if you need to worry about double hopping, you can open cmd and type klist
- if you see your tickets, you are good to go
![[../assets/Kerberos Double Hop/image 1 150.png|image 1 150.png]]
  
# Workarounds
## Workaround #1: PSCredential Object
use PSCredential object to pass our creds again to the next host
  
without doing this, we get an error because we can’t pass our auth on the Domain Controller
![[../assets/Kerberos Double Hop/image 2 134.png|image 2 134.png]]
  
To get around this, do the following steps:
  
Set up authentication
```JavaScript
$SecPassword = ConvertTo-SecureString '<pass>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('<DOMAIN>\<user>', $SecPassword)
```
  
Now we can run commands because we are passing creds along with it
```JavaScript
get-domainuser -spn -credential $Cred | select samaccountname
```
![[../assets/Kerberos Double Hop/image 3 120.png|image 3 120.png]]
- if you don’t use the -credential flag, it will throw the same error as above
  
  
# Workaround #2: Register PSSession Configuration
If we are on a Windows attack host or a remote host in the domain, and have the ability to connect to the target using WinRM or Enter-PSSession, we can interact with the DC without having to pass credentials with every command
  
First, establish a session on the remote host
```JavaScript
Enter-PSSession -ComputerName <name>.<DOMAIN> -Credential <DOMAIN>\<user>
```
  
run klist to see we that KDC called is empty (bottom)
![[../assets/Kerberos Double Hop/image 4 109.png|image 4 109.png]]
  
  
One trick is to register a new session configuration using “Register-PSSessionConfiguration”
```JavaScript
Register-PSSessionConfiguration -Name <name> -RunAsCredential <DOMAIN>\<user>
```
  
Restart the WinRM service by typing Restart-Service WinRM
- this will kick you out, so establish a new PSSession using the name set above
  
now that we have a new session, run klist again to see the KDC called
![[../assets/Kerberos Double Hop/image 5 106.png|image 5 106.png]]
  
can now run tools such as PowerView without creating a new PSCredential object
![[../assets/Kerberos Double Hop/image 6 94.png|image 6 94.png]]
  
# Note
- cannot use Register-PSSessionConfiguration from evil-winrm because we won’t be able to get the credentials popup
- if we try to set up a PSCredential object and attempt to run the command with “-RunAsCredential $Cred” it will throw an error because you can only use RunAs from an elevated PowerShell
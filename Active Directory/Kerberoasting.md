Kerberoasting can be done across forest trusts if authentication is permitted across the trust boundary
  
need at least one of the following to Kerberoast
- account’s cleartext password or NTLM hash
- shell access in the context of a domain user account
- SYSTEM level access on a domain-joined host
  
# Performing the Attack
the attack can be performed in multiple ways
- From a non-domain joined Linux host using valid domain user credentials.
- From a domain-joined Linux host as root after retrieving the keytab file.
- From a domain-joined Windows host authenticated as a domain user.
- From a domain-joined Windows host with a shell in the context of a domain account.
- As SYSTEM on a domain-joined Windows host.
- From a non-domain joined Windows host using [runas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771525\(v=ws.11\)) /netonly.
  
Several tools can be utilized to perform the attack
- Impacket’s [GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) from a non-domain joined Linux host.
- A combination of the built-in setspn.exe Windows binary, PowerShell, and Mimikatz.
- From Windows, utilizing tools such as PowerView, [Rubeus](https://github.com/GhostPack/Rubeus), and other PowerShell scripts.
  
# Efficacy of the Attack
- High risk finding if it leads to lateral movement and/or we are able to crack many
- Medium risk finding if we are only able to crack one and don’t get anywhere from it
  
# Encryption Types
Depending on the type of encryption used, it may not be worth trying to crack it as it will take too long
- if we see a hash that is using RC4 (type 23) → worth trying to crack
    
    - $krb5tgs$23$*
    
- if we see a hash using AES (type 17 & type 18) it may not be worth trying to crack
    
    - $krb5tgs$17$* and $krb5tgs$18$*
    
  
  
[[Export-08dd1444-22e8-4c73-ac0f-80d7e9ebe484/CPTS/Active Directory/Kerberoasting/From Linux|From Linux]]
[[Export-08dd1444-22e8-4c73-ac0f-80d7e9ebe484/CPTS/Active Directory/Kerberoasting/From Windows|From Windows]]
  
# Mitigation
- set long and complex passwords
- use Managed Service Accounts (MSA) and Group Managed Service Accounts (gMSA)
    
    - strong passwords, automatically rotate
    
  
# Detection
- Kerberoasting requests TGS tickets with RC4 encryption, which should not be the majority of Kerberos traffic
    
    - if we see an abnormal amount of TGS-REQ and TGS-REP requests, we know that Kerberoasting is most likely happening
    
- can log TGS ticket requests by selecting Audit Kerberos Service Ticket Operations in Group Policy
![[../assets/Kerberoasting/image 201.png|image 201.png]]
- Event ID 4769 - A Kerberos service ticket was requested
- Event ID 4770 - A Kerberos service ticket was renewed
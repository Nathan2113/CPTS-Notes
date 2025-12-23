Could leverage for lateral movement, privilege escalation, domain compromise, and persistence
  
GPO misconfigurations can be abused to perform the following
- Adding additional rights to a user (such as SeDebugPrivilege, SeTakeOwnershipPrivilege, or SeImpersonatePrivilege)
- Adding a local admin user to one or more hosts
- Creating an immediate scheduled task to perform any number of actions
  
# Enumerating GPO names with PowerView
```JavaScript
Get-DomainGPO |select displayname
```
![image 377.png](../../assets/Group%20Policy%20Object%20(GPO)%20Abuse/image%20377.png)
- can see that AutoLogon is used, meaning there may be a readable password in GPO
- also see that an AD CS is present on the domain
  
# Enumerating GPO Names with a Built-In Cmdlet
```JavaScript
Get-GPO -All | Select DisplayName
```
![image 1 281.png](../../assets/Group%20Policy%20Object%20(GPO)%20Abuse/image%201%20281.png)
  
# Enumerating Domain User GPO Rights
```JavaScript
$sid=Convert-NameToSid "Domain Users"
Get-DomainGPO | Get-ObjectAcl | ?{$_.SecurityIdentifier -eq $sid}
```
![image 2 239.png](../../assets/Group%20Policy%20Object%20(GPO)%20Abuse/image%202%20239.png)
- can see that Domain Users have various permissions over a GPO
    
    - WriteProperty and WriteDcal
    
# Converting GPO GUID to Name
```JavaScript
Get-GPO -Guid <GPO_GUID>
```
![image 3 205.png](../../assets/Group%20Policy%20Object%20(GPO)%20Abuse/image%203%20205.png)
  
Check BloodHound to see the rights that your user (in this case Domain Users) has over the GPO
![image 4 180.png](../../assets/Group%20Policy%20Object%20(GPO)%20Abuse/image%204%20180.png)
  
select the GPO and scroll to “Affected Objects” on “Node Info” to see what OU’s the GPO is applied to, and what the OU contains
![image 5 168.png](../../assets/Group%20Policy%20Object%20(GPO)%20Abuse/image%205%20168.png)
  
can take advantage of GPO configurations with SharpGPOAbuse
- add a user to the local admin groups on one of the affected hosts
- create immediate scheduled task
- configure malicious computer startup script
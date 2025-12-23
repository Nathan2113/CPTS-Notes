# Create an AD Snapshop with Active Directory Explorer
## Logging in with AD Explorer
![image 381.png](../../assets/Additional%20AD%20Auditing%20Techniques/image%20381.png)
  
## Browsing AD with AD Explorer
![image 1 285.png](../../assets/Additional%20AD%20Auditing%20Techniques/image%201%20285.png)
  
## Creating a Snapshop with AD Explorer
![image 2 242.png](../../assets/Additional%20AD%20Auditing%20Techniques/image%202%20242.png)
  
  
# PingCastle
evaluates overall security posture of an AD environment
- can give you a user-readable map of the domain
- can check PJPT notes for how to use
  
# Group3r
tool to find vulnerabilities in Active Directory associated with Group Policy
```JavaScript
group3r.exe -f <filepath-name.log>
```
![image 3 207.png](../../assets/Additional%20AD%20Auditing%20Techniques/image%203%20207.png)
- no indent → GPO itself
- one indent → policy settings
- two indents → findings
![image 4 182.png](../../assets/Additional%20AD%20Auditing%20Techniques/image%204%20182.png)
  
  
# ADRecon
```JavaScript
./ADRecon.ps1
```
![image 5 170.png](../../assets/Additional%20AD%20Auditing%20Techniques/image%205%20170.png)
  
will create logs you can look through to get more information about the domain and potential vulnerabilities
![image 6 145.png](../../assets/Additional%20AD%20Auditing%20Techniques/image%206%20145.png)
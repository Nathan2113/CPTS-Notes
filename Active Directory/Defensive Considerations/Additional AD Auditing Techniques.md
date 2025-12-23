# Create an AD Snapshop with Active Directory Explorer
## Logging in with AD Explorer
![[../../assets/Additional AD Auditing Techniques/image 381.png|image 381.png]]
  
## Browsing AD with AD Explorer
![[../../assets/Additional AD Auditing Techniques/image 1 285.png|image 1 285.png]]
  
## Creating a Snapshop with AD Explorer
![[../../assets/Additional AD Auditing Techniques/image 2 242.png|image 2 242.png]]
  
  
# PingCastle
evaluates overall security posture of an AD environment
- can give you a user-readable map of the domain
- can check PJPT notes for how to use
  
# Group3r
tool to find vulnerabilities in Active Directory associated with Group Policy
```JavaScript
group3r.exe -f <filepath-name.log>
```
![[../../assets/Additional AD Auditing Techniques/image 3 207.png|image 3 207.png]]
- no indent → GPO itself
- one indent → policy settings
- two indents → findings
![[../../assets/Additional AD Auditing Techniques/image 4 182.png|image 4 182.png]]
  
  
# ADRecon
```JavaScript
./ADRecon.ps1
```
![[../../assets/Additional AD Auditing Techniques/image 5 170.png|image 5 170.png]]
  
will create logs you can look through to get more information about the domain and potential vulnerabilities
![[../../assets/Additional AD Auditing Techniques/image 6 145.png|image 6 145.png]]
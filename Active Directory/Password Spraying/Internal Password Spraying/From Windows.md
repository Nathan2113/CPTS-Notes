# DomainPasswordSpray
## Authenticated
- do not need elevated powershell
- will automatically generate a user list from AD
- queries domain password policy and excludes users that are one attempt from locking out
    
    ```JavaScript
    Import-Module ./DomainPasswordSpray.ps1
    
    Invoke-DomainPasswordSpray -Password <pass_to_spray> -OutFile spray_success -ErrorAction SilentlyContinue
    ```
    
![image 359.png](/image%20359.png)
## Unauthenticated
- can still pass a user list to the tool
  
  
Could also just use Kerbrute
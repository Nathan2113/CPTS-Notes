SYSVOL can be a treasure trove of data
- VBScript and PowerShell scripts within script directory
    
    - readable to all users in domain
    
- can find old scripts containing since disabled accounts or old passwords
  
Example script called “reset_local_admin_pass.vbs” in SYSVOL
![[../../assets/Credentials in SMB Shares and SYSVOL Scripts/image 374.png|image 374.png]]
  
![[../../assets/Credentials in SMB Shares and SYSVOL Scripts/image 1 278.png|image 1 278.png]]
can test this password against all local administrators using crackmapexec with —local-auth flag
SYSVOL can be a treasure trove of data
- VBScript and PowerShell scripts within script directory
    
    - readable to all users in domain
    
- can find old scripts containing since disabled accounts or old passwords
  
Example script called “reset_local_admin_pass.vbs” in SYSVOL
![image 374.png](../../assets/Credentials%20in%20SMB%20Shares%20and%20SYSVOL%20Scripts/image%20374.png)
  
![image 1 278.png](../../assets/Credentials%20in%20SMB%20Shares%20and%20SYSVOL%20Scripts/image%201%20278.png)
can test this password against all local administrators using crackmapexec with —local-auth flag
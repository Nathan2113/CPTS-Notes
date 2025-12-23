![image 85.png](assets/Vintage/image%2085.png)
P.Rosa:Rosaisbest123
  
was able to get P.Rosa’s kerberos ticket
![image 1 52.png](assets/Vintage/image%201%2052.png)
- the authentication worked, but there was an error enumerating
![image 2 52.png](assets/Vintage/image%202%2052.png)
  
No SPN entries
![image 3 45.png](assets/Vintage/image%203%2045.png)
  
![image 4 41.png](assets/Vintage/image%204%2041.png)
:)
  
![image 5 40.png](assets/Vintage/image%205%2040.png)
  
Going into bloodhound and seeing FS01 has ReadGMSAPassword on GMSA01$
![image 6 36.png](assets/Vintage/image%206%2036.png)
- I guess domain computers can read gmsa passwords
  
can get FS01$’s ticket from TGT
- FS01$:fs01
    
    - maybe the default password is just the name of the computer?
    
![image 7 35.png](assets/Vintage/image%207%2035.png)
  
export ccache
![image 8 32.png](assets/Vintage/image%208%2032.png)
  
use bloodyAD to get hash
- bloodyAD --host dc01.vintage.htb -d "vintage.htb" -k get object 'GMSA01$' --attr msDS-ManagedPassword
![image 9 32.png](assets/Vintage/image%209%2032.png)
  
NTLM Hash for GMSA01$
- aad3b435b51404eeaad3b435b51404ee:7dc430b95e17ed6f817f69366f35be06
  
  
GMSA01$ has GenericWrite and AddSelf over ServiceManagers
![image 10 27.png](assets/Vintage/image%2010%2027.png)
  
and ServiceManagers has GenericAll over all service accounts
![image 11 26.png](assets/Vintage/image%2011%2026.png)
  
going to try to AddSelf GMSA01$ → ServiceManagers
- get-TGT and export
![image 12 20.png](assets/Vintage/image%2012%2020.png)
  
- bloodyAD --host dc01.vintage.htb -d vintage.htb -u 'GMSA01$' -k add groupMember "CN=SERVICEMANAGERS,OU=PRE-MIGRATION,DC=VINTAGE,DC=HTB" 'GMSA01$’
![image 13 20.png](assets/Vintage/image%2013%2020.png)
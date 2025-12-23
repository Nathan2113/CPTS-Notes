can look for requests to get information on IP addresses
```JavaScript
sudo tcpdump -i <interface>
```
```JavaScript
responder -I <interface> -A 
```
![[../assets/Initial Enumeration of Domain/image 198.png|image 198.png]]
- gives IP addresses and hostnames
  
once we enumerate hostnames and ip addresses, we can being active enumeration using an fping ping sweep
```JavaScript
fping -asgq <CIDR>
```
- a - targets that are alive
- s - stats at end of scan
- g - generate target list
- q - do not show per target results
  
now that we have a list of hosts, perform nmap scan on each one
```JavaScript
nmap -v -A -iL hosts.txt -oA <output_name>
```
- oA - gives all output formats for scan
  
  
now that we have hosts and scans, if we don’t have any users we can try kerbrute to find a valid domain user
https://github.com/ropnop/kerbrute
- can get precompiled binaries here
https://github.com/ropnop/kerbrute/releases/tag/v1.0.3
- use statisically likely usernames to enumerate for users
    
    - kerbrute is stealthier as pre-auth fails often do not trigger logs
    
https://github.com/insidetrust/statistically-likely-usernames
```JavaScript
kerbrute userenum -d <domain> --dc <ip> <userlist> -o valid_ad_users
```
  
once you get NT AUTHORITY\SYSTEM on a machine, you can use it to impersonate the machine account — almost equivalent to a domain user
![[../assets/Initial Enumeration of Domain/image 1 146.png|image 1 146.png]]
- if you see SeImpersonate, you can run Juicy Potato
https://github.com/ohpe/juicy-potato
  
once you have system access, you can do a couple attacks
![[../assets/Initial Enumeration of Domain/image 2 130.png|image 2 130.png]]
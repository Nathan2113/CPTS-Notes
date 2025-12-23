# Online Presence
SSL certificates can give you some good information
![[/image 46.png|image 46.png]]
- can also find more subdomains at crt.sh
[https://crt.sh/](https://crt.sh/)
  
can output the results fo [crt.sh](http://crt.sh) into a json flie
```JavaScript
curl -s https://crt.sh/\?q\=<DOMAIN>\&output\=json | jq .
```
![[/image 1 29.png|image 1 29.png]]
or sort the output into unique subdomains
```JavaScript
curl -s https://crt.sh/\?q\=<DOMAIN>\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```
![[../assets/Enumeration/image 167.png|image 167.png]]
  
can take this output and sort by first-party hosts that are directly accessible
- you want to filter out third-party to avoid going out of scope
```JavaScript
for i in $(cat subdomainlist);do host $i | grep "has address" | grep <DOMAIN> | cut -d" " -f1,4;done
```
![[../assets/Enumeration/image 2 118.png|image 2 118.png]]
  
  
## Shodan - IP List
- once we know what hosts we can investigate further, we can generate a list of IP addresses and run them through Shodan
```JavaScript
for i in $(cat subdomainlist);do host $i | grep "has address" | grep <DOMAIN> | cut -d" " -f4 >> ip-addresses.txt;done
for i in $(cat ip-addresses.txt);do shodan host $i;done
```
![[../assets/Enumeration/image 3 108.png|image 3 108.png]]
  
## DNS Records
```JavaScript
dig any <DOMAIN>
```
![[../assets/Enumeration/image 4 100.png|image 4 100.png]]
- A records - IP addresses that point to a specific (sub)domain
- MX records - mail server responsible for managing emails for the company
    
    - in this example, we can see they’re managed by google (ignore for now)
    
- NS - shows which name servers are used to resolve the FQDN to IP address
    
    - most hosting providers use their own name servers
    
- TXT records - contains verification keys for different third-party providers and other security aspect of DNS
    
    - SPF, DMARC, DKIM
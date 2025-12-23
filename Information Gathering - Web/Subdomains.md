# Active Subdomain Enumeration
- looking for DNS Zone Transfers
- brute-force enumeration
    
    - dnsenum
    
    - fierce
    
    - amass
    
    - assetfinder
    
    - puredns
    
    - ffuf
    
    - gobuster
    
  
# Passive Subdomain Enumeration
- Certificate Transparency (CT) logs
    
    - public repo of SSL/TLS certficiates
    
    - often have a list of subdomains included in their Subject Alternative Name (SAN) field
    
- google dorking
    
    - site:<site>
        
        - site:github.com
        
    
  
# Subdomain Bruteforcing
## DNSEnum
- DNS Record enumeration
- Zone Transfer attempts
- Subdomain Brute-Forcing
- Google Scraping
- Reverse Lookup
- WHOIS Lookups
  
```JavaScript
dnsenum --enum <DOMAIN> -f /usr/share/SecLists/Discovery/DNS/subdomains-top1million-20000.txt -r
```
```JavaScript
Nathan2112@htb[/htb]$ dnsenum --enum inlanefreight.com -f  /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt 
dnsenum VERSION:1.2.6
-----   inlanefreight.com   -----

Host's addresses:
__________________
inlanefreight.com.                       300      IN    A        134.209.24.248
[...]
Brute forcing with /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt:
_______________________________________________________________________________________
www.inlanefreight.com.                   300      IN    A        134.209.24.248
support.inlanefreight.com.               300      IN    A        134.209.24.248
[...]

done.
```
- -r for recursive
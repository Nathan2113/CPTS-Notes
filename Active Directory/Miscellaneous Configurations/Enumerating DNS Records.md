Enumerate DNS records with “adidnsdump” using a valid user account
- especially useful if the naming convention of the computer is similar to “SRV01934.<DOMAIN>”
    
    - if we access DNS records for the domain, you can discover DNS servers
    
https://github.com/dirkjanm/adidnsdump
  
# Using adidnsdump
```JavaScript
adidnsdump -u <DOMAIN>\\<user> ldap://<IP>
```
![[../../assets/Enumerating DNS Records/image 371.png|image 371.png]]
  
can use the -r flag to resolve unknown records by performing an “A” query
```JavaScript
adidnsdump -u <DOMAIN>\\<user> ldap://<IP> -r
```
![[../../assets/Enumerating DNS Records/image 1 277.png|image 1 277.png]]
## Viewing Contents of records.csv
![[../../assets/Enumerating DNS Records/image 2 236.png|image 2 236.png]]
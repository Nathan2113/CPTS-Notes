Enumerate DNS records with “adidnsdump” using a valid user account
- especially useful if the naming convention of the computer is similar to “SRV01934.<DOMAIN>”
    
    - if we access DNS records for the domain, you can discover DNS servers
    
https://github.com/dirkjanm/adidnsdump
  
# Using adidnsdump
```JavaScript
adidnsdump -u <DOMAIN>\\<user> ldap://<IP>
```
![image 371.png](../../assets/Enumerating%20DNS%20Records/image%20371.png)
  
can use the -r flag to resolve unknown records by performing an “A” query
```JavaScript
adidnsdump -u <DOMAIN>\\<user> ldap://<IP> -r
```
![image 1 277.png](../../assets/Enumerating%20DNS%20Records/image%201%20277.png)
## Viewing Contents of records.csv
![image 2 236.png](../../assets/Enumerating%20DNS%20Records/image%202%20236.png)
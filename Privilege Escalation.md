# Privesc Checklists
- HackTricks
- PayloadsAllTheThings
  
# Common Linux Enumeration Scripts
- LinEnum
- linexprivchecker
- linPEAS
  
# Common Windows Enumeration Scripts
- Seatbelt
- JAWS
  
# Kernel Exploits
- look at OS version the system is running
- can cause system instability
  
# User Privileges
## Sudo
- sudo -l
    
    - GTFObins
    
## SUID
  
  
## LOLBAS
- list of Windows applications which we can use to perform certain functions as a privileged user
    
    - download files
    
    - execute commands
    
  
## Windows Token Privileges
  
  
# Scheduled Tasks
if we can write to any of these, create a bash script to get root
- /etc/crontab
- /etc/cron.d
- /var/spool/cron/crontabs/root
  
  
# Exposed Credentials
- log files
- config files
- bash_history
- PSReadLine (Windows)
  
  
# SSH Keys
```JavaScript
vim id_rsa
chmod 600 id_rsa
ssh <user>@<IP> -i id_rsa
```
- if we have write to .ssh folder, put our public key in authorized_keys
```JavaScript
ssh-keygen -f key
echo <public_key_contents> >> /<user>/.ssh/authorized_keys
```
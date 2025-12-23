# PAM
- Pluggable Authentication Modules
- responsible for pam_unix.zo, pam_unix2.so
    
    - stored in `/usr/lib/x86_64-linux-gnu/security/` on debian-based systems
    
- pam_unix.so uses API calls from system libraries to update account info
    
    - reads and writes from /etc/passwd and /etc/shadow
    
  
# Passwd Files
- each entry has seven fields
- password hashes stored in /etc/shadow on modern systems
- the ‘x’ field means that the password hash is stored in /etc/shadow
```Markdown
htb-student:x:1000:1000:,,,:/home/htb-student:/bin/bash
```
|**Field**|**Value**|
|---|---|
|Username|`htb-student`|
|Password|`x`|
|User ID|`1000`|
|Group ID|`1000`|
|[GECOS](https://en.wikipedia.org/wiki/Gecos_field)|`,,,`|
|Home directory|`/home/htb-student`|
|Default shell|`/bin/bash`|
  
- if /etc/passwd is writable by accident, you can remove root entirely
```Markdown
Nathan2112@htb[/htb]$ head -n 1 /etc/passwd
root::0:0:root:/root:/bin/bash
```
- results in no password prompt when switching to root
```Markdown
Nathan2112@htb[/htb]$ su
root@htb[/htb]#
```
  
  
# Shadow File
- nine fields
```Markdown
htb-student:$y$j9T$3QSBB6CbHEu...SNIP...f8Ms:18955:0:99999:7:::
```
|**Field**|**Value**|
|---|---|
|Username|`htb-student`|
|Password|`$y$j9T$3QSBB6CbHEu...SNIP...f8Ms`|
|Last change|`18955`|
|Min age|`0`|
|Max age|`99999`|
|Warning period|`7`|
|Inactivity period|`-`|
|Expiration date|`-`|
|Reserved field|`-`|
- can glean hash information from the output
    
    - $<id>$<salt>$<hashed>
    
|**ID**|**Cryptographic Hash Algorithm**|
|---|---|
|`1`|[MD5](https://en.wikipedia.org/wiki/MD5)|
|`2a`|[Blowfish](https://en.wikipedia.org/wiki/Blowfish_\(cipher\))|
|`5`|[SHA-256](https://en.wikipedia.org/wiki/SHA-2)|
|`6`|[SHA-512](https://en.wikipedia.org/wiki/SHA-2)|
|`sha1`|[SHA1crypt](https://en.wikipedia.org/wiki/SHA-1)|
|`y`|[Yescrypt](https://github.com/openwall/yescrypt)|
|`gy`|[Gost-yescrypt](https://www.openwall.com/lists/yescrypt/2019/06/30/1)|
|`7`|[Scrypt](https://en.wikipedia.org/wiki/Scrypt)|
  
# Opasswd
- pam_unix.so can prevent users from reusing old passwords
- these old passwords are stored in /etc/security/opasswd
```Markdown
Nathan2112@htb[/htb]$ sudo cat /etc/security/opasswd
cry0l1t3:1000:2:$1$HjFAfYTG$qNDkF0zJ3v8ylCOrKB0kt0,$1$kcUjWZJX$E9uMSmiQeRh4pAAgzuvkq1
```
  
# Cracking Linux Credentials
```Markdown
# Unshadowing
unshadow passwd.bak shadow.bak > unshadowed.hashes
# Cracking
hashcat -m 1800 -a 0 unshadowed.hashes /usr/share/wordlists/rockyou.txt -o unshadowed.cracked
```
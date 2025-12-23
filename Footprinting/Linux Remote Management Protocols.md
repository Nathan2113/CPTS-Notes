# SSH
- banner for SSH starts with the version of the protocol that can be applied, followed by the version of the server itself
    
    - SSH-1.99-OpenSSH_3.9p1 means that we can use both protocol versions SSH-1 and 2, and we are dealing with an OpenSSH server 3.9p1
    
    - SSH-2.0-OpenSSH_8.2p1 means an OpenSSH server 8.2p1 that only accepts SSH-2
    
## Default Configuration
```JavaScript
Nathan2112@htb[/htb]$ cat /etc/ssh/sshd_config  | grep -v "#" | sed -r '/^\s*$/d'
Include /etc/ssh/sshd_config.d/*.conf
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding yes
PrintMotd no
AcceptEnv LANG LC_*
Subsystem       sftp    /usr/lib/openssh/sftp-server
```
  
## Dangerous Settings
|**Setting**|**Description**|
|---|---|
|`PasswordAuthentication yes`|Allows password-based authentication.|
|`PermitEmptyPasswords yes`|Allows the use of empty passwords.|
|`PermitRootLogin yes`|Allows to log in as the root user.|
|`Protocol 1`|Uses an outdated version of encryption.|
|`X11Forwarding yes`|Allows X11 forwarding for GUI applications.|
|`AllowTcpForwarding yes`|Allows forwarding of TCP ports.|
|`PermitTunnel`|Allows tunneling.|
|`DebianBanner yes`|Displays a specific banner when logging in.|
  
## Footprinting SSH
- can use a tool called ssh-audit
```JavaScript
ssh-audit <IP>
```
```JavaScript
Nathan2112@htb[/htb]$ ./ssh-audit.py 10.129.14.132
# general
(gen) banner: SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.3
(gen) software: OpenSSH 8.2p1
(gen) compatibility: OpenSSH 7.4+, Dropbear SSH 2018.76+
(gen) compression: enabled (zlib@openssh.com)                                   
# key exchange algorithms
(kex) curve25519-sha256                     -- [info] available since OpenSSH 7.4, Dropbear SSH 2018.76                            
(kex) curve25519-sha256@libssh.org          -- [info] available since OpenSSH 6.5, Dropbear SSH 2013.62
(kex) ecdh-sha2-nistp256                    -- [fail] using weak elliptic curves
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
(kex) ecdh-sha2-nistp384                    -- [fail] using weak elliptic curves
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
(kex) ecdh-sha2-nistp521                    -- [fail] using weak elliptic curves
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
(kex) diffie-hellman-group-exchange-sha256 (2048-bit) -- [info] available since OpenSSH 4.4
(kex) diffie-hellman-group16-sha512         -- [info] available since OpenSSH 7.3, Dropbear SSH 2016.73
(kex) diffie-hellman-group18-sha512         -- [info] available since OpenSSH 7.3
(kex) diffie-hellman-group14-sha256         -- [info] available since OpenSSH 7.3, Dropbear SSH 2016.73
# host-key algorithms
(key) rsa-sha2-512 (3072-bit)               -- [info] available since OpenSSH 7.2
(key) rsa-sha2-256 (3072-bit)               -- [info] available since OpenSSH 7.2
(key) ssh-rsa (3072-bit)                    -- [fail] using weak hashing algorithm
                                            `- [info] available since OpenSSH 2.5.0, Dropbear SSH 0.28
                                            `- [info] a future deprecation notice has been issued in OpenSSH 8.2: https://www.openssh.com/txt/release-8.2
(key) ecdsa-sha2-nistp256                   -- [fail] using weak elliptic curves
                                            `- [warn] using weak random number generator could reveal the key
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
(key) ssh-ed25519                           -- [info] available since OpenSSH 6.5
...SNIP...
```
- banner reveals OpenSSH server and version
  
### Change Authentication Method
```JavaScript
Nathan2112@htb[/htb]$ ssh -v cry0l1t3@10.129.14.132
OpenSSH_8.2p1 Ubuntu-4ubuntu0.3, OpenSSL 1.1.1f  31 Mar 2020
debug1: Reading configuration data /etc/ssh/ssh_config 
...SNIP...
debug1: Authentications that can continue: publickey,password,keyboard-interactive
```
- can see that one of the authentication options is password
  
- for potential brute-force, set the PreferredAuthentications flag to password
```JavaScript
ssh -v <user>@<IP> -o PreferredAuthentications=password
```
```JavaScript
Nathan2112@htb[/htb]$ ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password
OpenSSH_8.2p1 Ubuntu-4ubuntu0.3, OpenSSL 1.1.1f  31 Mar 2020
debug1: Reading configuration data /etc/ssh/ssh_config
...SNIP...
debug1: Authentications that can continue: publickey,password,keyboard-interactive
debug1: Next authentication method: password
cry0l1t3@10.129.14.132's password:
```
  
  
# Rsync
- tool for locally and remotely copying files
- listens on TCP/873
- can be configured to use SSH for secure transfers by piggybacking on top of an SSH session
- can be abused by listing the contents of a shared folder and retrieving files
    
    - sometimes can be done without authentication
    
  
### Scanning for Rsync
```JavaScript
sudo nmap -sV -p 873 <IP>
```
```JavaScript
Nathan2112@htb[/htb]$ sudo nmap -sV -p 873 127.0.0.1
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-19 09:31 EDT
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0058s latency).
PORT    STATE SERVICE VERSION
873/tcp open  rsync   (protocol version 31)
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.13 seconds
```
- can see protocol version 31
  
### Probing for an Open Share
```JavaScript
rsync -av --list-only rsync:://IP>/<share>
```
```JavaScript
Nathan2112@htb[/htb]$ rsync -av --list-only rsync://127.0.0.1/dev
receiving incremental file list
drwxr-xr-x             48 2022/09/19 09:43:10 .
-rw-r--r--              0 2022/09/19 09:34:50 build.sh
-rw-r--r--              0 2022/09/19 09:36:02 secrets.yaml
drwx------             54 2022/09/19 09:43:10 .ssh
sent 25 bytes  received 221 bytes  492.00 bytes/sec
total size is 0  speedup is 0.00
```
- if ssh is used, we can include ‘-e ssh’ or ‘-e “ssh -p2222”’
  
  
## R-Services
- suite of services hosted to enable remote access or issue commands between Unix hosts
- unencrypted like telnet
- use TCP ports 512, 513, and 514
  
### R-Commands
- rcp (`remote copy`)
- rexec (`remote execution`)
- rlogin (`remote login`)
- rsh (`remote shell`)
- rstat
- ruptime
- rwho (`remote who`)
  
### Abusing R-sync
  
**Commonly Abused Commands**
|**Command**|**Service Daemon**|**Port**|**Transport Protocol**|**Description**|
|---|---|---|---|---|
|`rcp`|`rshd`|514|TCP|Copy a file or directory bidirectionally from the local system to the remote system (or vice versa) or from one remote system to another. It works like the `cp` command on Linux but provides `no warning to the user for overwriting existing files on a system`.|
|`rsh`|`rshd`|514|TCP|Opens a shell on a remote machine without a login procedure. Relies upon the trusted entries in the `/etc/hosts.equiv` and `.rhosts` files for validation.|
|`rexec`|`rexecd`|512|TCP|Enables a user to run shell commands on a remote machine. Requires authentication through the use of a `username` and `password` through an unencrypted network socket. Authentication is overridden by the trusted entries in the `/etc/hosts.equiv` and `.rhosts` files.|
|`rlogin`|`rlogind`|513|TCP|Enables a user to log in to a remote host over the network. It works similarly to `telnet` but can only connect to Unix-like hosts. Authentication is overridden by the trusted entries in the `/etc/hosts.equiv` and `.rhosts` files.|
  
**/etc/hosts.equiv**
- contains a list of trusted hosts
    
    - if a user attempts access from one of these hosts, they are automatically granted access
    
```JavaScript
Nathan2112@htb[/htb]$ cat /etc/hosts.equiv
# <hostname> <local username>
pwnbox cry0l1t3
```
  
## Footprinting RSync
  
### Nmap
```JavaScript
sudo nmap -sV -p 512,513,514 <IP>
```
```JavaScript
Nathan2112@htb[/htb]$ sudo nmap -sV -p 512,513,514 10.0.17.2
Starting Nmap 7.80 ( https://nmap.org ) at 2022-12-02 15:02 EST
Nmap scan report for 10.0.17.2
Host is up (0.11s latency).
PORT    STATE SERVICE    VERSION
512/tcp open  exec?
513/tcp open  login?
514/tcp open  tcpwrapped
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 145.54 seconds
```
  
### Access Control and Trusted Relationships
- by default, Rsync uses Pluggable Authentication Modules (PAM)
    
    - also bypasses authentication through /etc/hosts.equiv and .rhosts files
        
        - hosts.equiv is global, .rhosts is per-user
        
    
  
### Sample .rhosts Files
```JavaScript
Nathan2112@htb[/htb]$ cat .rhosts
htb-student     10.0.17.5
+               10.0.17.10
+               +
```
- ‘+’ is a wildcard in this file
    
    - in this example, any external user can access r-commands from the htb-student account via the host 10.0.17.10
    
  
### Logging in with Rlogin
```JavaScript
rlogin <IP> -l <user>
```
```JavaScript
Nathan2112@htb[/htb]$ rlogin 10.0.17.2 -l htb-student
Last login: Fri Dec  2 16:11:21 from localhost
[htb-student@localhost ~]$
```
  
### Listing Authenticate Users Using Rwho
```JavaScript
Nathan2112@htb[/htb]$ rwho
root     web01:pts/0 Dec  2 21:34
htb-student     workstn01:tty1  Dec  2 19:57  2:25  
```
- can see that htb-student is currently authenticated to the workstn01 host
- root user is authenticated to the web01 host
- rwho daemon occasionally broadcasts information about logged-on users, so it could be beneficial to watch network traffic
  
### Listing Authenticated Users Using Rusers
```JavaScript
rusers -al <IP>
```
```JavaScript
Nathan2112@htb[/htb]$ rusers -al 10.0.17.5
htb-student     10.0.17.5:console          Dec 2 19:57     2:25
```
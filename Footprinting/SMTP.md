responsbile for sending emails out in a network
- can be used between an email client and an outgoing mail server or between two SMTP servers
- often combined with IMAP or POP3, which can fetch and send emails
- most often seen on port 25, but newer SMTP servers can also use TCP port 587
    
    - utilizes the STARTTLS command to switch to encrypted connection
    
    - encrypted connections can use other ports, for example, TCP port 465
    
  
Client (MUA) → Submission Agent (MSA) → Open Relay (MTA) → Mail Delivery Agent (MDA) → Mailbox (POP3/IMAP)
- MUA - Mail User Agent
- MSA - Mail Submission Agent
- MTA - Mail Transfer Agent
- MDA - Mail Delivery Agent
  
SMTP has two main disadvantages inherent to the network protocol
1. sending an email using SMTP does not return a usable deliver confirmation
1. users are not authenticated when a connection is established
    
    1. sender of an email is unverifiable
    
    1. to prevent this, DKIM and SPF are used
    
- because of these shortcomings, SMTP developed an extension called Extended SMTP (ESMTP)
    
    - this is often what people are referring to when the say SMTP
    
    - ESMTP uses TLS, which is done after EHLO command by sending STARTTLS
    
  
# Default Configuration
```JavaScript
Nathan2112@htb[/htb]$ cat /etc/postfix/main.cf | grep -v "#" | sed -r "/^\s*$/d"
smtpd_banner = ESMTP Server 
biff = no
append_dot_mydomain = no
readme_directory = no
compatibility_level = 2
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
myhostname = mail1.inlanefreight.htb
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
smtp_generic_maps = hash:/etc/postfix/generic
mydestination = $myhostname, localhost 
masquerade_domains = $myhostname
mynetworks = 127.0.0.0/8 10.129.0.0/16
mailbox_size_limit = 0
recipient_delimiter = +
smtp_bind_address = 0.0.0.0
inet_protocols = ipv4
smtpd_helo_restrictions = reject_invalid_hostname
home_mailbox = /home/postfix
```
- the sending and communication are also done by special commands
|**Command**|**Description**|
|---|---|
|`AUTH PLAIN`|AUTH is a service extension used to authenticate the client.|
|`HELO`|The client logs in with its computer name and thus starts the session.|
|`MAIL FROM`|The client names the email sender.|
|`RCPT TO`|The client names the email recipient.|
|`DATA`|The client initiates the transmission of the email.|
|`RSET`|The client aborts the initiated transmission but keeps the connection between client and server.|
|`VRFY`|The client checks if a mailbox is available for message transfer.|
|`EXPN`|The client also checks if a mailbox is available for messaging with this command.|
|`NOOP`|The client requests a response from the server to prevent disconnection due to time-out.|
|`QUIT`|The client terminates the session.|
- to interact with the SMTP server, we can connect with telnet
```JavaScript
Nathan2112@htb[/htb]$ telnet 10.129.14.128 25
Trying 10.129.14.128...
Connected to 10.129.14.128.
Escape character is '^]'.
220 ESMTP Server 

HELO mail1.inlanefreight.htb
250 mail1.inlanefreight.htb

EHLO mail1
250-mail1.inlanefreight.htb
250-PIPELINING
250-SIZE 10240000
250-ETRN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250-SMTPUTF8
250 CHUNKING
```
- the command CRFY can be used to enumerate existing users on the system
    
    - this does not always work
    
    - depending on configuration, the SMTP esrer may issue code 252 confirming that a user does not exist on the server
    
- below is all SMTP error codes
[https://serversmtp.com/smtp-error/?doing_wp_cron=1764771662.2541580200195312500000](https://serversmtp.com/smtp-error/?doing_wp_cron=1764771662.2541580200195312500000)
```JavaScript
Nathan2112@htb[/htb]$ telnet 10.129.14.128 25
Trying 10.129.14.128...
Connected to 10.129.14.128.
Escape character is '^]'.
220 ESMTP Server 
VRFY root
252 2.0.0 root

VRFY cry0l1t3
252 2.0.0 cry0l1t3

VRFY testuser
252 2.0.0 testuser

VRFY aaaaaaaaaaaaaaaaaaaaaaaaaaaa
252 2.0.0 aaaaaaaaaaaaaaaaaaaaaaaaaaaa
```
  
to connect to an SMTP server using a web proxy, the command will look something like this
```JavaScript
CONNECT <IP>:25 HTTP/1.0
```
  
## Sending an Email Example
```JavaScript
Nathan2112@htb[/htb]$ telnet 10.129.14.128 25
Trying 10.129.14.128...
Connected to 10.129.14.128.
Escape character is '^]'.
220 ESMTP Server

EHLO inlanefreight.htb
250-mail1.inlanefreight.htb
250-PIPELINING
250-SIZE 10240000
250-ETRN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250-SMTPUTF8
250 CHUNKING

MAIL FROM: <cry0l1t3@inlanefreight.htb>
250 2.1.0 Ok

RCPT TO: <mrb3n@inlanefreight.htb> NOTIFY=success,failure
250 2.1.5 Ok

DATA
354 End data with <CR><LF>.<CR><LF>
From: <cry0l1t3@inlanefreight.htb>
To: <mrb3n@inlanefreight.htb>
Subject: DB
Date: Tue, 28 Sept 2021 16:32:51 +0200
Hey man, I am trying to access our XY-DB but the creds don't work. 
Did you make any changes there?
.
250 2.0.0 Ok: queued as 6E1CF1681AB

QUIT
221 2.0.0 Bye
Connection closed by foreign host.
```
  
# Dangerous Settings
- to prevent a valid email from being marked as spam, you can use a relay server
    
    - SMTP server that is known and verified by all others
    
    - user must authenticate to the relay server before using it
    
    - leads to misconfigurations as administrators are often not aware of what IP ranges they have to allow, so they allow all IP addresses to avoid errors
    
  
## Open Relay Configuration
```JavaScript
mynetworks = 0.0.0.0/0
```
- with this setting, the SMTP server can send fake emails
- another attack is to spoof the email and read it
  
# Footprinting SMTP
## Nmap
```JavaScript
sudo nmap <IP> -sC -sV -p25
```
![[/image 137.png|image 137.png]]
  
## Nmap - Open Relay
```JavaScript
sudo nmap <IP> -p25 --script smtp-open-relay -v
```
![[/image 1 103.png|image 1 103.png]]
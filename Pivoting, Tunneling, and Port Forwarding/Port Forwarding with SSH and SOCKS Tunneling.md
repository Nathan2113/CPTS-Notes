# Local Port Forward
- if we want to run a remote exploit on a MySQL server, we will have to port forward first
- we can use local port forwarding to have the remote port 3306 forward to our local port (in this example 1234)
```JavaScript
ssh -L 1234:localhost:3306 <user>@<IP>
```
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 193.png|image 193.png]]
- this tells ssh client to send data via port 1234 to localhost:3306
  
- you can confirm the port forward with Netstat or Nmap
## Netstat
```JavaScript
netstat -antp | grep 1234
```
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 1 142.png|image 1 142.png]]
  
## Nmap
```JavaScript
nmap -v -p1234 localhost
```
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 2 129.png|image 2 129.png]]
  
## Forwarding Multiple Ports
```JavaScript
ssh -L 1324:localhost3306 -L 8080:localhost:80 <user>@<IP>
```
  
  
# Setting up to Pivot
check the ifconfig on the host to see if they have multiple NICs
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 3 116.png|image 3 116.png]]
- if we are on the same subnet as ens192, we will have to set up dynamic port forwarding to be able to interact to the network that ens224 is connected to
- need to set up a SOCKS listener on our local host, then configure SSH to forward traffic via SSH to the internal network
    
    - this is called SSH tunneling over SOCKS proxy
        
        - SOCKS = Socket Secure
        
    
    - SOCKS4 does not provide authentication and UDP support, SOCKS 5 does
    
  
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 4 105.png|image 4 105.png]]
- SSH client requests server to send some packets over the server, server acknowledges the request and starts listening on port 9050
    
    - whatever data you send will be sent to the entire other network (in this case 172.16.5.0/23
    
  
## Enabling Dynamic Port Forwarding with SSH
```JavaScript
ssh -D 9050 <user>@<IP>
```
- we need a tool that can route any tool’s packets over port 9050
    
    - can do this using proxychains
        
        - can redirect TCP connections through TOR, SOCKS, and HTTP/HTTPS
        
    
  
## Checking /etc/proxychains.conf
- in order to inform proxychains to use port 9050, we need to modify /etc/proxychains.conf
    
    - add “socks4 127.0.0.1 9050” to the last line
    
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 5 102.png|image 5 102.png]]
- now, when you run commands you want to route with proxychains, prepend all commands with “proxychains <command>”
## Using Nmap with Proxychains
```JavaScript
proxychains nmap -v -sn <IP> OR <IP_RANGE>
```
- for IP_RANGE, you can do something like 172.16.5.1-200
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 6 91.png|image 6 91.png]]
- proxychains doesn’t understand partial packets, so we aren’t able to do half connect scans like usual
    
    - proxychains will send the entire TCP connect scan
    
    - doing an nmap scan with full TCP connect scans takes a long time
    
  
  
## Enumerating Windows Target through Proxychains
```JavaScript
proxychains nmap -v -Pn -sT <IP>
```
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 7 88.png|image 7 88.png]]
- can see ports 135, 139, 445, and 3389 are up
  
# Using Metasploit with Proxychains
```JavaScript
proxychains msfconsole
```
- for our example, RDP was open on the host, so we’re going to try rdp_scanner
```JavaScript
search rdp-scanner
```
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 8 79.png|image 8 79.png]]
- we can see from this that RDP is open
  
## Using xfreerdp with Proxychains
```JavaScript
proxychains xfreerdp /v:<IP> /u:<user> /p:<pass>
```
![[../assets/Port Forwarding with SSH and SOCKS Tunneling/image 9 76.png|image 9 76.png]]
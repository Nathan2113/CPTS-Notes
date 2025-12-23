# Scripts
enumerate common web application directories
```JavaScript
nmap -sV --script=http-enum -oA <scan_file> <IP>
```
  
# Scan Network Range
```JavaScript
nmap <CIDR> -sn -oA <output> | cut -d" " -f5
```
- -sn disabeld port scanning
![image 34.png](assets/Nmap/image%2034.png)
  
Scan UDP ports with -sU
  
# Security Evasion
## Firewalls
firewalls often block SYN scans, but not ACK scans
- -sA flag
## IDS/IPS
much harder because these are passive traffic monitoring systems
- IDS systems examine all connections between hosts
    
    - administrator is notified if the IDS finds packets containing defined contents
    
    - usually there to help admins detect potential attacks, then the admin chooses how to handle it
    
- IPS take measures configured by administrator independently to prevent attacks automatically
    
    - IPS servers are used as a complement to IDS
    
    - one method to see if an IPS system is present is to scan from a single host (VPS)
        
        - if the host is blocked and has no access, we know that the admin took actions
            
            - would then continue the pentest with another host
            
        
    
  
## Decoys
-D flag
- example: -D RND:5 → 5 random IPs
- nmap generates various random IPs inserted into the IP header to disguise origin
- decoy hosts must be alive
![image 1 18.png](assets/Nmap/image%201%2018.png)
  
### Testing Firewall Rule
sometimes all but an entire subnet is blocked
- we can spoof our IP with the -S flag
  
BEFORE
![image 2 19.png](assets/Nmap/image%202%2019.png)
AFTER
![image 3 15.png](assets/Nmap/image%203%2015.png)
  
  
## DNS Proxying
Nmap gives a way to specify DNS servers ourselves
```JavaScript
--dns-server <ns>,<ns>
```
- could be fundamental if we are in the DMZ
    
    - company’s DNS servers are usually more trusted than the internet
    
  
can also use our TCP port 53 (dns) as the source port for the scans
```JavaScript
sudo nmap <IP> -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53
```
- if the admin does not filter IDS/IPS properly, then the packets from port 53 will be trusted and pushed through
  
BEFORE
![image 4 12.png](assets/Nmap/image%204%2012.png)
AFTER
![image 5 11.png](assets/Nmap/image%205%2011.png)
  
### Connect to the Filtered Port
```JavaScript
ncat -nv --source-port <port> <IP> <specified_port>
```
- the specified port is the port we used in the nmap scan (in their example 50000)
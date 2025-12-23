Domain Servers store various records
![image 172.png](/image%20172.png)
![image 1 131.png](/image%201%20131.png)
- IN - Internet
- CH - Chaosnet
- HS - Hesiod
  
# Why DNS Matters for Web Recon
- uncovering assets
    
    - subdomains
    
    - CNAME could point to an old and vulnerable system
    
    - mail servers
    
- mapping network infrastructure
    
    - NS for nameserver, A can show you a load balancer, etc
    
- Monitoring for Changes
    
    - continuously monitoring DNS can reveal changes to infra over time
    
  
# Digging DNS
## DNS Tools
|**Tool**|**Key Features**|**Use Cases**|
|---|---|---|
|`dig`|Versatile DNS lookup tool that supports various query types (A, MX, NS, TXT, etc.) and detailed output.|Manual DNS queries, zone transfers (if allowed), troubleshooting DNS issues, and in-depth analysis of DNS records.|
|`nslookup`|Simpler DNS lookup tool, primarily for A, AAAA, and MX records.|Basic DNS queries, quick checks of domain resolution and mail server records.|
|`host`|Streamlined DNS lookup tool with concise output.|Quick checks of A, AAAA, and MX records.|
|`dnsenum`|Automated DNS enumeration tool, dictionary attacks, brute-forcing, zone transfers (if allowed).|Discovering subdomains and gathering DNS information efficiently.|
|`fierce`|DNS reconnaissance and subdomain enumeration tool with recursive search and wildcard detection.|User-friendly interface for DNS reconnaissance, identifying subdomains and potential targets.|
|`dnsrecon`|Combines multiple DNS reconnaissance techniques and supports various output formats.|Comprehensive DNS enumeration, identifying subdomains, and gathering DNS records for further analysis.|
|`theHarvester`|OSINT tool that gathers information from various sources, including DNS records (email addresses).|Collecting email addresses, employee information, and other data associated with a domain from multiple sources.|
|`Online DNS Lookup Services`|User-friendly interfaces for performing DNS lookups.|Quick and easy DNS lookups, convenient when command-line tools are not available, checking for domain availability or basic information|
  
## dig (Domain Information Groper)
### Common dig Commands
|**Command**|**Description**|
|---|---|
|`dig domain.com`|Performs a default A record lookup for the domain.|
|`dig domain.com A`|Retrieves the IPv4 address (A record) associated with the domain.|
|`dig domain.com AAAA`|Retrieves the IPv6 address (AAAA record) associated with the domain.|
|`dig domain.com MX`|Finds the mail servers (MX records) responsible for the domain.|
|`dig domain.com NS`|Identifies the authoritative name servers for the domain.|
|`dig domain.com TXT`|Retrieves any TXT records associated with the domain.|
|`dig domain.com CNAME`|Retrieves the canonical name (CNAME) record for the domain.|
|`dig domain.com SOA`|Retrieves the start of authority (SOA) record for the domain.|
|`dig @1.1.1.1 domain.com`|Specifies a specific name server to query; in this case 1.1.1.1|
|`dig +trace domain.com`|Shows the full path of DNS resolution.|
|`dig -x 192.168.1.1`|Performs a reverse lookup on the IP address 192.168.1.1 to find the associated host name. You may need to specify a name server.|
|`dig +short domain.com`|Provides a short, concise answer to the query.|
|`dig +noall +answer domain.com`|Displays only the answer section of the query output.|
|`dig domain.com ANY`|Retrieves all available DNS records for the domain (Note: Many DNS servers ignore `ANY` queries to reduce load and prevent abuse, as per [RFC 8482](https://datatracker.ietf.org/doc/html/rfc8482)).|
  
## Groping DNS
```JavaScript
dig <DOMAIN>
```
```JavaScript
Nathan2112@htb[/htb]$ dig google.com
; <<>> DiG 9.18.24-0ubuntu0.22.04.1-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16449
;; flags: qr rd ad; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
;; WARNING: recursion requested but not available
;; QUESTION SECTION:
;google.com.                    IN      A
;; ANSWER SECTION:
google.com.             0       IN      A       142.251.47.142
;; Query time: 0 msec
;; SERVER: 172.23.176.1#53(172.23.176.1) (UDP)
;; WHEN: Thu Jun 13 10:45:58 SAST 2024
;; MSG SIZE  rcvd: 54
```
- Question
    
    - ;google.com IN A means what is the IPv4 (A) address of google.com
    
- Response
    
    - looks very similar, the added ‘0’ just means the TTL of the request
    
  
- if you just want the answer to the question, you can add ‘+short’
```JavaScript
dig +short <DOMAIN>
```
```JavaScript
Nathan2112@htb[/htb]$ dig +short hackthebox.com
104.18.20.126
104.18.21.126
```
  
### Finding Domain from IP
- query for the PTR record of the IP
```JavaScript
dig -x <IP> +short
```
```JavaScript
┌──(kali㉿kali)-[~/Documents/cpts]
└─$ dig -x 134.209.24.248 +short
inlanefreight.com.
```
  
  
# Zone Tranfers
- less invasive and potentially more efficient than brute-forcing
- zone transfers are designed for replicating DNS records between name servers
![image 2 123.png](/image%202%20123.png)
- Zone Transfer (axfr) request (full zone transfer)
- SOA record (start of authority)
    
    - contains vital info about the zone, including its serial number
    
- DNS Records transmission
    
    - transfers to secondary server one by one,
    
- Zone Transfer Complete
    
    - once all records have been transmitted
    
- Acknowledgement (ACK)
  
# Zone Transfer Vulnerability
- allowing anyone to do a zone transfer can give a wealth of info
    
    - subdomains
    
    - ip addresses
    
    - name server records
    
  
## Exploiting Zone Transfers
```JavaScript
dig axfr <DOMAIN> @<IP>
```
- using @ after domain worked for them, but not for me
- @ before IP worked for me, so try both
```Shell
Nathan2112@htb[/htb]$ dig axfr @nsztm1.digi.ninja zonetransfer.me; <<>> DiG 9.18.12-1~bpo11+1-Debian <<>> axfr @nsztm1.digi.ninja zonetransfer.me
; (1 server found)
;; global options: +cmd
zonetransfer.me.	7200	IN	SOA	nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.	300	IN	HINFO	"Casio fx-700G" "Windows XP"
zonetransfer.me.	301	IN	TXT	"google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
zonetransfer.me.	7200	IN	MX	0 ASPMX.L.GOOGLE.COM.
...
zonetransfer.me.	7200	IN	A	5.196.105.14
zonetransfer.me.	7200	IN	NS	nsztm1.digi.ninja.
zonetransfer.me.	7200	IN	NS	nsztm2.digi.ninja.
_acme-challenge.zonetransfer.me. 301 IN	TXT	"6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
_sip._tcp.zonetransfer.me. 14000 IN	SRV	0 0 5060 www.zonetransfer.me.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200	IN PTR www.zonetransfer.me.
asfdbauthdns.zonetransfer.me. 7900 IN	AFSDB	1 asfdbbox.zonetransfer.me.
asfdbbox.zonetransfer.me. 7200	IN	A	127.0.0.1
asfdbvolume.zonetransfer.me. 7800 IN	AFSDB	1 asfdbbox.zonetransfer.me.
canberra-office.zonetransfer.me. 7200 IN A	202.14.81.230
...
;; Query time: 10 msec
;; SERVER: 81.4.108.41#53(nsztm1.digi.ninja) (TCP);; WHEN: Mon May 27 18:31:35 BST 2024
;; XFR size: 50 records (messages 1, bytes 2085)
```
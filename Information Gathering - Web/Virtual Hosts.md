# Virtual Hosts and Subdomains
- key difference between the two is their relationsihp to the DNS and web server’s configuration
  
## Subdomains
- extensions of the main domain name (e.g. blog.example.com)
- typically have their own DNS records
- can be used to organize different sections or services of a website
  
  
## Virtual Hosts
- ability of web servers to distinguish between multiple websites or applications sharing the same IP address
    
    - achieved by using the HTTP Host header
    
- configurations within a web server that allow multiple websites or applications to be hosted on a single server
- associated with TLDs (e.g. example.com) or subdomains (dev.example.com)
    
    - each vhost can have its own separate configuration
    
- VHost fuzzing is used to discover public and non-public subdomains and vhosts by testing various hostnames against a known IP address
- VHosts can also be configured to use different domains
```JavaScript
Code: apacheconf
# Example of name-based virtual host configuration in Apache
<VirtualHost *:80>
    ServerName www.example1.com
    DocumentRoot /var/www/example1
</VirtualHost>
<VirtualHost *:80>
    ServerName www.example2.org
    DocumentRoot /var/www/example2
</VirtualHost>
<VirtualHost *:80>
    ServerName www.another-example.net
    DocumentRoot /var/www/another-example
</VirtualHost>
```
  
### Server VHost Lookup
![image 178.png](../assets/Virtual%20Hosts/image%20178.png)
  
### Types of Virtual Hosting
- Name-Based
    
    - relies solely on the HTTP Host header to distinguish between websites
    
    - most common and flexible
    
    - doesn’t require multiple IPs
    
    - cost effective and easy to set up
    
    - requires the web server to support name-based vhosting
    
    - can have limitations with SSL/TLS
    
- IP-Based
    
    - unique IPs to each website hosted on the server
    
    - doesn’t rely on the Host header
    
    - can be used with any protocol
    
    - offers better isolation between websites
    
- Port-Based
    
    - associated with different ports on the same IP
    
    - used when IP addresses are limited
    
    - not as common or user-friendly as name-based
    
    - might require user to specify the port number in the URL
    
  
## Virtual Host Discovery Tools
|**Tool**|**Description**|**Features**|
|---|---|---|
|[gobuster](https://github.com/OJ/gobuster)|A multi-purpose tool often used for directory/file brute-forcing, but also effective for virtual host discovery.|Fast, supports multiple HTTP methods, can use custom wordlists.|
|[Feroxbuster](https://github.com/epi052/feroxbuster)|Similar to Gobuster, but with a Rust-based implementation, known for its speed and flexibility.|Supports recursion, wildcard discovery, and various filters.|
|[ffuf](https://github.com/ffuf/ffuf)|Another fast web fuzzer that can be used for virtual host discovery by fuzzing the `Host` header.|Customizable wordlist input and filtering options.|
### Gobuster
```JavaScript
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain --domain <DOMAIN>
```
- -t flag increases number of threads
- -k ignores SSL/TLS certificates
- -o to save the output
```JavaScript
Nathan2112@htb[/htb]$ gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:             http://inlanefreight.htb:81
[+] Method:          GET
[+] Threads:         10
[+] Wordlist:        /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
[+] User Agent:      gobuster/3.6
[+] Timeout:         10s
[+] Append Domain:   true
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: forum.inlanefreight.htb:81 Status: 200 [Size: 100]
[...]
Progress: 114441 / 114442 (100.00%)
===============================================================
Finished
===============================================================
```
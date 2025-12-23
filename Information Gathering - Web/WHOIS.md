```JavaScript
whois <DOMAIN>
```
```JavaScript
Nathan2112@htb[/htb]$ whois inlanefreight.com
[...]
Domain Name: inlanefreight.com
Registry Domain ID: 2420436757_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.registrar.amazon
Registrar URL: https://registrar.amazon.com
Updated Date: 2023-07-03T01:11:15Z
Creation Date: 2019-08-05T22:43:09Z
[...]
```
Each WHOIS record typically contains the following information:
- `Domain Name`: The domain name itself (e.g., example.com)
- `Registrar`: The company where the domain was registered (e.g., GoDaddy, Namecheap)
- `Registrant Contact`: The person or organization that registered the domain.
- `Administrative Contact`: The person responsible for managing the domain.
- `Technical Contact`: The person handling technical issues related to the domain.
- `Creation and Expiration Dates`: When the domain was registered and when it's set to expire.
- `Name Servers`: Servers that translate the domain name into an IP address.
  
# Why WHOIS Matters for Web Recon
- tells you who matters
    
    - reveals name, email, phone number
        
        - good for social engineering or phishing campaigns
        
    
- discovering network infrastructure
    
    - name servers, IP addresses
    
- historical data analysis
    
    - WhoisFreaks shows record changes
        
        - changes in ownership, contact info, technical details
        
    
    - tracks evolution of company
    
  
# Utilizing WHOIS
## Phishing Investigation
- running whois on a phishing email tells the following
    
    - registration date - few days ago
    
    - registrant - info is hidden behind a privacy service
    
    - name server - associated with known bulletproof hosting provider known for malicious activity
    
- these details can raise red flags for the analyst
  
## Malware Analysis
- malware communicates with a remote server to receive commands from and exfiltrate data
- whois gives the following details
    
    - registrant - free emails service known for anonymity
    
    - location - country with high cybercrime
    
    - registrar - registrar with a history of lax abuse policies
    
- from this info, you can gather that the C2 is hosted on a compromised or “bulletproof” server, so you can find the hosting provider and let them know
  
## Threat Intelligence Report
- cyber firm tracking activities of threat actor group targetting financial institutions
- whois gives them
    
    - registration dates - registered in clusters, shortly before major attacks
    
    - registrants - various aliases
    
    - name servers - share the same name servers, suggesting common infra
    
    - takedown history - many domains taken down after attacks
    
- allows analyst to create a detailed profile of threat actor’s TTPs and create IOCs based on WHOIS data
  
# Using WHOIS
```JavaScript
whois <DOMAIN>
```
```JavaScript
Nathan2112@htb[/htb]$ whois facebook.com
   Domain Name: FACEBOOK.COM
   Registry Domain ID: 2320948_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.registrarsafe.com
   Registrar URL: http://www.registrarsafe.com
   Updated Date: 2024-04-24T19:06:12Z
   Creation Date: 1997-03-29T05:00:00Z
   Registry Expiry Date: 2033-03-30T04:00:00Z
   Registrar: RegistrarSafe, LLC
   Registrar IANA ID: 3237
   Registrar Abuse Contact Email: abusecomplaints@registrarsafe.com
   Registrar Abuse Contact Phone: +1-650-308-7004
   Domain Status: clientDeleteProhibited https://icann.org/epp\#clientDeleteProhibited
   Domain Status: clientTransferProhibited https://icann.org/epp\#clientTransferProhibited
   Domain Status: clientUpdateProhibited https://icann.org/epp\#clientUpdateProhibited
   Domain Status: serverDeleteProhibited https://icann.org/epp\#serverDeleteProhibited
   Domain Status: serverTransferProhibited https://icann.org/epp\#serverTransferProhibited
   Domain Status: serverUpdateProhibited https://icann.org/epp\#serverUpdateProhibited
   Name Server: A.NS.FACEBOOK.COM
   Name Server: B.NS.FACEBOOK.COM
   Name Server: C.NS.FACEBOOK.COM
   Name Server: D.NS.FACEBOOK.COM
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2024-06-01T11:24:10Z <<<
[...]
Registry Registrant ID:
Registrant Name: Domain Admin
Registrant Organization: Meta Platforms, Inc.
[...]
```
- Domain Registration
    
    - RegistrarSafe, LLC
    
    - Creation Date: 1997-03-29
    
    - Expiry Date: 2033-03-30
    
- Domain Owner
    
    - Registrant/Admin/Tech Organization: Meta Platforms, Inc.
    
    - Registrant/Admin/Tech Contact: Domain Admin
    
- Domain Status
    
    - clientDeleteProhibited, clientTransferProhibited, clientUpdateProhibited…
        
        - domain is protected against unauthorized changes
        
    
- Name Servers
    
    - A.NS.FACEBOOK.COM, B.NS.FACEBOOK.COM…
        
        - all internal, suggesting that Facebook manages its DNS infra
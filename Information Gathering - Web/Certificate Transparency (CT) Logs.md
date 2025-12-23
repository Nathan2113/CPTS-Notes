- public, append-only ledgers that record the assurance of SSL/TLS certificates
- global registry of certificates
  
# CT Logs and Web Recon
- can provide a definitive record of subdomains
- can unveil subdomains with old or expired certificates
    
    - can host outdated and vulnerable software
    
  
## Searching CT Logs
|**Tool**|**Key Features**|**Use Cases**|**Pros**|**Cons**|
|---|---|---|---|---|
|[crt.sh](https://crt.sh/)|User-friendly web interface, simple search by domain, displays certificate details, SAN entries.|Quick and easy searches, identifying subdomains, checking certificate issuance history.|Free, easy to use, no registration required.|Limited filtering and analysis options.|
|[Censys](https://search.censys.io/)|Powerful search engine for internet-connected devices, advanced filtering by domain, IP, certificate attributes.|In-depth analysis of certificates, identifying misconfigurations, finding related certificates and hosts.|Extensive data and filtering options, API access.|Requires registration (free tier available).|
  
### [crt.sh](http://crt.sh) Lookup
```JavaScript
curl -s "https://crt.sh/?q=<DOMAIN>&output=json" | jq -r '.[]
 | select(.name_value | contains("dev")) | .name_value' | sort -u
```
- this one just looks for subdomains with “dev” in it, change it up if needed
```JavaScript
Nathan2112@htb[/htb]$ curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[]
 | select(.name_value | contains("dev")) | .name_value' | sort -u
 
*.dev.facebook.com
*.newdev.facebook.com
*.secure.dev.facebook.com
dev.facebook.com
devvm1958.ftw3.facebook.com
facebook-amex-dev.facebook.com
facebook-amex-sign-enc-dev.facebook.com
newdev.facebook.com
secure.dev.facebook.com
```
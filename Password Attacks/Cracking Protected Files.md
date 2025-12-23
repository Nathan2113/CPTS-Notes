# Hunting for Encrypted Files
```Markdown
for ext in $(echo ".xls .xls* .xltx .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```
```Markdown
Nathan2112@htb[/htb]$ for ext in $(echo ".xls .xls* .xltx .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
File extension:  .xls
File extension:  .xls*
File extension:  .xltx
File extension:  .od*
/home/cry0l1t3/Docs/document-temp.odt
/home/cry0l1t3/Docs/product-improvements.odp
/home/cry0l1t3/Docs/mgmt-spreadsheet.ods
...SNIP...
```
  
# Hunting for SSH Keys
```Markdown
grep -rnE '^\-{5}BEGIN [A-Z0-9]+ PRIVATE KEY\-{5}$' /* 2>/dev/null
```
```Markdown
Nathan2112@htb[/htb]$ grep -rnE '^\-{5}BEGIN [A-Z0-9]+ PRIVATE KEY\-{5}$' /* 2>/dev/null
/home/jsmith/.ssh/id_ed25519:1:-----BEGIN OPENSSH PRIVATE KEY-----
/home/jsmith/.ssh/SSH.private:1:-----BEGIN RSA PRIVATE KEY-----
/home/jsmith/Documents/id_rsa:1:-----BEGIN OPENSSH PRIVATE KEY-----
<SNIP>
```
  
## Checking for Encrypted Keys
- checking if a key requires a passphrase
- you can see if the key is encrypted by trying to read it with ssh-keygen
```Markdown
ssh-keygen -yf ~/.ssh/<file>
# If you are not prompted for the passphrase, you are good to go
Nathan2112@htb[/htb]$ ssh-keygen -yf ~/.ssh/id_ed25519 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIpNefJd834VkD5iq+22Zh59Gzmmtzo6rAffCx2UtaS6
```
  
## Cracking Encrypted SSH Keys
```Markdown
locate *2john*
```
- show all available “2john” scripts
  
```Markdown
ssh2john.py <enc_key> > ssh.hash
john --wordlist=/usr/share/wordlists/rockyou.txt ssh.hash
john ssh.hash --show
```
```Markdown
Nathan2112@htb[/htb]$ ssh2john.py SSH.private > ssh.hash
Nathan2112@htb[/htb]$ john --wordlist=rockyou.txt ssh.hash
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
1234         (SSH.private)
1g 0:00:00:00 DONE (2022-02-08 03:03) 16.66g/s 1747Kp/s 1747Kc/s 1747KC/s Knightsing..Babying
Session completed

Nathan2112@htb[/htb]$ john ssh.hash --show
SSH.private:1234
1 password hash cracked, 0 left
```
  
# Cracking Password Protected Documents
## Office Documents
```Markdown
office2john.py <docx> > <output_hash>
john --wordlist=/usr/share/wordlists/rockyou.txt <output_hash>
john <output_hash> --show
```
```Markdown
Nathan2112@htb[/htb]$ office2john.py Protected.docx > protected-docx.hash
Nathan2112@htb[/htb]$ john --wordlist=rockyou.txt protected-docx.hash
Nathan2112@htb[/htb]$ john protected-docx.hash --show
Protected.docx:1234
1 password hash cracked, 0 left
```
  
## PDFs
```Markdown
pdf2john <pdf> > <output_hash>
john --wordlist=/usr/share/wordlists/rockyou.txt <output_hash>
john <output_hash> --show
```
```Markdown
Nathan2112@htb[/htb]$ pdf2john.py PDF.pdf > pdf.hash
Nathan2112@htb[/htb]$ john --wordlist=rockyou.txt pdf.hash
Nathan2112@htb[/htb]$ john pdf.hash --show
PDF.pdf:1234
1 password hash cracked, 0 left
```
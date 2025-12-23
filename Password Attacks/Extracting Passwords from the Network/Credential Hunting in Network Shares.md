# Common Credential Patterns
## Keywords
- passw
- user
- token
- key
- secret
  
## File Extensions
- .ini
- .cfg
- .env
- .xlsx
- .ps1
- .bat
  
## Files with Interesting Names
- config
- user
- passw
- cred
- initial
  
# Domain Credentials
- if you are looking for creds in INLANEFREIGHT.LOCAL, you can try to find files containing the string “INLANEFREIGHT\”
  
  
# Hunting
- before jumping into hundreds of shares, you can try the following before scaling up to more advanced tools
```Markdown
 Get-ChildItem -Recurse -Include *.ext \\Server\Share | Select-String -Pattern ...
```
  
## From Windows
### Snaffler
- when run on a domain-joined machine, it will automatically scan for interesting files in accessible shares
  
```Markdown
Snaffler.exe -s
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:42Z [Info] Parsing args...
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Parsed args successfully.
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Invoking DFS Discovery because no ComputerTargets or PathTargets were specified
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Getting DFS paths from AD.
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Found 0 DFS Shares in 0 namespaces.
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Invoking full domain computer discovery.
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Getting computers from AD.
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Got 1 computers from AD.
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Starting to look for readable shares...
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Info] Created all sharefinder tasks.
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Black}<\\DC01.inlanefreight.local\ADMIN$>()
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\ADMIN$>(R) Remote Admin
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Black}<\\DC01.inlanefreight.local\C$>()
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\C$>(R) Default share
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\Company>(R)
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\Finance>(R)
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\HR>(R)
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\IT>(R)
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\Marketing>(R)
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\NETLOGON>(R) Logon server share
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\Sales>(R)
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:43Z [Share] {Green}<\\DC01.inlanefreight.local\SYSVOL>(R) Logon server share
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:51Z [File] {Red}<KeepPassOrKeyInCode|R|passw?o?r?d?>\s*[^\s<]+\s*<|2.3kB|2025-05-01 05:22:48Z>(\\DC01.inlanefreight.local\ADMIN$\Panther\unattend.xml) 5"\ language="neutral"\ versionScope="nonSxS"\ xmlns:wcm="http://schemas\.microsoft\.com/WMIConfig/2002/State"\ xmlns:xsi="http://www\.w3\.org/2001/XMLSchema-instance">\n\t\t\ \ <UserAccounts>\n\t\t\ \ \ \ <AdministratorPassword>\*SENSITIVE\*DATA\*DELETED\*</AdministratorPassword>\n\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ </UserAccounts>\n\ \ \ \ \ \ \ \ \ \ \ \ <OOBE>\n\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ <HideEULAPage>true</HideEULAPage>\n\ \ \ \ \ \ \ \ \ \ \ \ </OOBE>\n\ \ \ \ \ \ \ \ </component
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:53Z [File] {Yellow}<KeepDeployImageByExtension|R|^\.wim$|29.2MB|2022-02-25 16:36:53Z>(\\DC01.inlanefreight.local\ADMIN$\Containers\serviced\WindowsDefenderApplicationGuard.wim) .wim
[INLANEFREIGHT\jbader@DC01] 2025-05-01 17:41:58Z [File] {Red}<KeepPassOrKeyInCode|R|passw?o?r?d?>\s*[^\s<]+\s*<|2.3kB|2025-05-01 05:22:48Z>(\\DC01.inlanefreight.local\C$\Windows\Panther\unattend.xml) 5"\ language="neutral"\ versionScope="nonSxS"\ xmlns:wcm="http://schemas\.microsoft\.com/WMIConfig/2002/State"\ xmlns:xsi="http://www\.w3\.org/2001/XMLSchema-instance">\n\t\t\ \ <UserAccounts>\n\t\t\ \ \ \ <AdministratorPassword>\*SENSITIVE\*DATA\*DELETED\*</AdministratorPassword>\n\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ </UserAccounts>\n\ \ \ \ \ \ \ \ \ \ \ \ <OOBE>\n\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ <HideEULAPage>true</HideEULAPage>\n\ \ \ \ \ \ \ \ \ \ \ \ </OOBE>\n\ \ \ \ \ \ \ \ </component
<SNIP>
```
  
- to reduce the amount of output, you can use the following flags
    
    - `-u` retrieves a list of users from AD and searches for references of them in files
    
    - `-i` and `-n` allow you to specify which shares are included in a search
    
  
- clean the output
```Markdown
(Get-Content snaffler.log) -replace '\[[^\]]+\]', '' | Set-Content snaffler_clean.log
```
- look for only info you want
```Markdown
Select-String snaffler_clean.log -Pattern `
"pass|password|pwd|cred|secret|token|key|cpassword" `
-Context 2,2 | Format-List
```
- alternatively, make it a file you can read with notepad
```Markdown
Select-String .\snaffler_clean.log -Pattern 'Keepass|cpassword|password|secret|token|key' |
ForEach-Object {
    $line = $_.Line -replace '\\r\\n',' ' -replace '\s{2,}',' '
    $line = $line.Substring(0, [Math]::Min($line.Length, 400))
    $line
} | Set-Content .\snaffler_hits.txt
```
  
### PowerHuntShares
- PowerShell script that doesn’t necessarily need to be run on a domain-joined machine
- generates an HTML report
```Markdown
Invoke-HuntSMBShares -Threads 100 -OutputDirectory c:\Users\Public
```
```Markdown
PS C:\Users\Public\PowerHuntShares> Invoke-HuntSMBShares -Threads 100 -OutputDirectory c:\Users\Public
 ===============================================================
 INVOKE-HUNTSMBSHARES
 ===============================================================
  This function automates the following tasks:
  o Determine current computer's domain
  o Enumerate domain computers
  o Check if computers respond to ping requests
  o Filter for computers that have TCP 445 open and accessible
  o Enumerate SMB shares
  o Enumerate SMB share permissions
  o Identify shares with potentially excessive privileges
  o Identify shares that provide read or write access
  o Identify shares thare are high risk
  o Identify common share owners, names, & directory listings
  o Generate last written & last accessed timelines
  o Generate html summary report and detailed csv files
  Note: This can take hours to run in large environments.
 ---------------------------------------------------------------
 |||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
 ---------------------------------------------------------------
 SHARE DISCOVERY
 ---------------------------------------------------------------
 [*][05/01/2025 12:51] Scan Start
 [*][05/01/2025 12:51] Output Directory: c:\Users\Public\SmbShareHunt-05012025125123
 [*][05/01/2025 12:51] Successful connection to domain controller: DC01.inlanefreight.local
 [*][05/01/2025 12:51] Performing LDAP query for computers associated with the inlanefreight.local domain
 [*][05/01/2025 12:51] -  computers found
 [*][05/01/2025 12:51] - 0 subnets found
 [*][05/01/2025 12:51] Pinging  computers
 [*][05/01/2025 12:51] -  computers responded to ping requests.
 [*][05/01/2025 12:51] Checking if TCP Port 445 is open on  computers
 [*][05/01/2025 12:51] - 1 computers have TCP port 445 open.
 [*][05/01/2025 12:51] Getting a list of SMB shares from 1 computers
 [*][05/01/2025 12:51] - 11 SMB shares were found.
 [*][05/01/2025 12:51] Getting share permissions from 11 SMB shares
<SNIP>
```
![image 353.png](../../assets/Credential%20Hunting%20in%20Network%20Shares/image%20353.png)
  
## From Linux
### MANSPIDER
- don’t have access to domain-joined machine or would rather search for files remotely
- best to run with the Docker container to avoid dependency issues
  
- basic scan looking for string “passw”
```Markdown
docker run --rm -v ./manspider:/root/.manspider blacklanternsecurity/manspider <IP> -c 'passw' -u '<user>' -p '<pass>'
```
  
### NetExec
- can be used to search through network shares with `--spider`
  
- basic scan of network shares for files containing “passwd”
```Markdown
nxc smb <IP> -u <user> -p '<pass>' --spider <share> --content --pattern "passw"
```
- add `-o DOWNLOAD_FLAG=true` to download all smb share files for offline enumeration
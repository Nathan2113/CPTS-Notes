# Windows Defender
can use Get-MpComputerStatus in powershell to get current defender status
- look for RealTiemProtectionEnabled
![[../assets/Enumerating Security Controls/image 199.png|image 199.png]]
  
# App Locker
Microsoft’s whitelisting solution
- companies often block PowerShell.exe, but forget about PowerShell executable locations
```JavaScript
%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
```
  
to get AppLocker information, we can use powershell
```JavaScript
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```
![[../assets/Enumerating Security Controls/image 1 147.png|image 1 147.png]]
  
# PowerShell Constrained Language Mode
locks down features to use PowerShell effectively
- block COM objects
- only allow approved .NET types, XAML-based workflows, PowerShell classes, etc
```JavaScript
$ExecutionContext.SessionState.LanguageMode
```
![[../assets/Enumerating Security Controls/image 2 131.png|image 2 131.png]]
  
  
# LAPS
can se what machines do not have LAPS installed
- LAPSToolkit
  
an account that has added a computer to a domain has All Extended Rights over that host
- gives the ability to read passwords
```JavaScript
Find-LAPSDelegatedGroups
```
![[../assets/Enumerating Security Controls/image 3 117.png|image 3 117.png]]
  
## Using Find-AdmPwdExtendedRights
```JavaScript
Find-AdmPwdExtendedRights
```
![[../assets/Enumerating Security Controls/image 4 106.png|image 4 106.png]]
- any user with All Extended Rights can read
  
## Using Get-LAPSComputers
search for computers that have LAPS enabled and when passwords expire
- even gives the cleartext passwords if our user has access
```JavaScript
Get-LAPSComputers
```
![[../assets/Enumerating Security Controls/image 5 103.png|image 5 103.png]]
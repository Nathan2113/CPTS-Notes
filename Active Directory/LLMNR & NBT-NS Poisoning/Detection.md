Not always possible to disabled LLMNR/NBT-NS
- one option is to create honeypots
- inject LLMNR and NBT-NS requests for non-existent hosts and alerting if any of the responses receive answers
[https://www.praetorian.com/blog/a-simple-and-effective-way-to-detect-broadcast-name-resolution-poisoning-bnrp](https://www.praetorian.com/blog/a-simple-and-effective-way-to-detect-broadcast-name-resolution-poisoning-bnrp)
  
Hosts can be monitored on port UDP 5355 and 137
  
SIEM alert IDs
- Event ID 4697 - A service was installed in the system
- Event ID 7045 - **A new service was installed in the system**
  
Monitor registry key for changes in EnableMulticast DWORD value
- 0 means that LLMNR is disabled
```JavaScript
HKLM\Software\Policies\Microsoft\Windows NT\DNSClient
```
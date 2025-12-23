Users with the “Exchange Windows Permissions” can write a DACL to the domain object
- can be leveraged to give a user DC Sync privileges
  
Another powerful Exchange group is called “Organization Management” (basically domain admins)
- full control over the OU called “Microsoft Exchange Security Groups”, which contains “Exchange Windows Permissions”
  
viewing organization management’s permissions
![image 369.png](../../assets/Exchange%20Related%20Group%20Membership/image%20369.png)
  
Dumping credentials from memory in and Exchange server could produce many cleartext passwords or NTLM hashes
- due to users logging into Outlook Web Access (OWA) and Exchange caching their credentials in memory
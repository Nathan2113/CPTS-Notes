NFS - Network File Share
  
same purpose as SMB
- used between Linux and Unix system, meaning it cannot communicate directly with SMB servers
  
NFSv3 authenticates with the client computer
NFSv4 the user must authenticate (just like SMB)
  
|**Version**|**Features**|
|---|---|
|`NFSv2`|It is older but is supported by many systems and was initially operated entirely over UDP.|
|`NFSv3`|It has more features, including variable file size and better error reporting, but is not fully compatible with NFSv2 clients.|
|`NFSv4`|It includes Kerberos, works through firewalls and on the Internet, no longer requires portmappers, supports ACLs, applies state-based operations, and provides performance improvements and high security. It is also the first version to have a stateful protocol.|
NFS is based on the Open Network Computing Remote Procedure Call (ONC-RPC/SUN-RPC) exposed on TCP and UDP ports 111
- uses External Data Representation (XDR) for system-independent exchange of data
- NFS has no mechanism for authentcation or authorization
    
    - authentication is completely shifed to the RPC protocol’s options
    
    - authorization is derived from the available file system information
    
- most common authentication is via UNIX UID/GID and group memberships
  
# Default Configuration
not difficult to configure as there are not as many options as FTP or SMB
- /etc/exports file contains a table of physical filesystems accessible to users over NFS
  
## Exports File
```JavaScript
Nathan2112@htb[/htb]$ cat /etc/exports 
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
```
- default exports file has examples of configuring NFS shares
|**Option**|**Description**|
|---|---|
|`rw`|Read and write permissions.|
|`ro`|Read only permissions.|
|`sync`|Synchronous data transfer. (A bit slower)|
|`async`|Asynchronous data transfer. (A bit faster)|
|`secure`|Ports above 1024 will not be used.|
|`insecure`|Ports above 1024 will be used.|
|`no_subtree_check`|This option disables the checking of subdirectory trees.|
|`root_squash`|Assigns all permissions to files of root UID/GID 0 to the UID/GID of anonymous, which prevents `root` from accessing files on an NFS mount.|
### ExportFS
```JavaScript
root@nfs:~# echo '/mnt/nfs  10.129.14.0/24(sync,no_subtree_check)' >> /etc/exports
root@nfs:~# systemctl restart nfs-kernel-server 
root@nfs:~# exportfs
/mnt/nfs      	10.129.14.0/24
```
- this example shares the folder /mnt/nfs to the subnet 10.129.14.0/24
  
  
# Dangerous Settings
|**Option**|**Description**|
|---|---|
|`rw`|Read and write permissions.|
|`insecure`|Ports above 1024 will be used.|
|`nohide`|If another file system was mounted below an exported directory, this directory is exported by its own exports entry.|
|`no_root_squash`|All files created by root are kept with the UID/GID 0.|
- ports above 1024 are dangerous as ports below 1024 can only be used by root
  
# Footprinting NFS
ports 111 and 2049 are essential
```JavaScript
sudo nmap <IP> -p111,2049 -sV -sC
```
![image 171.png](../assets/NFS/image%20171.png)
  
## NSE Scripts
```JavaScript
sudo nmap --script nfs* <IP> -sV -p111,2049
```
![image 1 130.png](../assets/NFS/image%201%20130.png)
- shows contents of shares and their stats
  
## Show Available NFS Shares
```JavaScript
showmount -e <IP>
```
![image 2 122.png](../assets/NFS/image%202%20122.png)
  
## Mounting NFS Share
```JavaScript
mkdir target-NFS
sudo mount -t nfs <IP>:/ ./target-NFS/ -o nolock
cd target-NFS
tree .
```
![image 3 111.png](../assets/NFS/image%203%20111.png)
  
## List Contents with Usernames & Group Names
```JavaScript
ls -l mnt/nfs/
```
![image 4 102.png](../assets/NFS/image%204%20102.png)
  
## List Contents with UIDs & GUIDs
```JavaScript
ls -n mnt/nfs/
```
![image 5 99.png](../assets/NFS/image%205%2099.png)
- if the root_squash option was set, we couldn’t edit [backup.sh](http://backup.sh) even if we are root
  
## Unmounting
```JavaScript
sudo umount ./target-NFS
```
  
  
# Attacks
- some nfs shares don’t allow direct access, but here is one way to bypass that worked in CPTS
- got the info i needed from the nmap scan (scan above)
```JavaScript
sudo mount -t nfs -o vers=3,nolock,lookupcache=none <IP>:/<share> /mnt/<share>
sudo ls -lah /mnt/<share>
sudo cat /mnt/<share>
```
```JavaScript
sudo mount -t nfs -o vers=3,nolock,lookupcache=none 10.129.202.41:/TechSupport /mnt/TechSupport
┌──(kali㉿kali)-[/mnt]                                                                      
└─$ sudo ls -lah /mnt/TechSupport                                                           
total 72K                                                                                   
drwx------ 2 4294967294 4294967294  64K Dec  6 23:48 .                                      
drwxr-xr-x 3 root       root       4.0K Dec  7 01:04 ..                                     
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283649.txt                
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283650.txt                
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283651.txt                
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283652.txt                
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283653.txt                
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283654.txt                
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283655.txt                
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283656.txt                
-rwx------ 1 4294967294 4294967294    0 Nov 10  2021 ticket4238791283657.txt
```
```JavaScript
┌──(kali㉿kali)-[/mnt]
└─$ sudo cat /mnt/TechSupport/ticket4238791283782.txt
Conversation with InlaneFreight Ltd
Started on November 10, 2021 at 01:27 PM London time GMT (GMT+0200)
---
01:27 PM | Operator: Hello,. 
  
So what brings you here today?
01:27 PM | alex: hello
01:27 PM | Operator: Hey alex!
01:27 PM | Operator: What do you need help with?
01:36 PM | alex: I run into an issue with the web config file on the system for the smtp ser
ver. do you mind to take a look at the config? 
01:38 PM | Operator: Of course
01:42 PM | alex: here it is:
 1smtp {
 2    host=smtp.web.dev.inlanefreight.htb
 3    \#port=25
 4    ssl=true
 5    user="alex"
 6    password="lol123!mD"
 7    from="alex.g@web.dev.inlanefreight.htb"
 8}
```
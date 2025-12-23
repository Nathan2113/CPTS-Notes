![image 75.png](assets/Active/image%2075.png)
  
enum4linux doesn’t give anything good
  
Domain: active.htb
![image 1 42.png](assets/Active/image%201%2042.png)
  
anonymous SMB on Replication
![image 2 42.png](assets/Active/image%202%2042.png)
  
Groups.xml in the Replication share policies
![image 3 36.png](assets/Active/image%203%2036.png)
  
Groups.xml has a username and password
![image 4 32.png](assets/Active/image%204%2032.png)
User: SVC_TGS (pog)
Pass: edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ: GPPstillStandingStrong2k18
  
gpp-decrypt cracks the above hash
![image 5 31.png](assets/Active/image%205%2031.png)
  
SVC_TGS can access some new shares
![image 6 27.png](assets/Active/image%206%2027.png)
  
can log into Users share with SVC_TGS and get the user flag
![image 7 26.png](assets/Active/image%207%2026.png)
  
requesting UserSPNs gives us an admin hash
![image 8 23.png](assets/Active/image%208%2023.png)
  
running hashcat on the SPN hash
![image 9 23.png](assets/Active/image%209%2023.png)
![image 10 18.png](assets/Active/image%2010%2018.png)
User: Administrator
Pass: Ticketmaster1968
  
ez money
![image 11 17.png](assets/Active/image%2011%2017.png)
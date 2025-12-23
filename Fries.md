Starting creds:
d.cooper@fries.htb
D4LE11maan!!
  
![image 134.png](assets/Fries/image%20134.png)
  
no smb or rpc
![image 1 100.png](assets/Fries/image%201%20100.png)
![image 2 99.png](assets/Fries/image%202%2099.png)
  
  
80 is a resturant website
![image 3 92.png](assets/Fries/image%203%2092.png)
![image 4 87.png](assets/Fries/image%204%2087.png)
  
443 is a password manager
![image 5 85.png](assets/Fries/image%205%2085.png)
![image 6 81.png](assets/Fries/image%206%2081.png)
![image 7 78.png](assets/Fries/image%207%2078.png)
- can update the configuration without authenticating to LDAP first
  
fuzzing is throttled hard
![image 8 74.png](assets/Fries/image%208%2074.png)
  
tried to log into pwm with his creds and said directory wasn’t available
![image 9 71.png](assets/Fries/image%209%2071.png)
- says the certificate doesn’t match the configuration trust store so it’s probably just not a valid account in there
  
was able to go to configuration just like the first warning said
![image 10 61.png](assets/Fries/image%2010%2061.png)
  
different error this time, the password given just doesn’t work
![image 11 57.png](assets/Fries/image%2011%2057.png)
  
  
nothing on directory fuzz
![image 12 48.png](assets/Fries/image%2012%2048.png)
  
no subdomains
![image 13 48.png](assets/Fries/image%2013%2048.png)
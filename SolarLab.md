![image 69.png](assets/SolarLab/image%2069.png)
nothing good from fuzzing port 80
![image 1 36.png](assets/SolarLab/image%201%2036.png)
  
now looking for subdomains
- didnt get anything from that either
  
Checking smb share anonymously
![image 2 36.png](assets/SolarLab/image%202%2036.png)
excel spreadsheet found in the smb share (details-file.xlsx)
![image 3 31.png](assets/SolarLab/image%203%2031.png)
  
going to port 6791 takes us to report.solarlab.htb
![image 4 27.png](assets/SolarLab/image%204%2027.png)
- trying blake.byte gave us “user not found”
![image 5 26.png](assets/SolarLab/image%205%2026.png)
- doing “AlexanderK” and “ClaudiaS” gives a different error
![image 6 22.png](assets/SolarLab/image%206%2022.png)
- trying a cluster bomb attack with these two users
![image 7 21.png](assets/SolarLab/image%207%2021.png)
nothing good here
  
There are 3 members found on their website, but only two have this naming scheme
- going to try BlakeB with the passwords instead
![image 8 18.png](assets/SolarLab/image%208%2018.png)
request 28 gives us a redirect
user: BlakeB
pass: ThisCanB3typedeasily1@
![image 9 18.png](assets/SolarLab/image%209%2018.png)
- those creds worked
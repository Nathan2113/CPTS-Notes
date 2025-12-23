![image 68.png](assets/Cap/image%2068.png)
  
the website has a security snapshot that takes you to a new URL that holds other pcaps
![image 1 35.png](assets/Cap/image%201%2035.png)
  
more fuzzing gives a bunch of ids for pcap files
![image 2 35.png](assets/Cap/image%202%2035.png)
  
looking at 10.10.10.245/data/0 gives us the correct pcap file with a username and password
![image 3 30.png](assets/Cap/image%203%2030.png)
![image 4 26.png](assets/Cap/image%204%2026.png)
user: nathan
pass: Buck3tH4TF0RM3!
  
ssh with those credentials to get user flag
![image 5 25.png](assets/Cap/image%205%2025.png)
  
![image 6 21.png](assets/Cap/image%206%2021.png)
linpeas shows this as critical
  
![image 7 20.png](assets/Cap/image%207%2020.png)
ez money
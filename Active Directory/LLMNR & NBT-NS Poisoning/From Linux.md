Starting responder with default settings
- for LLMNR poisoning, turn rogue servers (SMB, HTTP) off
```JavaScript
sudo responder -I <interface>
```
  
Running responder with settings from PJPT
```JavaScript
sudo responder -I <interface> -dP
```
![image 354.png](../../assets/From%20Linux/image%20354.png)
![image 1 262.png](../../assets/From%20Linux/image%201%20262.png)
  
hashcat mode for NTLMv2 is -m 5600
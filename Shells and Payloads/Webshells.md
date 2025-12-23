# Laudanum
[https://github.com/jbarcia/Web-Shells/tree/master/laudanum](https://github.com/jbarcia/Web-Shells/tree/master/laudanum)
- repository of ready-made files
- includes injectable files for many different webapp languages
    
    - asp
    
    - aspx
    
    - jsp
    
    - php
    
    - and more
    
  
# Antak
https://github.com/samratashok/nishang
- offensive PowerShell toolset
- located in `/usr/share/nishang/Antak-WebShell`
  
## Antak Demonstration
```Markdown
cp /usr/share/nishang/Antak-WebShell/antak.aspx /home/kali/Upload.aspx
```
- make sure to change the username and password for the shell so that other people can’t use it
![image 189.png](../assets/Webshells/image%20189.png)
- once uploaded, it will ask for the creds once you navigate to it
![image 1 140.png](../assets/Webshells/image%201%20140.png)
- the session acts just like PowerShell
![image 2 128.png](../assets/Webshells/image%202%20128.png)
  
  
# PHP
https://github.com/WhiteWinterWolf/wwwolf-php-webshell
## Bypassing File Type Restriction
- the example they use changes the Content-Type header
![image 3 115.png](../assets/Webshells/image%203%20115.png)
- change Content-Type from application/x-php to image/gif
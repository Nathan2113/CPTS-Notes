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
![[../assets/Webshells/image 189.png|image 189.png]]
- once uploaded, it will ask for the creds once you navigate to it
![[../assets/Webshells/image 1 140.png|image 1 140.png]]
- the session acts just like PowerShell
![[../assets/Webshells/image 2 128.png|image 2 128.png]]
  
  
# PHP
https://github.com/WhiteWinterWolf/wwwolf-php-webshell
## Bypassing File Type Restriction
- the example they use changes the Content-Type header
![[../assets/Webshells/image 3 115.png|image 3 115.png]]
- change Content-Type from application/x-php to image/gif
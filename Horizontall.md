![image 120.png](assets/Horizontall/image%20120.png)
  
![image 1 86.png](assets/Horizontall/image%201%2086.png)
  
![image 2 85.png](assets/Horizontall/image%202%2085.png)
  
  
  
![image 3 78.png](assets/Horizontall/image%203%2078.png)
  
![image 4 74.png](assets/Horizontall/image%204%2074.png)
js file reveals subdomain
  
![image 5 73.png](assets/Horizontall/image%205%2073.png)
  
![image 6 69.png](assets/Horizontall/image%206%2069.png)
  
looking up unauthenticated exploits give us CVE-2019-19609
https://github.com/glowbase/CVE-2019-19609
![image 7 66.png](assets/Horizontall/image%207%2066.png)
  
![image 8 62.png](assets/Horizontall/image%208%2062.png)
linpeas shows that the path may be vulnerable
- /opt/strapi/myapi/node_modules/.bin:/opt/strapi/myapi/node_modules/.bin:/opt/strapi
![image 9 59.png](assets/Horizontall/image%209%2059.png)
  
![image 10 50.png](assets/Horizontall/image%2010%2050.png)
- found the strapi database file with developer creds
  
developer:\#J!:F9Zt2u
- this is for the mysql instance
  
![image 11 47.png](assets/Horizontall/image%2011%2047.png)
  
![image 12 40.png](assets/Horizontall/image%2012%2040.png)
![image 13 40.png](assets/Horizontall/image%2013%2040.png)
- thats crazy
  
![image 14 37.png](assets/Horizontall/image%2014%2037.png)
- also has port 1337 and 8000 open
  
![image 15 34.png](assets/Horizontall/image%2015%2034.png)
admin:Password123 worked on strapi
  
![image 16 30.png](assets/Horizontall/image%2016%2030.png)
bottom-left of dash has the version number
- Strapi v3.0.0-beta.17.4
  
![image 17 24.png](assets/Horizontall/image%2017%2024.png)
can upload files, but the link redirects to potr 1337
  
![image 18 22.png](assets/Horizontall/image%2018%2022.png)
  
  
![image 19 20.png](assets/Horizontall/image%2019%2020.png)
  
- starting connection with ligolo
![image 20 17.png](assets/Horizontall/image%2020%2017.png)
  
![image 21 17.png](assets/Horizontall/image%2021%2017.png)
  
![image 22 14.png](assets/Horizontall/image%2022%2014.png)
ligolo broke the machine, so i guess im using chisel
  
![image 23 12.png](assets/Horizontall/image%2023%2012.png)
![image 24 11.png](assets/Horizontall/image%2024%2011.png)
  
configure a foxyproxy proxy with the following settings
![image 25 7.png](assets/Horizontall/image%2025%207.png)
- make sure its turned on
  
port 1337 works now
![image 26 5.png](assets/Horizontall/image%2026%205.png)
  
![image 27 5.png](assets/Horizontall/image%2027%205.png)
port 8000 has laravel
  
pivoting with chisel wasn’t working correctly, so i put my ssh key into the machine and port forwarded from there
- now we can run the exploit on our machine and it’ll work since we can contact github
  
exploits debug mode to run commands as root
https://github.com/nth347/CVE-2021-3129_exploit
![image 28 5.png](assets/Horizontall/image%2028%205.png)
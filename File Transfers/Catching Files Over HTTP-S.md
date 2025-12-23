- most common to transfer files over HTTP/S because that is most commonly what’s allowed through firewalls
  
# Nginx - Enabling PUT
## Create a Directory to Handle Uploaded Files
```Markdown
sudo mkdir -p /var/www/uploads/SecretUploadDirectory
```
## Change Owner to www-data
```Markdown
sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```
## Create Nginx Configuration File
```Markdown
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```
## Symlink our Site to the sites-enabled Directory
```Markdown
sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
```
## Start Nginx
```Markdown
sudo systemctl restart nginx.service
```
## Verifying Errors
```Markdown
tail -2 /var/log/nginx/error.log
# Example
Nathan2112@htb[/htb]$ tail -2 /var/log/nginx/error.log
2020/11/17 16:11:56 [emerg] 5679#5679: bind() to 0.0.0.0:`80` failed (98: A`ddress already in use`)
2020/11/17 16:11:56 [emerg] 5679#5679: still could not bind()
```
- if something is already listening on port 80 (nginx’s default), run the following
```Markdown
# Remove NginxDefault Configuration
sudo rm /etc/nginx/sites-enabled/default
# Test uploads with cURL
curl -T /etc/passwd http://localhost:9001/SecretUploadDirectory/users.txt
sudo tail -1 /var/www/uploads/SecretUploadDirectory/users.txt
```
- by default, Nginx does not display files in the absence of an index.html like apache does
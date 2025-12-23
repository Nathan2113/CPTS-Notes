# Popular Web Crawlers
## Burp Suite Spider
- map out webapps
- identify hidden content
- uncover potential vulnerabilities
  
## OWASP ZAP (Zed Attack Proxy)
- free and open source
- webapp security scanner
- automated and manual modes
- spider component to crawl webapps and identify potential vulnerabilities
  
## Scrapy (Python Framework)
- extracts structured data from websites
- handles complex crawling scenarios
- automated data processing
  
## Apache Nutch (Scalable Crawler)
- highly extensible and scalable open-source web crawler written in Java
- handles massive crawls across the entire web or focus on specific domains
- requires more technial expertise to set up and configure
    
    - however, its power and flexibility make it a valuable asset for large-scale recon projects
    
  
# Scrapy
```JavaScript
python3 ReconSpider.py http://<domain>
```
## Install
```JavaScript
pip3 install scrapy
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
unzip ReconSpider.zip 
```
  
## results.json
- after running reconspider, the data will be saved in a json file
```JavaScript
{
    "emails": [
        "lily.floid@inlanefreight.com",
        "cvs@inlanefreight.com",
        ...
    ],
    "links": [
        "https://www.themeansar.com",
        "https://www.inlanefreight.com/index.php/offices/",
        ...
    ],
    "external_files": [
        "https://www.inlanefreight.com/wp-content/uploads/2020/09/goals.pdf",
        ...
    ],
    "js_files": [
        "https://www.inlanefreight.com/wp-includes/js/jquery/jquery-migrate.min.js?ver=3.3.2",
        ...
    ],
    "form_fields": [],
    "images": [
        "https://www.inlanefreight.com/wp-content/uploads/2021/03/AboutUs_01-1024x810.png",
        ...
    ],
    "videos": [],
    "audio": [],
    "comments": [
        "<!-- \#masthead -->",
        ...
    ]
}
```
![[../assets/Creepy Crawlies/image 181.png|image 181.png]]
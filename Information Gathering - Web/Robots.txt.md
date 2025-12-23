- etiquette guide for bots
- outlines which areas of a website they are allowed to access and which are off-limits
  
- simple text file places in root directory of a website
- adheres to the Robots Exclusion Standard
  
# How robots.txt Works
```JavaScript
User-agent: *
Disallow: /private/
```
- tells user agents (* is a wildcard) that they are not allowed to access any URLs that start with /private/
  
## Understanding robots.txt Structure
- each record consists fo two main components
    
    - User-agent
        
        - specifies which crawler or bot the following rules to apply to
        
        - wildcard (*) indicates that the rules apply to all bots
        
        - specific user-agents can be targeted, such as Googlebot or Bingbot
        
    
    - Directives
        
        - provide specific instructions to the identified user-agent
        
    
    ![image 180.png](../assets/Robots.txt/image%20180.png)
    
  
# Why Respect robots.txt?
- bots don’t HAVE to follow it
- most legitimate web crawlers will respect it for these reasons:
    
    - avoiding overburdening servers
    
    - protecting sensitive information
    
    - legal and ethical compliance
    
  
# robots.txt in Web Reconnaissance
- uncovering hidden directories
- mapping website structure
- detecting crawler traps
    
    - some websites may intentionally include honeypot directories to lure malicious bots
    
    - identifying traps can provide insights into the target’s security awareness
    
  
# Analyzing robots.txt
Example robots.txt file
```JavaScript
User-agent: *
Disallow: /admin/
Disallow: /private/
Allow: /public/
User-agent: Googlebot
Crawl-delay: 10
Sitemap: https://www.example.com/sitemap.xml
```
- all user agents are disallowed from accessing /admin/ and /private/
- all user agents are allowed to access /public/
- Googlebot is specifically instructed to wait 10 seconds between requests
- the sitemap is provided for easier crawling and indexing
- from this output, we can tell the website has a /admin/ and /private/ directory
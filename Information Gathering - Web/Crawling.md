- automated process of systematically browsing the web
  
# How Web Crawlers Work
- starts with seed (homepage)
- fetches this page, parses its content, and extracts all its links
- adds links to the queue and crawls them, repeating the process iteratively
![image 179.png](../assets/Crawling/image%20179.png)
  
# Breadth-First Crawling
![image 1 136.png](../assets/Crawling/image%201%20136.png)
- explores web’s depth before going deep
  
# Depth-First Crawling
![image 2 126.png](../assets/Crawling/image%202%20126.png)
- prioritizes depth over breadth
  
# Extracting Valuable Information
- links (internal or external)
    
    - connecting pages within the site (internal)
    
    - connecting links to other websites (external)
    
- comments
    
    - users inadvertantly reveal sensitive deails, internal processes, or hints of vulnerabilities
    
- metadata
    
    - data about data
    
    - page titles, descriptions, keywords, author names, dates, etc
    
    - context about a page’s content, purpose, and relevance to recon goals
    
- sensitive files
    
    - backup files (.bak, .old)
    
    - configuration files (web.config, settings.php)
    
    - log files (error_log, access_log)
    
    - API keys
    
    - database credentials
    
    - encryption keys
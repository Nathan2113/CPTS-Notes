- automated process of systematically browsing the web
  
# How Web Crawlers Work
- starts with seed (homepage)
- fetches this page, parses its content, and extracts all its links
- adds links to the queue and crawls them, repeating the process iteratively
![[../assets/Crawling/image 179.png|image 179.png]]
  
# Breadth-First Crawling
![[../assets/Crawling/image 1 136.png|image 1 136.png]]
- explores web’s depth before going deep
  
# Depth-First Crawling
![[../assets/Crawling/image 2 126.png|image 2 126.png]]
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
![](assets/Help/Untitled.png)
  
on port 3000, graphql is being used
- you can tell when navigating to /graphql and the webpage says “GET query missing”
![](assets/Help/Untitled%201.png)
hacktricks has a default query to enumerate for information
- ?query={__schema{types{name,fields{name,args{name,description,type{name,kind,ofType{name, kind}}}}}}}
![](assets/Help/Untitled%202.png)
the table “User” has fields “username” and “password”, so we query User for username and password using
- ?query={user{username,password}}
![](assets/Help/Untitled%203.png)
password is md5 encoded
![](assets/Help/Untitled%204.png)
  
searchsploit for helpdeskz (the application being used) shows us an authenticated SQL Injection
![](assets/Help/Untitled%205.png)
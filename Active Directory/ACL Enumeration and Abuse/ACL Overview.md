ACLs are defined as:
- who has access to which asset/resource
- the level of access they are provisioned
  
The settings themselves in an ACL are called Access Control Entries (ACEs)
- each ACE maps back to a user, group, or process
- defines the rights granted on that principal
  
Every object has an ACL, but can have multiple ACEs
- multiple security principals can access objects in AD
  
Two types of ACLs
- Discretionary Access Control List (DACL)
    
    - defines which security principals are granted or denied access to an object
    
    - made up of ACEs that allow or deny access
    
    - if DACL does not exist → all who attempt to access the object are given full rights
    
    - if DACL does exist, but does not have any ACE entries → system will deny access to all
    
- System Access Control Lists (SACL)
    
    - allow admins to log access attempts made to secured objects
    
![image 362.png](../../assets/ACL%20Overview/image%20362.png)
  
SACLs can be seen within the auditing tab
![image 1 270.png](../../assets/ACL%20Overview/image%201%20270.png)
  
# Access Control Entries (ACEs)
Three types of ACEs
![image 2 229.png](../../assets/ACL%20Overview/image%202%20229.png)
  
Each ACE is made up of four components:
- SID of user/group that has access
- flag denoting the type of ACE (access denied, allowed, or system audit ACE)
- set of flags specifying whether or not child containers/objects can inherit the given ACE from the parent
- access mask which is a 32-bit value that defines the rights granted to object
![image 3 198.png](../../assets/ACL%20Overview/image%203%20198.png)
  
# Graph of ACE Attacks
![image 4 174.png](../../assets/ACL%20Overview/image%204%20174.png)
  
# Common Attack Scenarios
![image 5 163.png](../../assets/ACL%20Overview/image%205%20163.png)
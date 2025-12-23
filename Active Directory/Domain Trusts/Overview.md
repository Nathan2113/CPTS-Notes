Trusts are used to establish a forest-forest or domain-domain (intra-domain)
- allows users to access resources or perform administrative tasks in another domain
- can allow for one-way or two-way (bidirectional) communication
  
Organizations can create various types of trusts
- Parent-child
    
    - Two or more domains within the same forest. The child domain has a two-way transitive trust with the parent domain, meaning that users in the child domain corp.inlanefreight.local could authenticate into the parent domain inlanefreight.local, and vice-versa.
    
- Cross-link
    
    - A trust between child domains to speed up authentication.
    
- External
    
    - A non-transitive trust between two separate domains in separate forests which are not already joined by a forest trust. This type of trust utilizes SID filtering or filters out authentication requests (by SID) not from the trusted domain.
    
- Tree-root
    
    - A two-way transitive trust between a forest root domain and a new tree root domain. They are created by design when you set up a new tree root domain within a forest.
    
- Forest
    
    - A transitive trust between two forest root domains.
    
- ESAE
    
    - A bastion forest used to manage Active Directory.
    
  
Trusts can be transitive and non-transitive
- transitive - trust is extended to objects that the child domain trusts
    
    - Domain A has a trust with Domain B, and Domain B has a transitive trust with Domain C, meaning Domain A will automatically trust Domain A
    
- non-transitive - child domain itself is the only trusted domain
![[/image 362.png|image 362.png]]
  
Trust Table
![[/image 1 270.png|image 1 270.png]]
  
Trust Directions
- one-way - users in a trusted domain can access resources in a trusting domain, not vice-versa
- bidirectional - users form both trusting and trusted can access each other’s resources
  
Domain trusts are often set up incorrectly and provide attack paths
  
Graphical representation of trust types
![[/image 2 229.png|image 2 229.png]]
  
# SidHistory
- used in migration scenarios
    
    - if a user is migrated to another domain, a new account is created in the second domain
    
    - the original SID will be added to the user’s SID history
    
    - attackers can use mimikatz to perform SID history injection and add an admininstrator account to the SID history attirbute of an account they control
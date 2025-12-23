Many apps and printers store LDAP creds in their web admin console connected to the domain
- often left with weak or default passwords
  
one exploit to try to the “test connection” button, which attempts a connection to the Domain Controller
- we can change the LDAP address to our IP and set up a netcat listener on port 389
  
may need a full LDAP server to pull off the attack
[https://grimhacker.com/2018/03/09/just-a-printer](https://grimhacker.com/2018/03/09/just-a-printer)
results from a flaw in the Exchange server called “PushSubscription”
- allows any domain user with a mailbox to force the Exchange server to authenticate to any host provided by the client over HTTP
    
    - Exchange service run sas SYSTEM and is over-privileged by default (WriteDacl permissions on the domain pre-2019 update)
    
  
used to relay LDAP and dump the domain NTDS database
- if you cannot relay LDAP, you can relay and authenticate to other hosts in the domain
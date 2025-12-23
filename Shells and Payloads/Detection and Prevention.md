# Monitoring
## Events to Watch For
- file uploads - especially with Web Applications
- Suspicious non-admin user actions
    
    - bash commands
    
    - cmd commands
    
    - PowerShell
    
    - whoami
    
- Anomalous Network Sessions
    
    - heartbeat on a nonstandard port (like 4444)
        
        - default port used my meterpreter
        
    
    - remote login attempts using GET / POST requests
    
  
## Establish Network Visibility
- visual diagrams for your network
    
    - netbrain, draw.io
    
- suspicious traffic in cleartext
    
    - can follow the stream in wireshark and it can show you what they’re doing
    
![[../assets/Detection and Prevention/image 190.png|image 190.png]]
  
## Protecting End Devices
- Windows Defender
- patch management strategy
- only make exceptions for approved applications based on a change management process
  
  
# Potential Mitigations
- application sandboxing
- least privilege permission policies
- host segmentation and hardening
- physical and application layer firewalls
![[../../assets/Windows Authentication Process/image 348.png|image 348.png]]
- Local interactive logon is handled through WinLogon and LogonUI
- authentication packages are DLLs responsbile for performing auth checks
    
    - for non-domain-joined logins, Windows uses Msv1_0.dll
    
- WinLogon is the system process responsible for managing security-related user interactions
    
    - launching LogonUI
    
    - handling password changes
    
    - locking and unlocking the workstation
    
- to obtain the user’s creds, WinLogon relies on credential providers, which are COM objects implemented as DLLs
- WinLogon is the only process that intercepts login requests from the keyboard, which are sent via RPC messages from Win32k.sys
    
    - WinLogon passes them mto the Local Security Authority (LSA) to authenticate the user
    
  
# LSASS
- Local Security Authority Subsystem Service
- governs all authentication processes
- located at `%SystemRoot%\System32\Lsass.exe`
  
|**Authentication Packages**|**Description**|
|---|---|
|`Lsasrv.dll`|The LSA Server service both enforces security policies and acts as the security package manager for the LSA. The LSA contains the Negotiate function, which selects either the NTLM or Kerberos protocol after determining which protocol is to be successful.|
|`Msv1_0.dll`|Authentication package for local machine logons that don't require custom authentication.|
|`Samsrv.dll`|The Security Accounts Manager (SAM) stores local security accounts, enforces locally stored policies, and supports APIs.|
|`Kerberos.dll`|Security package loaded by the LSA for Kerberos-based authentication on a machine.|
|`Netlogon.dll`|Network-based logon service.|
|`Ntdsa.dll`|This library is used to create new records and folders in the Windows registry.|
  
# SAM Database
- Security Account Manager
- stores user account credentials as LM or NTLM hashes
    
    - located in `%SystemRoot%\system32\config\SAM`
    
- mounted under HKLM\SAM
- viewing requires SYSTEM level privileges
  
- if a system is assigned to a workgroup, it handles SAM locally
- if it’s domain-joined, the DC must validate credentials from the NTDS.dit
    
    - stored in `%SystemRoot%\ntds.dit`
    
    - protected by the SYSKEY (syskey.exe)
        
        - partially encrypts the SAM file on disk
        
    
  
# Credential Manager
- built-in feature of all Windows operating systems that allows users to store and manage creds
- saved credentials are stored in the `Credential Locker`
stored in `%SystemRoot%\[Username]\AppData\Local\Microsoft\[Vault/Credentials]\`
  
# NTDS
- synchronized across all Domain Controllers, with the exception of Read-Only Domain Controllers (RODCs)
- database that stored AD data
    
    - user accounts
    
    - group accounts
    
    - computer accounts
    
    - group policy objects
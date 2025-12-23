# Using Sessions
- background a session with `[CTRL] + [Z]` or typing ‘background’
  
## Listing Active Sessions
- `sessions`
```Markdown
msf6 exploit(windows/smb/psexec_psh) > sessions
Active sessions
===============
  Id  Name  Type                     Information                 Connection
  --  ----  ----                     -----------                 ----------
  1         meterpreter x86/windows  NT AUTHORITY\SYSTEM @ MS01  10.10.10.129:443 -> 10.10.10.205:50501 (10.10.10.205)
```
  
## Interacting with a Session
- `sessions -i <number>`
```Markdown
msf6 exploit(windows/smb/psexec_psh) > sessions -i 1
[*] Starting interaction with 1...
meterpreter > 
```
- good to use different sessions for post exploitation activities
    
    - i.e. first session got a reverse shell into a system, second session is exploit suggestor
    
  
# Jobs
- if we need to use a port thats already in use for another exploit, we can’t simply `[CTRL] + [C]` because the port will stay open
    
    - this is where `jobs` comes in
    
- look at currently active tasks running in the background and terminate old ones to free the port
- other types of tasks inside sessions can also be converted into jobs to run in the background, even if the session dies or disappears
  
## Jobs Command Help Menu
```Markdown
msf6 exploit(multi/handler) > jobs -h
Usage: jobs [options]
Active job manipulation and interaction.
OPTIONS:
    -K        Terminate all running jobs.
    -P        Persist all running jobs on restart.
    -S <opt>  Row search filter.
    -h        Help banner.
    -i <opt>  Lists detailed information about a running job.
    -k <opt>  Terminate jobs by job ID and/or range.
    -l        List all running jobs.
    -p <opt>  Add persistence to job by job ID
    -v        Print more detailed info.  Use with -i and -l
```
  
## Exploit Command Help Menu
- when running an exploit, you can run the exploit as a job using `exploit -j`
```Markdown
msf6 exploit(multi/handler) > exploit -h
Usage: exploit [options]
Launches an exploitation attempt.
OPTIONS:
    -J        Force running in the foreground, even if passive.
    -e <opt>  The payload encoder to use.  If none is specified, ENCODER is used.
    -f        Force the exploit to run regardless of the value of MinimumRank.
    -h        Help banner.
    -j        Run in the context of a job.
	
<SNIP
```
  
## Running an Exploit as a Background Job
```Markdown
msf6 exploit(multi/handler) > exploit -j
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.
[*] Started reverse TCP handler on 10.10.14.34:4444
```
  
## Listing Running Jobs
- to see all running jobs, use `jobs -l`
- to kill a job, use `kill [index no.]`
- kill all running jobs with `jobs -K`
```Markdown
msf6 exploit(multi/handler) > jobs -l
Jobs
====
 Id  Name                    Payload                    Payload opts
 --  ----                    -------                    ------------
 0   Exploit: multi/handler  generic/shell_reverse_tcp  tcp://10.10.14.34:4444
```
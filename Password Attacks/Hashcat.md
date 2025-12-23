```Markdown
hashcat -a 0 -m <mode> <hashes> [wordlist, rule, mask, ...]
```
  
# Hashid
```Markdown
hashid -m <hash>
```
```Markdown
Nathan2112@htb[/htb]$ hashid -m '$1$FNr44XZC$wQxY6HHLrgrGX0e1195k.1'
Analyzing '$1$FNr44XZC$wQxY6HHLrgrGX0e1195k.1'
[+] MD5 Crypt [Hashcat Mode: 500]
[+] Cisco-IOS(MD5) [Hashcat Mode: 500]
[+] FreeBSD MD5 [Hashcat Mode: 500]
```
  
# Attack Modes
## Dictionary Attack
- `-a 0` for dictionary
  
### Rules
- can add rules to hashcat to try more passwords
```Markdown
Nathan2112@htb[/htb]$ ls -l /usr/share/hashcat/rules
total 2852
-rw-r--r-- 1 root root 309439 Apr 24  2024 Incisive-leetspeak.rule
-rw-r--r-- 1 root root  35802 Apr 24  2024 InsidePro-HashManager.rule
-rw-r--r-- 1 root root  20580 Apr 24  2024 InsidePro-PasswordsPro.rule
-rw-r--r-- 1 root root  64068 Apr 24  2024 T0XlC-insert_00-99_1950-2050_toprules_0_F.rule
-rw-r--r-- 1 root root   2027 Apr 24  2024 T0XlC-insert_space_and_special_0_F.rule
-rw-r--r-- 1 root root  34437 Apr 24  2024 T0XlC-insert_top_100_passwords_1_G.rule
-rw-r--r-- 1 root root  34813 Apr 24  2024 T0XlC.rule
-rw-r--r-- 1 root root   1289 Apr 24  2024 T0XlC_3_rule.rule
-rw-r--r-- 1 root root 168700 Apr 24  2024 T0XlC_insert_HTML_entities_0_Z.rule
-rw-r--r-- 1 root root 197418 Apr 24  2024 T0XlCv2.rule
-rw-r--r-- 1 root root    933 Apr 24  2024 best64.rule
-rw-r--r-- 1 root root    754 Apr 24  2024 combinator.rule
-rw-r--r-- 1 root root 200739 Apr 24  2024 d3ad0ne.rule
-rw-r--r-- 1 root root 788063 Apr 24  2024 dive.rule
-rw-r--r-- 1 root root  78068 Apr 24  2024 generated.rule
-rw-r--r-- 1 root root 483425 Apr 24  2024 generated2.rule
drwxr-xr-x 2 root root   4096 Oct 19 15:30 hybrid
-rw-r--r-- 1 root root    298 Apr 24  2024 leetspeak.rule
-rw-r--r-- 1 root root   1280 Apr 24  2024 oscommerce.rule
-rw-r--r-- 1 root root 301161 Apr 24  2024 rockyou-30000.rule
-rw-r--r-- 1 root root   1563 Apr 24  2024 specific.rule
-rw-r--r-- 1 root root     45 Apr 24  2024 toggles1.rule
-rw-r--r-- 1 root root    570 Apr 24  2024 toggles2.rule
-rw-r--r-- 1 root root   3755 Apr 24  2024 toggles3.rule
-rw-r--r-- 1 root root  16040 Apr 24  2024 toggles4.rule
-rw-r--r-- 1 root root  49073 Apr 24  2024 toggles5.rule
-rw-r--r-- 1 root root  55346 Apr 24  2024 unix-ninja-leetspeak.rule
```
- rules are found in /usr/share/hashcat/rules
- for example, best64.rule tries adding the 64 most common modifications to a password (i.e. appending numbers, or substituting for leet requirements)
    
    - best64 is the most commonly used rule list
    
- can be used with `-r <ruleset>`
```Markdown
Nathan2112@htb[/htb]$ hashcat -a 0 -m 0 1b0556a75770563578569ae21392630c /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
...SNIP...
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 1b0556a75770563578569ae21392630c
Time.Started.....: Sat Apr 19 09:16:35 2025 (0 secs)
Time.Estimated...: Sat Apr 19 09:16:35 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Mod........: Rules (/usr/share/hashcat/rules/best64.rule)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........: 13624.4 kH/s (5.40ms) @ Accel:512 Loops:77 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 236544/1104517645 (0.02%)
Rejected.........: 0/236544 (0.00%)
Restore.Point....: 2048/14344385 (0.01%)
Restore.Sub.#1...: Salt:0 Amplifier:0-77 Iteration:0-77
Candidate.Engine.: Device Generator
Candidates.#1....: slimshady -> drousd
Hardware.Mon.#1..: Util: 47%
Started: Sat Apr 19 09:16:35 2025
Stopped: Sat Apr 19 09:16:37 2025
```
  
## Mask Attack
- `-a 3`
- brute-force attack where the keyspace is defined by user
  
### Character Sets
|**Symbol**|**Charset**|
|---|---|
|?l|abcdefghijklmnopqrstuvwxyz|
|?u|ABCDEFGHIJKLMNOPQRSTUVWXYZ|
|?d|0123456789|
|?h|0123456789abcdef|
|?H|0123456789ABCDEF|
|?s|«space»!"#$%&'()*+,-./:;<=>?@[]^_`{|
|?a|?l?u?d?s|
|?b|0x00 - 0xff|
- l = lowercase
- u = uppercase
- d = decimal
- h = hex
- H = uppercase hex
- s = special
- a = all
- b = hex format (0x00, 0xff)
  
- custom character sets can be deined with -1, -2, -3, -4 arguments, then referred to with ?1, ?2, ?3, ?4
  
**Example**
- want to try passwords which start with an uppercase letter, continue with 4 lowercase, a digit, then a symbol
    
    - resulting mask would be ?u?l?l?l?l?d?s
    
  
```Markdown
Nathan2112@htb[/htb]$ hashcat -a 3 -m 0 1e293d6912d074c0fd15844d803400dd '?u?l?l?l?l?d?s'
...SNIP...
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 1e293d6912d074c0fd15844d803400dd
Time.Started.....: Sat Apr 19 09:43:02 2025 (4 secs)
Time.Estimated...: Sat Apr 19 09:43:06 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Mask.......: ?u?l?l?l?l?d?s [7]
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   101.6 MH/s (9.29ms) @ Accel:512 Loops:1024 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 456237056/3920854080 (11.64%)
Rejected.........: 0/456237056 (0.00%)
Restore.Point....: 25600/223080 (11.48%)
Restore.Sub.#1...: Salt:0 Amplifier:5120-6144 Iteration:0-1024
Candidate.Engine.: Device Generator
Candidates.#1....: Uayvf7- -> Dikqn5!
Hardware.Mon.#1..: Util: 98%
Started: Sat Apr 19 09:42:46 2025
Stopped: Sat Apr 19 09:43:08 2025
```
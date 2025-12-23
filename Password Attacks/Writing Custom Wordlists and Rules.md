# Common Syntax
|**Description**|**Password Syntax**|
|---|---|
|First letter is uppercase|`Password`|
|Adding numbers|`Password123`|
|Adding year|`Password2022`|
|Adding month|`Password02`|
|Last character is an exclamation mark|`Password2022!`|
|Adding special characters|`P@ssw0rd2022!`|
  
# Hashcat Custom Rules Functions
|**Function**|**Description**|
|---|---|
|`:`|Do nothing|
|`l`|Lowercase all letters|
|`u`|Uppercase all letters|
|`c`|Capitalize the first letter and lowercase others|
|`sXY`|Replace all instances of X with Y|
|`$!`|Add the exclamation character at the end|
```Markdown
Nathan2112@htb[/htb]$ cat custom.rule
:
c
so0
c so0
sa@
c sa@
c sa@ so0
$!
$! c
$! so0
$! sa@
$! c so0
$! c sa@
$! so0 sa@
$! c so0 sa@
```
  
- can use the following command to apply the rules in a custom rule list against a wordlist and store the results in a file
```Markdown
hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```
```Markdown
Nathan2112@htb[/htb]$ cat mut_password.list
password
Password
passw0rd
Passw0rd
p@ssword
P@ssword
P@ssw0rd
password!
Password!
passw0rd!
p@ssword!
Passw0rd!
P@ssword!
p@ssw0rd!
P@ssw0rd!
```
  
# Generating Wordlists Using CeWL
- scans potential words from a company’s website and saves them in a separate list
```Markdown
cewl https://<url> -d <depth> -m <min_word_length> --lowercase -w <wordlist_output>
wc -l <wordlist_output>
```
```Markdown
97268a8ae45ac7d15c3cea4ce6ea550b
```
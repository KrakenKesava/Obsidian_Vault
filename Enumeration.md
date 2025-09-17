
	1. information Gathering
	2. Enumeration 
	3. Exploitation(initial access)
	4. Post-Exploitation
	5. Reporting

```
$ nmap -Pn -sS -sC -sV -O <ip_add> -oX NameofXML
```

This NameofXML is used to import to Metasploit.

```
$ service postgresql start

$ msfconsole 


Msf5> db_status

Msf5> workspace

Workspace -a win2k12

Workspace

Msf5> db_import <path of NameofXML nmap>

MsF5> hosts

Msf5> services

Msf5> workspace -a nmap_msf

Msf5> workspace

Msf5> db_nmap -Pn -sS -sV -O <ip-add> these scans will automatically be saved in metasploit framework

Msf5> vulns
```
<h2>FTP Enumeration</h2>
<i>Standard Port Number: 21</i>

```
ftp target.ine.local -p 21 

use auxiliary/scanner/ftp/ftp_version

use auxiliary/scanner/ftp/ftp_login
```
```
$ msfconsole

Mf5> workspace -a ftp_enum

Mf5> search type:auxiliary name:ftp

Mf5> use auxiliary/scanner/ftp/ftp_login

Mf5>  set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt

Mf5> set USER_FILE /usr/share/metasploit-framework/data/wordlists/unix_password.txt
```
There are different modules such as finding version and brute forcing .

SMB , Samba Enumeration

Standard port number 139 and 445

In windows SMB , in linux Samba

They can interact with each other .

Windows smb uses port 445 and its NetBios at 139

To set global variable

```
Mf5> setg RHOSTS ipadd
```

Same as FTP

```
MF5> use auxiliary/scanner/smb/smb_enumusers

Mf5> use auxiliary/scanner/smb/smb_enumshares

Mf5> use auxiliary/scanner/smb/smb_login
```

```
 smbclient -L //192.168.0.3 -U admin
```
Enter password

This will give you the list of shares that admin possesses.

Take a share name ex xyp

```
 smbclient //target.ine.local/print$ -U josh
```
Enter password

And you are in

# Connect to a specific share (anonymous login)

```
smbclient -L //target.ine.local -N

rpcclient -U "" -N demo.ine.local
```
Tools:

.nmblookup

.smbclient

.rpcclient

To find Netbios name use

Nmblookup -A ipadd

Nmblookup -B ipadd '*'

#!/bin/bash

# Define the target and wordlist location

TARGET="target.ine.local"

WORDLIST="/root/Desktop/wordlists/shares.txt"

# Check if the wordlist file exists

```
if [ ! -f "$WORDLIST" ]; then

 echo "Wordlist not found: $WORDLIST"

 exit 1

fi
```
# Loop through each share in the wordlist

```
while read -r SHARE; do
 echo "Testing share: $SHARE"
 smbclient //$TARGET/$SHARE -N -c "ls" &>/dev/null
 if [ $? -eq 0 ]; then
 echo "[+] Anonymous access allowed for: $SHARE"
 else
 echo "[-] Access denied for: $SHARE"
 fi
done < "$WORDLIST"
```
<h2>Web Server Enumeration</h2>
<i>Standard port number HTTP 80 and HTTPS 443</i>

Metasploit modules

```
auxiliary/scanner/http/http_version

auxiliary/scanner/http/http_header

auxiliary/scanner/http/robots_txt

auxiliary/scanner/http/dir_scanner

auxiliary/scanner/http/files_dir

auxiliary/scanner/http/http_login

auxiliary/scanner/http/apache_userdir_enum

 auxiliary/scanner/http/apache_userdir_enum
 auxiliary/scanner/http/brute_dirs
 auxiliary/scanner/http/dir_scanner
 auxiliary/scanner/http/dir_listing
 auxiliary/scanner/http/http_put
 auxiliary/scanner/http/files_dir
 auxiliary/scanner/http/http_login
 auxiliary/scanner/http/http_header
 auxiliary/scanner/http/http_version
 auxiliary/scanner/http/robots_txt
```

<h2>MYSQL Enumeration</h2>
<i>Standard port number 3306</i>
```
 auxiliary/scanner/mysql/mysql_version
 auxiliary/scanner/mysql/mysql_login
```
This will do brute force attack

```
 auxiliary/admin/mysql/mysql_enum
```
From the previous stage we enter username and password and this is enumerate and give information

```
 auxiliary/admin/mysql/mysql_sql
```
Execute the sql queries

```
 auxiliary/scanner/mysql/mysql_file_enum
 auxiliary/scanner/mysql/mysql_hashdump
 auxiliary/scanner/mysql/mysql_schemadump
 auxiliary/scanner/mysql/mysql_writable_dirs
```
<h2>SSH Enumeration</h2>

<i>Standard port number 22</i>

```
auxiliary/scanner/ssh/ssh_version
auxiliary/scanner/ssh/ssh_enumers
auxiliary/scanner/ssh/ssh_login
```
The following username and password dictionary will be useful: - /usr/share/metasploit-framework/data/wordlists/common_users.txt - /usr/share/metasploit-framework/data/wordlists/common_passwords.txt

<h2>SMTP Enumeration</h2>

<i>Standard port number 25 but when SSL/TLS configured 465 and 587.</i>

```
auxiliary/scanner/smtp/smtp_version

auxiliary/scanner/smtp/smtp_enum
```
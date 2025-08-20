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




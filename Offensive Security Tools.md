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





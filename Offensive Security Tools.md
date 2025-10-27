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



# Gobuster: The Basics








  

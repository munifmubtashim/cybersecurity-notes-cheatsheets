# Introduction
The OWASP Top 10 vulnerabilities. Each section explains how the flaw works, why it occurs, and how to exploit and mitigate it through practical challenges.
Topics include Broken Access Control, Cryptographic Failures, Injection, Insecure Design, Security Misconfiguration, Outdated Components, Authentication Failures,
Integrity Failures, Logging & Monitoring Failures, and SSRF.

# Broken Access Control

Websites have pages that are protected from regular visitors. For example, only the site's admin user should be able to access a page to manage other users. If a website visitor can access protected pages they are not meant to see, then the access controls are broken.

A regular visitor being able to access protected pages can lead to the following:

- Being able to view sensitive information from other users
- Accessing unauthorized functionality
  
Simply put, broken access control allows attackers to bypass authorisation, allowing them to view sensitive data or perform tasks they aren't supposed to.

For example, a vulnerability was found in 2019, where an attacker could get any single frame from a Youtube video marked as private. The researcher who found the vulnerability showed that he could ask for several frames and somewhat reconstruct the video. Since the expectation from a user when marking a video as private would be that nobody had access to it, this was indeed accepted as a broken access control vulnerability.

# Broken Access Control (IDOR Challenge)

IDOR or Insecure Direct Object Reference refers to an access control vulnerability where you can access resources you wouldn't ordinarily be able to see. This occurs when the programmer exposes a Direct Object Reference, which is just an identifier that refers to specific objects within the server. By object, we could mean a file, a user, a bank account in a banking application, or anything really.

For example, let's say we're logging into our bank account, and after correctly authenticating ourselves, we get taken to a URL like this https://bank.thm/account?id=111111. On that page, we can see all our important bank details, and a user would do whatever they need to do and move along their way, thinking nothing is wrong.

There is, however, a potentially huge problem here, anyone may be able to change the id parameter to something else like 222222, and if the site is incorrectly configured, then he would have access to someone else's bank information.

The application exposes a direct object reference through the id parameter in the URL, which points to specific accounts. Since the application isn't checking if the logged-in user owns the referenced account, an attacker can get sensitive information from other users because of the IDOR vulnerability. Notice that direct object references aren't the problem, but rather that the application doesn't validate if the logged-in user should have access to the requested account

Deploy the machine and go to http://10.201.36.158 - Login with the username noot and the password test1234.

Look at other users' notes. What is the flag? flag{fivefourthree}

# Cryptographic Failures

Cryptographic failures happen when applications misuse or skip proper encryption, causing sensitive data to be exposed—either while it’s being sent over the network (data in transit) or when it’s stored on servers (data at rest). For example, a webmail service must encrypt browser-server traffic so eavesdroppers can’t read messages, and ideally encrypt stored emails so even the provider can’t read them; if encryption is weak or absent an attacker (or a man-in-the-middle) can intercept or access usernames, passwords, financial data, DOBs, etc. In short: always use strong, well-tested cryptography correctly to protect confidentiality and prevent simple or advanced attacks.
 
 ### Metarial 01
 The most common (and simplest) format of a flat-file database is an SQLite database. These can be interacted with in most programming languages and have a dedicated client for querying them on the command line. This client is called **sqlite3** and is installed on many Linux distributions by default.

 To access it, we use 
 ```sql
sqlite3 <database-name>
```
From here, we can see the tables in the database by using the **.tables** command:
```bash
┌──(codex㉿kali)-[~]
└─$ sqlite3 database.db
SQLite version 3.46.1 2024-08-13 09:16:08
Enter ".help" for usage hints.
sqlite> .tables
sqlite> CREATE TABLE USERS ( id INTEGER PRIMARY KEY , name TEXT , email TEXT );
sqlite> .tables
USERS

```

At this point, we can dump all the data from the table, but we won't necessarily know what each column means unless we look at the table information. First, let's use **PRAGMA table_info(USERS);** to see the table information. Then we'll use **SELECT * FROM USERS;** to dump the information from the table:

```bash
┌──(codex㉿kali)-[~]
└─$ sqlite3 database.db
SQLite version 3.46.1 2024-08-13 09:16:08
Enter ".help" for usage hints.
sqlite> .tables
USERS
sqlite> PRAGMA table_info(USERS);
0|id|INTEGER|0||1
1|name|TEXT|0||0
2|email|TEXT|0||0
sqlite> SELECT * FROM USERS;
1|Codex|codex@gmail.com
sqlite> 
```
### Material 02

When it comes to hash cracking, Kali comes pre-installed with various tools. If you know how to use these, then feel free to do so; however, they are outwith the scope of this material.

Instead, we will be using the online tool: https://crackstation.net/ This website is extremely good at cracking weak password hashes. For more complicated hashes, we would need more sophisticated tools; however, all of the crackable password hashes used in today's challenge are weak MD5 hashes, which Crackstation should handle very nicely.

**Q&A**

What is the name of the mentioned directory?/assets

Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data?webapp.db


Use the supporting material to access the sensitive data. What is the password hash of the admin user?6eea9b7ef19179a06954edd0f6c05ceb

What is the admin's plaintext password?qwertyuiop

Log in as the admin. What is the flag THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}






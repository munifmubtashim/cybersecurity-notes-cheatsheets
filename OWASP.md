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

## Injection

Definition: Occur when user input is interpreted as commands or queries by an application.

Examples:

- SQL Injection: Injecting SQL code into queries → access, modify, delete database data.

- Command Injection: Injecting system commands → execute arbitrary commands on server.

Prevention:

- Allow list validation: Accept only predefined safe inputs.

- Input sanitization: Remove or escape dangerous characters.

- Use secure libraries/frameworks: Prevent direct execution of user input.

## Command Injection


Occurs when server-side code (e.g., PHP, Python, Node) passes user-controlled input into a function that runs shell/OS commands. An attacker can then craft input that the application executes on the server, effectively running arbitrary OS commands.

**Why it’s dangerous:**

- Attacker can list/read/write files, escalate privileges, pivot to other systems, exfiltrate data, or install
 malware — basically full control comparable to a shell on the server.

**Code Example**

Let's consider a scenario: MooCorp has started developing a web-based application for cow ASCII art with customisable text. While searching for ways to implement their app, they've come across the cowsay command in Linux, which does just that! Instead of coding a whole web application and the logic required to make cows talk in ASCII, they decide to write some simple code that calls the cowsay command from the operating system's console and sends back its contents to the website.

Let's look at the code they used for their app.  See if you can determine why their implementation is vulnerable to command injection.  We'll go over it below.

```php
<?php
    if (isset($_GET["mooing"])) {
        $mooing = $_GET["mooing"];
        $cow = 'default';

        if(isset($_GET["cow"]))
            $cow = $_GET["cow"];
        
        passthru("perl /usr/bin/cowsay -f $cow $mooing");
    }
?>
```
In simple terms, the above snippet does the following:

- Checking if the parameter "mooing" is set. If it is, the variable **$mooing** gets what was passed into the input field.
- Checking if the parameter "cow" is set. If it is, the variable **$cow** gets what was passed through the parameter.
- The program then executes the function **passthru("perl /usr/bin/cowsay -f $cow $mooing");**. The passthru function simply executes a command in the operating system's console and sends the output back to the user's browser. You can see that our command is formed by concatenating the $cow and $mooing variables at the end of it. Since we can manipulate those variables, we can try injecting additional commands by using simple tricks. If you want to, you can read the docs on passthru() on PHP's website for more information on the function itself.

**Exploiting Command Injection**

Now that we know how the application works behind the curtains, we will take advantage of a bash feature called "inline commands" to abuse the cowsay server and execute any arbitrary command we want. Bash allows you to run commands within commands. This is useful for many reasons, but in our case, it will be used to inject a command within the cowsay server to get it executed.

To execute inline commands, you only need to enclose them in the following format `$(your_command_here)`. If the console detects an inline command, it will execute it first and then use the result as the parameter for the outer command. Look at the following example, which runs `whoami` as an inline command inside an `echo` command:

**Q&A**
What strange text file is in the website's root directory?drpepper.txt


How many non-root/non-service/non-daemon users are there?0


What user is this app running as?apache


What is the user's shell set as?/sbin/nologin

What version of Alpine Linux is running?3.16.0
  

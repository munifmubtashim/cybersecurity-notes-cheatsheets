# Web Application Basics

## Web Application Overview
onsider a **web application as a planet**. Astronauts travel to the planet to explore its surface, similar to how someone uses a web browser to explore a web application. Although we only see the surface, a lot is happening beneath it. The planet represents the web server, and the surface represents what the user interacts with — the web pages or apps.  

---

## Front End

The **Front End** is like the surface of the planet — what an astronaut (user) can see and interact with. It uses technologies like **HTML, CSS, and JavaScript**.

### HTML (Hypertext Markup Language)
- HTML is the foundational code that tells a browser **what to display and how**.
- Analogy: Simple organisms on the planet, with **DNA** instructing their structure.



---

### CSS (Cascading Style Sheets)
- CSS defines **visual appearance**: colors, text styles, layout.
- Analogy: DNA parts that define **color, shape, size, and texture** of organisms.



---

### JavaScript (JS)
- JS enables **complex behavior** and interactivity in the browser.
- Analogy: The **brain of an advanced organism**, making decisions based on interactions.



---

## Back End

The **Back End** consists of components you **don’t see in the browser**, but are essential for functionality.

### Database
- Stores, modifies, and retrieves information.
- Analogy: Inhabitants storing maps, diaries, books, and files — the planet’s ecosystem of resources.



---

### Infrastructure
- Includes **web servers, application servers, networking, and storage**.
- Analogy: Roads, vehicles, and fuel that enable the planet’s activity.


---

### Web Application Firewall (WAF)
- An optional component that **filters dangerous requests** and protects the application.
- Analogy: The planet’s atmosphere protecting inhabitants from harmful UV rays.


---

### Summary

| Component | Web Analogy | Function |
|-----------|------------|---------|
| HTML      | Basic organisms | Structure of pages |
| CSS       | DNA color/texture | Visual styling |
| JavaScript| Brain of organism | Interactivity |
| Database  | Ecosystem/library | Data storage & retrieval |
| Infrastructure | Roads & vehicles | Support and connectivity |
| WAF       | Atmosphere | Security & filtering |

### Q&A
Which component on a computer is responsible for hosting and delivering content for web applications? web server


Which tool is used to access and interact with web applications? web browser

Which component acts as a protective layer, filtering incoming traffic to block malicious attacks, and ensuring the security of the the web application? web application firewall

## Uniform Resource Locator

A **Uniform Resource Locator (URL)** is a web address that allows you to access online content—webpages, videos, images, or other media. It guides your browser to the correct location on the Internet.

---

### Anatomy of a URL

A URL is made up of several components, each serving a different purpose. Understanding these parts is important for browsing, web development, and cybersecurity.
Example URL: https://username.example.com:443/path/to/page?search=query#section


---

### 1. Scheme
- The **protocol** used to access the website.
- Common examples: `HTTP` and `HTTPS`.  
- **HTTPS** is preferred as it **encrypts the connection** for security.

---

### 2. User
- Some URLs include login details (usually a username).  
- Rare today because **putting credentials in URLs is unsafe**.  
- Can expose sensitive information if intercepted.

---

### 3. Host / Domain
- The **main address of the website**.  
- Each domain must be unique and registered.  
- Security tip: Watch out for **typosquatting**, where fake domains mimic real ones to trick users.

---

### 4. Port
- Directs the browser to the **specific service** on the web server.  
- Range: 1–65,535  
- Common ports:  
  - `80` → HTTP  
  - `443` → HTTPS  

---

### 5. Path
- Points to the **specific file or page** on the server.  
- Think of it as a **roadmap** to the content.  
- Important to secure paths to prevent unauthorized access.

---

### 6. Query String
- Starts with `?` and often carries search terms or form inputs.  
- Users can modify it, so it should be **validated and sanitized**.  
- Vulnerable to injection attacks if not handled properly.

---

### 7. Fragment
- Starts with `#` and points to a **specific section** of a webpage.  
- Example: jumping to a heading or table.  
- Like query strings, fragments should be handled securely to prevent attacks.


Q&A
Which protocol provides encrypted communication to ensure secure data transmission between a web browser and a web server?HTTPS

What term describes the practice of registering domain names that are misspelt variations of popular websites to exploit user errors and potentially engage in fraudulent activities?Typosquatting


What part of a URL is used to pass additional information, such as search terms or form inputs, to the web server?Query String

# HTTP Messages

HTTP messages are **packets of data exchanged** between a client (user) and a web server. They are crucial for understanding how web applications communicate because they show how requests and responses are structured.

---

## Types of HTTP Messages

1. **HTTP Requests**  
   - Sent by the client to trigger actions on the web application.

2. **HTTP Responses**  
   - Sent by the server in response to the client’s request.

---

## Anatomy of an HTTP Message

### 1. Start Line
- The **introduction** of the message.  
- Indicates whether the message is a **request** or **response**.  
- Provides important information about how the message should be processed.

### 2. Headers
- Composed of **key-value pairs** providing extra information.  
- Examples:  
  - Content type  
  - Authentication details  
  - Security instructions  

### 3. Empty Line
- Separates **headers** from the **body**.  
- Essential for correct message parsing.

### 4. Body
- Contains the **actual data**.  
- **In requests:** user-submitted data (e.g., form input)  
- **In responses:** server content (e.g., webpage, JSON data)

---

## Importance of Understanding HTTP Messages

- **Communication:** Forms the foundation of client-server interaction.  
- **Troubleshooting:** Helps diagnose web communication issues.  
- **Security:** Ensures proper handling of data and protects against attacks.  

#  HTTP Requests – Cybersecurity Notes

An **HTTP request** is what a client (user) sends to a web server to interact with a web application. Since this is often the **first point of contact** between the user and the server, understanding it is crucial for both **development** and **cybersecurity**.

---

##  Request Line (Start Line)

Format:

METHOD /path HTTP/version

markdown
Copy code

- **Method** → What action to perform (`GET`, `POST`, etc.)
- **Path** → The resource being requested (`/login`, `/api/users/123`)
- **Version** → HTTP protocol version (`HTTP/1.1`, `HTTP/2`)

---

##  Common HTTP Methods & Security Notes

| Method   | Purpose | Security Considerations |
|----------|---------|--------------------------|
| **GET** | Fetches data | Never include sensitive info in URLs (tokens/passwords). |
| **POST** | Sends data (create/update) | Validate & sanitize input to prevent **SQLi/XSS**. |
| **PUT** | Replace/update resource | Require **authorization** checks. |
| **DELETE** | Remove resource | Require **authorization** checks. |
| **PATCH** | Partially update | Validate to avoid inconsistent states. |
| **HEAD** | Same as GET but only headers | Useful for metadata checks. |
| **OPTIONS** | Shows available methods | Disable if not needed (information leak risk). |
| **TRACE** | Debugging | Often disabled for security reasons. |
| **CONNECT** | Establishes secure tunnel (e.g., HTTPS) | Critical for encrypted communication. |

---

##  URL Path

The **URL path** tells the server which resource to fetch.  
Example:  

https://tryhackme.com/api/users/123



- Path = `/api/users/123`  
- Identifies a specific user/resource.  

**Security best practices:**
- Validate paths to prevent unauthorized access.
- Sanitize inputs to block injection attacks.
- Protect sensitive data through proper access controls.

---

## ⚡ HTTP Versions

| Version    | Year | Key Features |
|------------|------|--------------|
| **HTTP/0.9** | 1991 | Only `GET`, very basic. |
| **HTTP/1.0** | 1996 | Added headers, MIME support, caching. |
| **HTTP/1.1** | 1997 | Persistent connections, chunked transfer, better caching. |
| **HTTP/2**   | 2015 | Multiplexing, header compression, prioritization. |
| **HTTP/3**   | 2022 | Based on QUIC protocol, faster & more secure. |

>  **Tip:** While HTTP/1.1 is still widely used, upgrading to **HTTP/2 or HTTP/3** improves performance and security.

---

#  HTTP Request: Headers and Body

##  Request Headers

Request Headers allow extra information to be conveyed to the web server.  

| Header       | Example | Description |
|--------------|---------|-------------|
| **Host** | `Host: tryhackme.com` | Specifies the name of the web server. |
| **User-Agent** | `User-Agent: Mozilla/5.0` | Info about the client/browser. |
| **Referer** | `Referer: https://www.google.com/` | Indicates the source URL of the request. |
| **Cookie** | `Cookie: user_type=student; room=introtowebapplication; room_status=in_progress` | Stores stateful info provided by the server. |
| **Content-Type** | `Content-Type: application/json` | Defines the format of the request body data. |

---

##  Request Body

Used in requests such as **POST** or **PUT** where data is sent to the server.  
Common formats:  

---

### 1.  URL Encoded (application/x-www-form-urlencoded)

- Data in **key=value** pairs, separated by `&`.  
- Special characters are percent-encoded.  

**Example:**

POST /profile HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

name=Aleksandra&age=27&country=US



---

### 2. 📂 Form Data (multipart/form-data)

- Supports multiple blocks of data.  
- Often used for **file uploads**.  
- Boundaries separate different parts of the data.  

**Example:**

POST /upload HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here representing the image]
----WebKitFormBoundary7MA4YWxkTrZu0gW--



---

### 3. 🧾 JSON (application/json)

- Data formatted as **name:value** pairs.  
- Lightweight, commonly used in APIs.  

**Example:**

POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Content-Length: 62

{
"name": "Aleksandra",
"age": 27,
"country": "US"
}



---

### 4. 📑 XML (application/xml)

- Data wrapped in **tags** with opening & closing markers.  
- Supports nested structures.  

**Example:**

POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/xml
Content-Length: 124

<user> <name>Aleksandra</name> <age>27</age> <country>US</country> </user> ```
 Security Reminders
Always validate and sanitize request data.

Use HTTPS to protect headers and body data.

Handle cookies & tokens securely to prevent hijacking.

Disable unused HTTP methods to reduce attack surface.

#  HTTP Responses – Cybersecurity Notes

When you interact with a web application, the **server sends back an HTTP response** to let you know whether your request was successful or something went wrong.  

Responses contain:  
- **Status Line** → protocol version, status code, reason phrase  
- **Headers** → extra info (content type, length, cookies, etc.)  
- **Body** → optional data (HTML, JSON, files, etc.)  

---

##  Status Line

Format:

HTTP/version status_code reason_phrase



Example:

HTTP/1.1 200 OK



- **HTTP Version** → Protocol version (e.g., HTTP/1.1)  
- **Status Code** → Three-digit code showing result of request  
- **Reason Phrase** → Short human-readable message  

---

##  Status Codes & Categories

Status codes fall into **five main groups**:

| Category | Range | Meaning |
|----------|-------|---------|
| **Informational** | 100–199 | Request received, keep going |
| **Success** | 200–299 | Request processed successfully |
| **Redirection** | 300–399 | Resource moved, client must follow |
| **Client Error** | 400–499 | Issue with request (bad input, missing auth) |
| **Server Error** | 500–599 | Server failed to handle valid request |

---

##  Common HTTP Status Codes

| Code | Reason Phrase | Meaning |
|------|---------------|---------|
| **100** | Continue | Server got initial request, waiting for more. |
| **200** | OK | Request succeeded, resource returned. |
| **301** | Moved Permanently | Resource moved to a new URL. |
| **302** | Found | Temporary redirection (often used for login redirects). |
| **400** | Bad Request | Malformed request syntax or invalid data. |
| **401** | Unauthorized | Authentication required or failed. |
| **403** | Forbidden | Client is not allowed to access this resource. |
| **404** | Not Found | Resource not found at given URL. |
| **500** | Internal Server Error | Generic server-side failure. |
| **502** | Bad Gateway | Invalid response from upstream server. |
| **503** | Service Unavailable | Server overloaded or down for maintenance. |

---

## 🛡️ Security Reminders

- Avoid exposing sensitive error details in responses (stack traces, DB errors).  
- Use **custom error pages** to reduce information leakage.  
- Monitor **500-series errors** (may indicate app/server issues).  
- Rate-limit repeated **400-series errors** (could be brute force attempts).  
- Ensure **redirects (3xx)** cannot be abused for phishing/open redirect attacks.  

#  HTTP Response: Headers and Body

When a web server responds to an HTTP request, it includes **HTTP response headers** (key-value pairs) and often a **response body**.  
- **Headers** → Provide metadata and handling instructions.  
- **Body** → Contains the actual data (HTML, JSON, images, files, etc.).  

---

##  Response Headers

###  Required Response Headers
Some headers are essential for proper communication between client and server:

| Header | Example | Description |
|--------|---------|-------------|
| **Date** | `Date: Fri, 23 Aug 2024 10:43:21 GMT` | Timestamp when the response was generated. |
| **Content-Type** | `Content-Type: text/html; charset=utf-8` | Defines the type of content (HTML, JSON, XML, etc.) and character set. |
| **Server** | `Server: nginx` | Identifies the server software handling the request. (⚠️ Can reveal info to attackers, often hidden/obscured). |

---

###  Other Common Response Headers

| Header | Example | Description |
|--------|---------|-------------|
| **Set-Cookie** | `Set-Cookie: sessionId=38af1337es7a8; HttpOnly; Secure` | Sends cookies to the client. Use `HttpOnly` (prevents JavaScript access) and `Secure` (HTTPS only). |
| **Cache-Control** | `Cache-Control: max-age=600` | Tells the client how long to cache the response. Use `no-cache` for sensitive data. |
| **Location** | `Location: /index.html` | Used in **redirection (3xx)** responses. Must be validated to prevent **open redirect attacks**. |

---

##  Response Body

The **HTTP response body** contains the actual resource requested.  
Examples include:  
- HTML pages  
- JSON data (APIs)  
- Images, files, media  

**Security Best Practices:**
- **Sanitize and escape** user-generated data before including it (to prevent **XSS**).  
- Only return **necessary information** to minimize leakage.  
- Use **Content Security Policy (CSP)** headers to mitigate script injection.  

---

##  Example HTTP Response

HTTP/1.1 200 OK
Date: Fri, 23 Aug 2024 10:43:21 GMT
Server: nginx
Content-Type: text/html; charset=utf-8
Content-Length: 137

<!DOCTYPE html> <html> <head> <title>Welcome</title> </head> <body> <h1>Hello, Aleksandra!</h1> </body> </html> ```
## Security Reminders
Hide or obfuscate Server headers to reduce fingerprinting.

Always use secure cookie attributes (HttpOnly, Secure, SameSite).

Control caching of sensitive data with proper Cache-Control headers.

Validate redirect Location headers to prevent open redirects.

Escape and sanitize all response content to stop injection attacks.

#  HTTP Security Headers

**HTTP Security Headers** help improve the security of web applications by mitigating common attacks such as **Cross-Site Scripting (XSS)**, **Clickjacking**, and **MIME-type sniffing**.  

You can test any site’s security headers at: [securityheaders.io](https://securityheaders.io/)  

---

##  Content-Security-Policy (CSP)

The **CSP header** adds an extra security layer by restricting where resources (scripts, styles, images, etc.) can be loaded from. It helps mitigate **XSS attacks** by allowing only trusted sources.  

**Example:**

Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'

pgsql
Copy code

**Directives:**
- **default-src 'self'** → Only load resources from the current site.  
- **script-src 'self' https://cdn.tryhackme.com** → Scripts allowed from same origin + CDN.  
- **style-src 'self'** → Styles only from same origin.  

 **Best Practice:** Start with restrictive defaults (`default-src 'self'`) and whitelist only what’s necessary.  

---

##  Strict-Transport-Security (HSTS)

The **HSTS header** enforces **HTTPS-only connections**. Browsers will refuse to connect via HTTP once this is set.  

**Example:**

Strict-Transport-Security: max-age=63072000; includeSubDomains; preload

pgsql
Copy code

**Directives:**
- **max-age=63072000** → 2 years in seconds.  
- **includeSubDomains** → Apply HSTS to all subdomains.  
- **preload** → Allows site to be preloaded into browser HSTS lists.  

 **Best Practice:** Always set HSTS for HTTPS sites; combine with `preload` for extra protection.  

---

##  X-Content-Type-Options

This header prevents browsers from **MIME-sniffing** and ensures files are interpreted only as declared in `Content-Type`.  

**Example:**

X-Content-Type-Options: nosniff

yaml
Copy code

**Directive:**
- **nosniff** → Browser must trust `Content-Type` and not guess.  

 **Best Practice:** Always set `nosniff` to reduce attack surface (especially against malicious uploads).  

---

##  Referrer-Policy

Controls how much **referrer information** is sent when navigating from one site to another.  

**Examples:**

Referrer-Policy: no-referrer
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin

pgsql
Copy code

**Directives:**
- **no-referrer** → Sends no referrer info at all.  
- **same-origin** → Sends referrer only for same-origin requests.  
- **strict-origin** → Sends referrer origin only if protocol stays the same (HTTPS→HTTPS).  
- **strict-origin-when-cross-origin** → Full URL for same-origin; only origin for cross-origin.  

 **Best Practice:** Use `strict-origin-when-cross-origin` (modern secure default).  

---

##  Summary of Security Headers

| Header | Purpose | Example |
|--------|---------|---------|
| **CSP** | Prevent XSS by restricting resource loading | `Content-Security-Policy: default-src 'self'` |
| **HSTS** | Force HTTPS connections | `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload` |
| **X-Content-Type-Options** | Prevent MIME sniffing | `X-Content-Type-Options: nosniff` |
| **Referrer-Policy** | Control referrer info in requests | `Referrer-Policy: strict-origin-when-cross-origin` |

 Adding these headers significantly strengthens a website’s **defense-in-depth strategy**.  
# JavaScript Essentials
 #  JavaScript Basics

## Variables

Variables are containers that allow you to store data values in them. Like any other language, variables in JS are similar to containers to store data. When you store something in a bucket, you also need to label it so that it can be referenced later on easily. Similarly, in JS, each variable has a name; when we store a certain value in a variable, we assign a name to it to reference it later.

There are three ways to declare variables in JS: **var**, **let**, and **const**. While var is function-scoped, both let, and const are block-scoped, offering better control over variable visibility within specific code blocks.

## Data Types

In JS, data types define the type of value a variable can hold. Examples include **string** (text), **number**, **boolean** (true/false),**null**, **undefined**, and **object** (for more complex data like arrays or objects).

## Functions

A function represents a block of code designed to perform a specific task. Inside a function, you group code that needs to perform a similar task. For example, you are developing a web application in which you need to print students' results on the web page. The ideal case would be to create a function **PrintResult(rollNum)** that would accept the roll number of the user as an argument.
```bash
<script>
        function PrintResult(rollNum) {
            alert("Username with roll number " + rollNum + " has passed the exam");
            // any other logic to display the result
        }

        for (let i = 0; i < 100; i++) {
            PrintResult(rollNumbers[i]);
        }
    </script>
```
So, instead of writing the same print code for all the students, we will use a simple function to print the result.

Loops

Loops allow you to run a code block multiple times as long as a condition is **true**. Common loops in JS are **for**, **while**, and **do...while**, which are used to repeat tasks, like going through a list of items. For example, if we want to print the results of 100 students, we can call the PrintResult(rollNum) function 100 times by writing it 100 times, or we can create a loop that will be iterated through 1 to 100 and will call the PrintResult(rollNum) function as shown below.
```js
       // Function to print student result
        function PrintResult(rollNum) {
            alert("Username with roll number " + rollNum + " has passed the exam");
            // any other logic to the display result
        }

        for (let i = 0; i < 100; i++) {
            PrintResult(rollNumbers[i]); // this will be called 100 times 
        }
```
## JavaScript Overview
In this task, we’ll use JS to create our first program. JS is an interpreted language, meaning the code is executed directly in the browser without prior compilation. Below is a sample JS code demonstrating key concepts, such as defining a variable, understanding data types, using control flow statements, and writing simple functions. These essential building blocks help create more dynamic and interactive web apps. Don’t worry if it looks a bit new now - we will discuss each of these concepts in detail later on.
```js
 // Hello, World! program
console.log("Hello, World!");

// Variable and Data Type
let age = 25; // Number type

// Control Flow Statement
if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are a minor.");
}

// Function
function greet(name) {
    console.log("Hello, " + name + "!");
}

// Calling the function
greet("Bob");
```
Is JavaScript a compiled or interpreted language?Interpreted

## Integrating JavaScript in HTML

### Internal JavaScript

Internal JS refers to embedding the JS code directly within an HTML document. This method is preferable for beginners because it allows them to see how the script interacts with the HTML. The script is inserted between **<script>** tags. These tags can be placed inside the **<head>** section, typically used for scripts that need to be loaded before the page content is rendered, or inside the **<body>** section, where the script can be utilised to interact with elements as they are loaded on the web page.

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <script>
        let x = 5;
        let y = 10;
        let result = x + y;
        document.getElementById("result").innerHTML = "The result is: " + result;
    </script>
</body>
</html>
```
In this HTML document, we are using internal JS, meaning the code is placed directly inside the HTML file within the **<script>**tag. The script performs a simple task: it adds two numbers (x and y) and then displays the result on the web page. The JS interacts with the HTML by selecting an element **(<p> with id="result")** and updating its content using **document.getElementById("result").innerHTML**. This internal JS is executed when the browser loads the HTML file.

## External JavaScript

External JS involves creating and storing JS code in a separate file ending with a .js file extension. This method helps developers keep the HTML document clean and organised. The external JS file can be stored or hosted on the same web server as the HTML document or stored on an external web server such as the cloud.

We will use the same example for external JS but separate the JS code into a different file.

First, create a new file named script.js and save it on the Desktop with the following code:
```js
let x = 5;
let y = 10;
let result = x + y;
document.getElementById("result").innerHTML = "The result is: " + result;
```
Next, create a new file named external.html and paste the following code (notice that the HTML code is the same as that of the previous example):
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>External JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <!-- Link to the external JS file -->
    <script src="script.js"></script>
</body>
</html>
```
## Abusing Dialogue Function

One of the main objectives of JS is to provide dialogue boxes for interaction with users and dynamically update content on web pages. JS provides built-in functions like **alert**, **prompt**, and **confirm** to facilitate this interaction. These functions allow developers to display messages, gather input, and obtain user confirmation. However, if not implemented securely, attackers may exploit these features to execute attacks like Cross-Site Scripting (XSS).

### Alert

The alert function displays a message in a dialogue box with an **"OK"** button, typically used to convey information or warnings to users. For example, if we want to display "**Hello THM**" to the user, we would use an **alert("HelloTHM")**;. To try it out, open the Chrome console, type **alert("Hello THM")**, and press Enter. A dialogue box with the message will appear on the screen.
```js
alert("Massage");
```

## Prompt

The prompt function displays a dialogue box that asks the user for input. It returns the entered value when the user clicks "**OK**", or null if the user clicks "**Cancel**". For example, to ask the user for their name, we would use **prompt("What is your name?")**;.
```js
name = prompt("What is your name?");
    alert("Hello " + name);
```
## Confirm

The confirm function displays a dialogue box with a message and two buttons: "OK" and "**Cancel**". It returns true if the user clicks "**OK**" and false if the user clicks "**Cancel**". For example, to ask the user for confirmation, we would use **confirm("Are you sure?")**;. To try this out, open the Chrome console, type **confirm("Do you want to proceed?")**, and press Enter.
```js
confirm("Do you want to proceed?")
````

## How Hackers Exploit the Functionality

Imagine receiving an email from a stranger with an attached HTML file. The file looks harmless, but when you open it, it contains JS that disrupts your browsing experience. For example, the following code will show an alert box with the message "Hacked" three times:
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Hacked</title>
</head>
<body>
    <script>
        for (let i = 0; i < 3; i++) {
            alert("Hacked");
        }
    </script>
</body>
</html>
```
## Bypassing Control Flow Statements
Control flow in JS refers to the order in which statements and code blocks are executed based on certain conditions. JS provides several control flow structures such as if-else, switch statements to make decisions, and loops like **for**, **while**, and **do...while** to repeat actions. Proper use of control flow ensures that a program can handle various conditions effectively.

### Conditional Statements in Action

One of the most used conditional statements is the **if-else** statements, which allows you to execute different blocks of code depending on whether a condition evaluates to **true** or **false**.
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Age Verification</title>
</head>
<body>
    <h1>Age Verification</h1>
    <p id="message"></p>

    <script>
        age = prompt("What is your age")
        if (age >= 18) {
            document.getElementById("message").innerHTML = "You are an adult.";
        } else {
            document.getElementById("message").innerHTML = "You are a minor.";
        }
    </script>
</body>
</html>
```
### Bypassing Login Forms

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Login Page</title>
</head>
<body>
    <h2>Login Authentication</h2>

    <script>
        let username = prompt("Enter your username:");
        let password = prompt("Enter your password:");

        if (username === "admin" && password === "ComplexPassword") {
            document.write("You are successfully authenticated!");
        } else {
            document.write("Authentication failed. Incorrect username or password.");
        }
    </script>
</body>
</html>
```
## Exploring Minified Files
Minification in JS is the process of compressing JS files by removing all unnecessary characters, such as spaces, line breaks, comments, and even shortening variable names. This helps reduce the file size and improves the loading time of web pages, especially in production environments. Minified files make the code more compact and harder to read for humans, but they still function exactly the same.

Similarly, **obfuscation** is often used to make JS harder to understand by adding undesired code, renaming variables and functions to meaningless names, and even inserting dummy code.

```js
function hi() {
  alert("Welcome to THM");
}
hi();
```
**Obfuscation in Action : https://obf-io.deobfuscate.io**
After Obfuscation
```js
(function(_0x114713,_0x2246f2){var _0x51a830=_0x33bf,_0x4ce60b=_0x114713();while(!![]){try
{var _0x51ecd3=-parseInt(_0x51a830(0x88))/(-0x1bd3+-0x9a+0x2*0xe37)*(parseInt(_0x51a830(0x94))/
(-0x15c1+-0x2*-0x3b3+0xe5d))+parseInt(_0x51a830(0x8d))/(0x961*0x1+0x2*0x4cb+0x4bd*-0x4)*
(-parseInt(_0x51a830(0x97))/(-0x22b3+0x16e9+0x1*0xbce))+parseInt(_0x51a830(0x89))/
(-0x631+0x20cd+0x8dd*-0x3)*(-parseInt(_0x51a830(0x95))/(-0x8fc+0x161+0x7a1))+-
parseInt(_0x51a830(0x93))/(-0x1c38+0x193+0x1aac)*(parseInt(_0x51a830(0x8e))/
(-0x1*-0x17a6+-0x167e+-0x3*0x60))+-parseInt(_0x51a830(0x91))/(-0x2*-0x1362+-0x4a8*0x5+-0xf73)*
(parseInt(_0x51a830(0x8b))/(-0xb31*0x2+0x493*0x5+0x1*-0x73))+parseInt(_0x51a830(0x8f))/
(-0x257a+-0x1752+0x3cd7)+parseInt(_0x51a830(0x90))/(-0x2244+-0x15f9+0x3849);if(_0x51ecd3
===_0x2246f2)break;else _0x4ce60b['push'](_0x4ce60b['shift']());}catch(_0x38d15c)
{_0x4ce60b['push'](_0x4ce60b['shift']());}}}(_0x11ed,-0x17d11*-0x1+0x2*0x2e27+0x100f*0x17));
function hi(){var _0x48257e=_0x33bf,_0xab1127={'xMVHQ':function(_0x4eefa0,_0x4e5f74)
{return _0x4eefa0(_0x4e5f74);},'FvtWc':_0x48257e(0x96)+_0x48257e(0x92)};_0xab1127[_0x48257e(0x8c)
](alert,_0xab1127[_0x48257e(0x8a)]);}function _0x33bf(_0xb07259,_0x5949fe){var _0x3a386b
=_0x11ed();return _0x33bf=function(_0x4348ee,_0x1bbf73){_0x4348ee=_0x4348ee-(0x11f7+-
0x1*0x680+-0x3a5*0x3);var _0x423ccd=_0x3a386b[_0x4348ee];return _0x423ccd;},_0x33bf
(_0xb07259,_0x5949fe);}function _0x11ed(){var _0x4c8fa8=['7407EbJESQ','\x20THM',
'2700698TTmqXC','10ILFtfZ','190500QONgph','Welcome\x20to',
'4492QOmepo','21623eEAyaP','65XMlsxw','FvtWc','2410qfnGAy','xMVHQ','321PfYXZg',
'8XBaIAe','1946483GviJfa','15167592PYYhTN'];_0x11ed=function(){return _0x4c8fa8;};return _0x11ed();}hi();
```


### Avoid Relying on Client-Side Validation Only

One of JS's primary functions is performing client-side validation. Developers sometimes use it for validating forms and rely entirely on it, which is not a good practice. Since a user can disable/manipulate JS on the client side, performing validation on the server side is also essential.

### Refrain from Adding Untrusted Libraries

As discussed in earlier tasks, JS allows you to include any other JS scripts using the src attribute inside a script tag. But have you considered whether the library we include is from a trusted source? Bad actors have uploaded a bundle of libraries on the internet with names that resemble legitimate ones. So, if you blindly include a malicious library, you will expose your web application to threats.

### Avoid Hardcoded Secrets

Never hardcode sensitive data like API keys, access tokens, or credentials into your JS code, as the user can easily check the source code and get the password. 
```js 
// Bad Practice
const privateAPIKey = 'pk_TryHackMe-1337'; 
```
### Minify and Obfuscate Your JavaScript Code

Minifying and obfuscating JS code reduces its size, improves load time, and makes it harder for attackers to understand the logic of the code. Therefore, always minify and obfuscate the code when using code in production. The attacker can eventually reverse engineer it, but getting the original code will take at least some effort. 
















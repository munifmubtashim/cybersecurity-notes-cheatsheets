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


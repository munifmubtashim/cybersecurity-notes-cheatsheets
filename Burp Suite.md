# What is Burp Suite ?
Burp Suite is Java based freamework designed to serve as a comprehensive solution for conducting web application penetration testing.It captures and enables manipulation of all the HTTP?HTTPS traffic between a browser and a web server.

---
Burp Suite is an integrated platform for performing security testing of web applications. It includes various tools for scanning, fuzzing, intercepting, and analysing web traffic. It is used by security professionals worldwide to find and exploit vulnerabilities in web applications.
---

We will focus on the **Burp Suite Community Edition**, which is freely accessible for non-commercial use within legal boundaries. However, it's worth noting that Burp Suite also offers Professional and Enterprise editions, which come with advanced features and require licensing:

**1. Burp Suite Professional is an unrestricted version of Burp Suite Community. It comes with features such as:**

- An automated vulnerability scanner.
- A fuzzer/brute-forcer that isn't rate limited.
- Saving projects for future use and report generation.
- A built-in API to allow integration with other tools.
- Unrestricted access to add new extensions for greater functionality.
- Access to the Burp Suite Collaborator (effectively providing a unique request catcher self-hosted or running on a Portswigger-owned server).

**2.Burp Suite Enterprise**

In contrast to the community and professional editions, is primarily utilized for continuous scanning. It features an automated scanner that periodically scans web applications for vulnerabilities, similar to how tools like Nessus perform automated infrastructure scanning. Unlike the other editions, which allow manual attacks from a local machine, Burp Suite Enterprise resides on a server and constantly scans the target web applications for potential vulnerabilities.

**Q&A**

Which edition of Burp Suite runs on a server and provides constant scanning for target web apps?Burp Suite Enterprise

Burp Suite is frequently used when attacking web applications and ______ applications.Mobile

# Features of Burp Community



**Proxy**
- Intercept & modify requests/responses  
- Main tool for analyzing traffic  

**Repeater**
- Capture, edit, and resend requests  
- Great for testing payloads (e.g., SQLi, XSS)  

**Intruder (limited in Community)**
- Automates multiple requests  
- Used for brute force & fuzzing  

**Decoder**
- Encode / Decode / Transform data  
- Useful for analyzing encoded info or preparing payloads  

**Comparer**
- Compare two sets of data (word/byte level)  
- Quick shortcut to send data for comparison  

**Sequencer**
- Tests randomness of tokens (e.g., session IDs, cookies)  
- Helps find weak token generation  

---

**Extensions**
- Burp supports add-ons via **Extender**  
- Extensions can be written in **Java, Python (Jython), Ruby (JRuby)**  
- **BApp Store** → download community extensions  
- Example: **Logger++** → better logging  

---

**Note:** Burp Community = Limited Intruder, but still powerful for web app testing & learning.

**Q&A**
Which Burp Suite feature allows us to intercept requests between ourselves and the target?Proxy

Which Burp tool would we use to brute-force a login form?Intruder


# Installation

https://portswigger.net/burp/releases





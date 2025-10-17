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

# The Dashboard

- **Tasks**:Define Background tasks that Burp Suite will perform while use the application.
- **Event Log**: Provides the actions performed by Burp Suite.
-  **Issue Activity**:It displays the vulnerabilities by automated scanner in Burp Suite Profressional.
-  **Advisory**:Provides more detailed information about the identified vulnerbilities.

  What menu provides information about the actions performed by Burp Suite, such as starting the proxy, and details about connections made through Burp?Event log.

# Navigation

|Shortcut   |  Tab      | 
|-----------|-----------|
|Ctrl + Shift + D|	Dashboard|
|Ctrl + Shift + T|	Target tab|
|Ctrl + Shift + P|	Proxy tab|
|Ctrl + Shift + I|	Intruder tab|
|Ctrl + Shift + R|	Repeater tab|

# Options

### There are two types of settings:
- **Global Settings**: these settings affect the entire Burp Suite installation and every time start the application.
- **Project Settings**:are specific to the current project and apply only during the session.

### In the Settings window, you will find a menu on the left-hand side. This menu allows you to switch between different types of settings, including:

- **Search**: Enables searching for specific settings using keywords.
- **Type filter**: Filters the settings for User and Project options.
- **User settings**: Shows settings that affect the entire Burp Suite installation.
- **Project settings**: Displays settings specific to the current project.
- **Categories**: Allows selecting settings by category.

**Q&A**
- In which category can you find a reference to a "Cookie jar"?Sessions

- In which base category can you find the "Updates" sub-category, which controls the Burp Suite update behaviour?Suite

- What is the name of the sub-category which allows you to change the keybindings for shortcuts in Burp Suite?Hotkeys


- If we have uploaded Client-Side TLS certificates, can we override these on a per-project basis (yea/nay)?yea

# Introduction to the Burp Proxy

The Burp Proxy is a fundamental and crucial tool within Burp Suite. It enables the capture of requests and responses between the user and the target web server.

### Key Points to Understand About the Burp Proxy
- **Intercepting Requests**: When requests are made through the Burp Proxy, they are intercepted and held back from reaching the target server. The requests appear in the Proxy tab, allowing for further actions such as forwarding, dropping, editing, or sending them to other Burp modules. To disable the intercept and allow requests to pass through the proxy without interruption, click the Intercept is on button.
- **Taking Control**: The ability to intercept requests empowers testers to gain complete control over web traffic, making it invaluable for testing web applications.
- **Capture and Logging**: Burp Suite captures and logs requests made through the proxy by default, even when the interception is turned off. This logging functionality can be helpful for later analysis and review of prior requests.
- **WebSocket Support**: Burp Suite also captures and logs WebSocket communication, providing additional assistance when analysing web applications.
- **Logs and History**: The captured requests can be viewed in the HTTP history and WebSockets history sub-tabs, allowing for retrospective analysis and sending the requests to other Burp modules as needed.

### Some Notable Features in the Proxy Settings
- **Response Interception**: By default, the proxy does not intercept server responses unless explicitly requested on a per-request basis. The "Intercept responses based on the following rules" checkbox, along with the defined rules, allows for a more flexible response interception.
- **Match and Replace**: The "Match and Replace" section in the Proxy settings enables the use of regular expressions (regex) to modify incoming and outgoing requests. This feature allows for dynamic changes, such as modifying the user agent or manipulating cookies.


# Connecting through the Proxy (FoxyProxy)

### Here are the steps to configure the Burp Suite Proxy with FoxyProxy:

- Install FoxyProxy: Download and install the FoxyProxy Basic extension.

Note: FoxyProxy is already installed on the AttackBox.

- Access FoxyProxy Options: Once installed, a button will appear at the top right of the Firefox browser. Click on the FoxyProxy button to access the FoxyProxy options pop-up.
- Create Burp Proxy Configuration: In the FoxyProxy options pop-up, click the Options button. This will open a new browser tab with the FoxyProxy configurations. Click the Add button to create a new proxy configuration.
- Add Proxy Details: On the "Add Proxy" page, fill in the following values:

Title: Burp (or any preferred name)
Proxy IP: 127.0.0.1
Port: 8080
- Save Configuration: Click Save to save the Burp Proxy configuration.
- Activate Proxy Configuration: Click on the FoxyProxy icon at the top-right of the Firefox browser and select the Burp configuration. This will redirect your browser traffic through 127.0.0.1:8080. Note that Burp Suite must be running for your browser to make requests when this configuration is activated.
- Test the Proxy: Open Firefox and try accessing a website, such as the homepage for http://10.201.70.12/. Your browser will hang, and the proxy will populate with the HTTP request. Congratulations, you have successfully intercepted your first request!
  
# Site Map and Issue Definitions

- **Site map**:Shows every page and endpoint Burp has seen in a tree view. Helps you quickly explore what exists on the site (pages, APIs, files). Tip: just browse the app with Burp proxy on and the site map fills up automatically.

- **Issue definitions**:A built-in catalog of web vulnerabilities (descriptions and references). Useful as a quick reference when you want to understand or explain an issue — even if you’re not using the automated scanner.

- **Scope settings**:Tell Burp which hosts/URLs you do or don’t want to test. Keeps your testing focused and prevents accidental capture of unrelated traffic.
**Q&A**
  What is the flag you receive after visiting the unusual endpoint?THM{NmNlZTliNGE1MWU1ZTQzMzgzNmFiNWVk}

# Scoping and Targeting

Scoping in Burp Suite is used to focus your testing on specific web applications and avoid unnecessary traffic. By adding your target to the scope from the **Target** tab (right-click → **Add to Scope**), Burp will only log and display activity related to that target. You can review and edit this scope in the **Scope settings** sub-tab, where you can include or exclude certain domains or IPs. To make Burp ignore all other traffic completely, go to **Proxy → Options → Intercept Client Requests** and enable **And URL Is in target scope**. This keeps your workspace clean and ensures you only capture and test what’s relevant.





# Linux Essentials & Recon Tools Cheat Sheet  

## 1. Basic Linux Commands  
### Navigation & Files  
- `pwd` → print working directory  
- `ls -la` → list all files (detailed)  
- `cd <dir>` → change directory  
- `touch file.txt` → create file  
- `mkdir dir` → create directory  
- `cp src dest` → copy file  
- `mv src dest` → move/rename file  
- `rm file.txt` → remove file  

### Viewing & Editing  
- `cat file.txt` → view file  
- `less file.txt` → view paged output  
- `head -n 10 file.txt` → first 10 lines  
- `tail -f log.txt` → live logs  
- `nano file.txt` → edit in nano  

### Permissions  
- `chmod 755 file.sh` → change permissions  
- `chown user:group file` → change ownership  
- `chown +x file` → file permission 
---

## 2. System & Network Info  
- `whoami` → show current user  
- `id` → show UID & groups  
- `ifconfig` or `ip a` → show network interfaces  
- `ping target.com` → test connectivity  
- `curl http://site.com` → fetch webpage  
- `wget http://site.com` → download file  
- `netstat -tulnp` or `ss -tulnp` → show open ports  

---

## 3. Reconnaissance Tools  

### 🕵️ Sherlock  
Find usernames across social media.  
```bash
git clone https://github.com/sherlock-project/sherlock.git
cd sherlock
python3 sherlock username
```

### 🌐 whois  
Get domain registration info.  
```bash
whois example.com
```

### 📧 theHarvester  
Gather emails, names, subdomains from search engines.  
```bash
theHarvester -d example.com -l 200 -b google
```

### 🔗 Crosslinked  
LinkedIn enumeration (employees, usernames).  
```bash
git clone https://github.com/m8r0wn/crosslinked.git
cd crosslinked
python3 crosslinked.py -d company.com
```

### 🔍 recon-ng  
Framework for web-based reconnaissance.  
```bash
recon-ng
# inside recon-ng
modules search
modules load recon/domains-hosts/bing_domain_web
set SOURCE example.com
run
```

---

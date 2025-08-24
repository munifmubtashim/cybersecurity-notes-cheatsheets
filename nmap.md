#  Nmap Full Cheat Sheet

Nmap (Network Mapper) is a free and open-source tool for network discovery and security auditing.  
This file contains categorized commands for quick reference.

---

##  1. Basics
```bash
nmap <target>             # Default scan
nmap scanme.nmap.org      # Example target
nmap 192.168.1.1          # Single IP scan
```
---

##  2. Targeting
```bash
nmap target1 target2           # Multiple targets
nmap 192.168.1.1,2,3           # List of IPs
nmap 192.168.1.1-50            # Range of IPs
nmap 192.168.1.0/24            # Subnet scan
nmap -iL list.txt              # Targets from file
```
---

##  3. Host Discovery
```bash
nmap -sn 192.168.1.0/24        # Ping sweep only
nmap -Pn <target>              # Skip host discovery
nmap -PR 192.168.1.0/24        # ARP discovery
```
---

##  4. Port Scanning
```bash
nmap -p 80 <target>            # Specific port
nmap -p 22,80,443 <target>     # Multiple ports
nmap -p 1-1000 <target>        # Port range
nmap -F <target>               # Fast scan (100 ports)
nmap --top-ports 20 <target>   # Top 20 ports
```
---

##  5. Scan Types
```bash
nmap -sS <target>              # SYN (stealth) scan
nmap -sT <target>              # TCP connect scan
nmap -sU <target>              # UDP scan
nmap -sA <target>              # ACK scan
nmap -sW <target>              # Window scan
nmap -sM <target>              # Maimon scan
nmap -sN <target>              # Null scan
nmap -sF <target>              # FIN scan
nmap -sX <target>              # Xmas scan
```
---

##  6. Service & OS Detection
```bash
nmap -sV <target>              # Service version detection
nmap -O <target>               # OS detection
nmap -A <target>               # Aggressive (OS + version + traceroute + scripts)
nmap --traceroute <target>     # Traceroute
```
---

##  7. Output Options
```bash
nmap -oN output.txt <target>   # Normal output
nmap -oX output.xml <target>   # XML output
nmap -oG output.gnmap <target> # Grepable format
nmap -oA results <target>      # All formats
```
---

##  8. NSE Scripts (Nmap Scripting Engine)
```bash
nmap --script=default <target> # Default scripts
nmap --script=vuln <target>    # Vulnerability detection
nmap --script=auth <target>    # Authentication related
nmap --script=discovery <tgt>  # Host discovery scripts
nmap --script=safe <target>    # Safe scripts only
nmap --script=http-enum <tgt>  # HTTP enumeration
nmap --script=smb-os-discovery <tgt> # SMB OS discovery
nmap --script=dns-brute <tgt>  # DNS subdomain brute force
```
---

##  9. Firewall / IDS Evasion
```bash
nmap -D RND:10 <target>        # Decoy scan
nmap -S <spoofedIP> <target>   # IP spoofing
nmap -g 80 <target>            # Source port manipulation
nmap --data-length 50 <target> # Append random data
nmap -f <target>               # Fragment packets
```
---

##  10. IPv6 Scanning
```bash
nmap -6 <target>               # IPv6 scanning
nmap -6 -sS <target>           # SYN scan over IPv6
```
---

##  11. Performance & Timing
```bash
nmap -T0 <target>              # Paranoid (IDS evasion)
nmap -T1 <target>              # Sneaky
nmap -T3 <target>              # Normal (default)
nmap -T4 <target>              # Aggressive (faster)
nmap -T5 <target>              # Insane (fastest)
```
---

##  12. Miscellaneous
```bash
nmap -n <target>               # No DNS resolution
nmap -R <target>               # Force DNS resolution
nmap --reason <target>         # Show reason for results
nmap --open <target>           # Show only open ports
nmap --packet-trace <target>   # Show all packets
nmap --iflist                  # Show interfaces & routes
nmap -d <target>               # Debug mode
```

---

# Quick Reference
- **Simple scan:** `nmap target`
- **Subnet scan:** `nmap 192.168.1.0/24`
- **Service detection:** `nmap -sV target`
- **OS detection:** `nmap -O target`
- **Aggressive scan:** `nmap -A target`
- **Vulnerability check:** `nmap --script=vuln target`
- **Save results:** `nmap -oN file.txt target`

---



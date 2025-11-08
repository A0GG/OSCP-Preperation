## 🌐 Web Enumeration (Machines 161–170)

```bash
# Common web ports and service detection
nmap -p 80,443,8080,8000,8443 -sV <IP>

# Web technology fingerprinting
whatweb http://<IP>
wappalyzer http://<IP>

# Directory enumeration
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirb/common.txt -x php,html,txt
ffuf -u http://<IP>/FUZZ -w /usr/share/wordlists/dirb/common.txt

# Virtual host enumeration
ffuf -u http://<IP> -H "Host: FUZZ.<domain>" -w /usr/share/wordlists/dns/subdomains-top1million-5000.txt

# Check for common files
curl -I http://<IP>/.git/
curl -I http://<IP>/robots.txt
curl -I http://<IP>/backup.zip

# CMS detection
wpscan --url http://<IP> --enumerate u,vp,vt,cb,dbe --api-token <token>
droopescan scan drupal -u http://<IP>
joomscan -u http://<IP>
```

### Observations from Machines 161–170

- Several machines hosted WordPress sites with exposed `/xmlrpc.php`, vulnerable plugins, or outdated core versions.
- Joomla sites revealed SQL injection via vulnerable components.
- Upload forms were tested for content-type bypass and extension filtering (.php5, .phtml).
- Virtual hosts and subdomain brute-force yielded access to admin panels in some machines.
- `.git` and `.svn` folders found in multiple machines, leading to source code disclosure.

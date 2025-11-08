
# Web Enumeration (Machines 191–200)

## Enumeration Process

### Common Techniques:
- **Directory Bruteforce**: 
```bash
gobuster dir -u http://<IP> -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```

- **Service Detection**: 
```bash
nmap -sC -sV <IP>  # Detect versions and scripts for common services.
```

- **Exposed Admin Pages**:
```bash
curl http://<IP>/admin.php  # Checking for admin panels
```

- **Exposed Git Repositories**:
```bash
curl -I http://<IP>/.git/  # Checking for .git repository leaks.
```

- **Common CMS Enumeration**:
```bash
whatweb http://<IP>  # Identify CMS and technologies used.
```

## Results:
- **Machine 191**: Found /admin.php → Further enumeration needed on wp-login.php.
- **Machine 192**: .git directory found → Recovered sensitive information from GitHub.
- **Machine 193**: Discovered exposed WordPress site → Brute-forced admin login credentials.
- **Machine 194**: Exposed backups in /backup → Downloaded configuration files.

## Tools Used:
- Gobuster
- WhatWeb
- Nmap
- Curl
- Nikto

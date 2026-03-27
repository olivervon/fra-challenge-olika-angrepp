# FRA challenge: Olika angrepp

## Navigation

[00-initial-observation](/00-initial-observation/README.md)  
[01-attack-1-credential-brute-force](/01-attack-1-credential-brute-force/README.md)  
[02-attack-2-remote-command-execution](/02-attack-2-remote-command-execution/README.md)  
[03-attack-3-sql-injection](/03-attack-3-sql-injection/README.md)  
[04-attack-4-file-upload](/04-attack-4-file-upload/README.md)  

## Executive Summary

The analysis of the provided network capture revealed a multi-stage attack carried out within an internal environment.

Key findings:
- A credential brute force / credential stuffing attack targeting a web application login
- Exploitation of a file upload vulnerability to deploy a malicious PHP web shell
- Remote command execution through the uploaded web shell
- SQL injection attacks used to enumerate and extract sensitive database information

The attacker demonstrates a complete attack chain:
1. Initial access attempts via brute force
2. Exploitation of application vulnerabilities
3. Deployment of a web shell
4. Execution of system commands and data exfiltration

Overall severity: Critical

## Attack Timeline

1. Initial brute force activity observed
   - Multiple login attempts using different usernames and passwords
   - Credentials transmitted via HTTP GET requests

2. Continued authentication attempts
   - No clear indication of successful login from responses

3. File upload activity detected
   - HTTP POST request to /applications/app2/
   - multipart/form-data indicating file upload

4. Web shell access observed
   - Access to /hackable/uploads/funny.php
   - Execution of system commands via "cmd" parameter

5. Remote command execution
   - Commands executed: id, ls -la, uname -r, arch
   - Sensitive files accessed: /etc/passwd, /etc/shadow

6. SQL injection activity detected
   - UNION SELECT payloads in /applications/app3/
   - Enumeration of database structure
   - Extraction of user credentials from database

## Attack Chain Analysis

The observed activity follows a typical multi-stage attack:

- Initial Access → Brute force / credential stuffing
- Execution → Web shell command execution
- Persistence → Uploaded PHP web shell
- Credential Access → Extraction of /etc/shadow and database credentials
- Discovery → System and database enumeration

The attacker demonstrates systematic progression from initial access attempts to full system compromise.

## Answers to Key Questions

### What happened?
An attacker performed multiple attacks against a web application, including brute force login attempts, SQL injection, file upload exploitation, and remote command execution.

### What vulnerability was exploited?
- Weak authentication mechanisms (no rate limiting)
- SQL injection vulnerability (unsanitized input)
- File upload vulnerability allowing execution of malicious files

### What information was accessed?
- System information (uname, architecture)
- User data from /etc/passwd
- Password hashes from /etc/shadow
- Database contents, including user credentials

### What commands were executed?
- id
- ls -la
- uname -r
- arch
- cat /etc/passwd
- cat /etc/shadow

### Were any commands unsuccessful?
No clear evidence of failed commands was observed in the captured traffic.

### Severity assessment
Critical

### Worst-case scenario
- Full system compromise
- Credential theft and reuse
- Persistent backdoor via web shell
- Lateral movement within the network

## Analyst Notes

The activity appears to be conducted within a controlled or simulated environment, but reflects realistic attacker techniques and methodologies.

The analysis demonstrates how multiple vulnerabilities can be chained together to achieve full system compromise.

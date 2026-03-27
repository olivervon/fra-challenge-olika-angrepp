<- [Root README](/README.md)  
<- [01-attack-1-credential-brute-force](/01-attack-1-credential-brute-force/README.md)  
[03-attack-3-sql-injection](/03-attack-3-sql-injection/README.md) ->

# Attack 2 – Remote Command Execution (Web Shell)

After filtering out login attempts, new HTTP requests were identified targeting a file:

/hackable/uploads/funny.php

![RCE Activity](/screenshots/08_attack2_rce.png)

Observed parameters:
- cmd=id
- cmd=ls -la
- cmd=cat /etc/passwd
- cmd=cat /etc/shadow
- cmd=uname -r
- cmd=arch

Analysis:
- The attacker is executing system commands via a web-accessible script
- This indicates the presence of a web shell

Conclusion:
- The attacker has achieved remote code execution on the target system

## Vulnerability

- File upload vulnerability (malicious PHP file)
- Lack of input validation
- Remote command execution via unsanitized input

## Severity

Critical

The attacker is able to execute arbitrary system commands and access sensitive system files, including password hashes.

## MITRE ATT&CK

- T1059 – Command and Scripting Interpreter
- T1105 – Ingress Tool Transfer
- T1003 – OS Credential Dumping

<- [Root README](/README.md)  
<- [01-attack-1-credential-brute-force](/01-attack-1-credential-brute-force/README.md)  
[03-attack-3-sql-injection](/03-attack-3-sql-injection/README.md) ->

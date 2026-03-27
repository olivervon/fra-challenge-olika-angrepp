<- [Root README](/README.md)  
<- [02-attack-2-remote-command-execution](/02-attack-2-remote-command-execution/README.md)  
[04-attack-4-file-upload](/04-attack-4-file-upload/README.md) ->

# Attack 3 – SQL Injection (Data Exfiltration)

Multiple HTTP requests containing SQL injection payloads were identified targeting the "id" parameter in:

/applications/app3/

![SQL Injection](screenshots/09_sqli_requests.png)

Observed payloads include:

- UNION SELECT statements
- Enumeration of database metadata via information_schema
- Extraction of sensitive data from application tables

Examples:
- union all select 1,2,3
- union all select @@version
- union all select table_name from information_schema.tables
- union all select column_name from information_schema.columns
- union all select user,password from users

Analysis:
- The attacker successfully injects SQL queries into the application
- The database structure is enumerated
- Sensitive data, including user credentials, is extracted

Conclusion:
- The application is vulnerable to SQL injection
- The attacker has successfully accessed database contents

## Vulnerability

- SQL injection due to lack of input sanitization
- User-controlled input is directly used in SQL queries
- No parameterized queries or prepared statements

## Severity

Critical

The attacker is able to extract sensitive database information, including user credentials.

Worst-case scenario:
- Full database compromise
- Credential reuse across systems
- Privilege escalation and lateral movement

## MITRE ATT&CK

- T1190 – Exploit Public-Facing Application
- T1005 – Data from Local System
- T1552 – Unsecured Credentials

<- [Root README](/README.md)  
<- [02-attack-2-remote-command-execution](/02-attack-2-remote-command-execution/README.md)  
[04-attack-4-file-upload](/04-attack-4-file-upload/README.md) ->

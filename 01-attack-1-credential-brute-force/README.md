<- [Root README](/00-initial-observation/README.md)  
<- [00-initial-observation](/00-initial-observation/README.md)  
[03-attack-3-sql-injection](/03-attack-3-sql-injection/README.md) ->

# Attack 1 – Credential Brute Force / Credential Stuffing

Multiple HTTP GET requests were observed targeting the login functionality.

![Brute Force Attempts](/screenshots/06_bruteforce_attempts.png)

Observed patterns:
- Multiple usernames were tested
- Multiple passwords were attempted for each username
- Credentials were transmitted in cleartext via HTTP

Analysis:
- The attacker is performing a credential stuffing / brute force attack
- The attack targets multiple accounts rather than a single user
- This increases the likelihood of successful compromise

Note:
- All responses returned HTTP 200 OK, indicating that status codes alone cannot determine authentication success
- Response content must be analyzed for confirmation

## TCP Stream Analysis

Two representative login attempts were analyzed:

- password=123456
- password=princesa

![TCP Stream](screenshots/07_tcp_stream.png)

Findings:
- Both responses returned HTTP/1.1 200 OK
- Content-Length was identical (677 bytes)
- Response structure and content appeared consistent

Conclusion:
- The application returns identical responses regardless of authentication success or failure
- It is not possible to determine successful login attempts based on response analysis alone

## Vulnerability

The application contains multiple security weaknesses:

- Credentials are transmitted via HTTP GET parameters, exposing sensitive information in cleartext
- No rate limiting or account lockout mechanism is in place, allowing unlimited login attempts
- Weak passwords are accepted (e.g., common passwords such as "123456")
- The application provides identical responses for all login attempts, making it difficult to detect successful authentication

These weaknesses make the application highly susceptible to brute force and credential stuffing attacks.

## Severity

Medium to High

The attack demonstrates that the application is vulnerable to brute force and credential stuffing attacks.

Although successful authentication could not be definitively confirmed, the lack of protective mechanisms and the use of weak credentials significantly increase the risk of account compromise.

Worst-case scenario:
- Unauthorized access to user accounts
- Potential administrative access
- Further exploitation of the system

The absence of rate limiting and secure authentication mechanisms allows attackers to systematically attempt credential combinations without restriction.

<- [Root README](/00-initial-observation/README.md)  
<- [00-initial-observation](/00-initial-observation/README.md)  
[03-attack-3-sql-injection](/03-attack-3-sql-injection/README.md) ->

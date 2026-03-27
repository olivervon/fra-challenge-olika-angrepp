<- [Root README](/README.md)  
<- [03-attack-3-sql-injection](/03-attack-3-sql-injection/README.md)

# Attack 4 – File Upload (Web Shell Deployment)

A POST request to the following endpoint was identified:

/applications/app2/

![File Upload](screenshots/10_file_upload.png)

The request contains multipart/form-data, indicating a file upload operation.

Although the packet is marked as malformed, key indicators confirm file upload activity:
- Use of HTTP POST method
- multipart/form-data content type
- Presence of MIME boundaries

Shortly after this request, a PHP file is accessed:

/hackable/uploads/funny.php

Analysis:
- The attacker likely uploaded a malicious PHP file via this request
- The uploaded file is later used to execute system commands

Conclusion:
- The attacker successfully deployed a web shell on the server
- This enabled remote command execution and full system interaction

## Vulnerability

- Unrestricted file upload
- Lack of server-side validation
- Executable files allowed in upload directory

Even though the upload content is not fully visible, the sequence of events provides strong evidence of successful web shell deployment.

<- [Root README](/README.md)  
<- [03-attack-3-sql-injection](/03-attack-3-sql-injection/README.md)

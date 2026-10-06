# Ideas
## Sanitization of User Input
- Hvad skal man sikre sig imod når man modtager input fra brugeren?
  - A tags skal tilføjes rel="noopener noreferrer" for at forhindre reverse tabnabbing.
  - HTML input skal sanitizes for at forhindre XSS-angreb.
  - 

## JWT Handling
- Find vulnerabilities in current JWT handling in localStorage. 
- Try with cookies
- Try deploying on a server and steal the token from the network tab.

## OWASP Top 10
### A01: Broken Access Control
- Common access control vulnerabilities include:
  - Violation of the principle of least privilege, commonly known as deny by default
  - Bypassing access control checks by modifying the URL (parameter tampering)
  - Permitting viewing or editing someone else's account by providing its unique identifier
  - An accessible API with missing access controls for POST, PUT, and DELETE
  - Admin access to user accounts or data without proper authorization
  - Metadata manipulation, such as replaying or tampering with a (JWT) 
  - CORS misconfiguration
  - Force browsing (guessing URLs) to authenticated pages

#### Topics:
- What are the common access control techniques?
- How are they implemented in web applications?
- How can they be bypassed?
- How are roles commonly implemented in web applications?
- What are the common mistakes developers make when implementing access control?

#### Steps to secure access control:
- Deny by default
- 

#### CWE-200: Exposure of Sensitive Information to an Unauthorized Actor 
#### CWE-201: Exposure of Sensitive Information Through Sent Data, 
#### CWE-918 Server-Side Request Forgery (SSRF) 
#### CWE-352: Cross-Site Request Forgery (CSRF)


# What is the process of making a web application secure?
1. 
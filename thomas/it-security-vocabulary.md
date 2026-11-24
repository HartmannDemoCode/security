# Vocabulary for IT Security

A practical glossary for learning IT security, working through TryHackMe labs, and discussing the security of web applications. Examples frequently use a React frontend, a Java REST API, and a database.

Entries explain the meaning, why the concept matters, and an example or important distinction. Definitions are introductory explanations rather than quotations from a particular standard.

## CIA Triad

- **Confidentiality:** Information is available only to authorized people and systems. Example: a student can see their own exam result, but not other students’ results.
- **Integrity:** Information and systems are protected against unauthorized modification or destruction. Example: an exam result cannot be changed without permission and an accountable process.
- **Availability:** Authorized users can access information and services when needed. Example: the exam portal remains usable during a busy submission period.
- A single incident can affect several principles: ransomware can disrupt availability, alter data, and expose confidential information.

## Asset

- Anything valuable that needs protection, including data, software, hardware, accounts, services, and organizational reputation.
- Identifying assets helps determine what security controls are needed and which incidents would matter most.
- **Example:** A database, its backups, and the credentials used to access it are separate assets with related risks.

## Authentication

- Verifying the identity of a user, device, or service: **“Who are you?”**
- Methods include passwords, security keys, passkeys, certificates, and biometrics used within an authentication system.
- **Example:** A Java API validates a user’s login credentials. Successfully logging in does not automatically give that user access to every resource.

## Authorization

- Deciding whether an identity may perform a particular action on a particular resource: **“What are you allowed to do?”**
- Decisions can depend on roles, resource ownership, attributes, and context.
- **Example:** Both Alice and Bob can log in, but Alice is not authorized to delete Bob’s account. Hiding a delete button in React does not enforce this rule; the API must check permission.

## Access Control

- The policies and mechanisms that enforce who or what can access resources and perform actions.
- **RBAC — Role-Based Access Control:** Permissions are assigned to roles such as administrator, teacher, or student.
- **ABAC — Attribute-Based Access Control:** Decisions use attributes such as department, resource classification, or time of access.
- **DAC — Discretionary Access Control:** An owner can decide who receives access. **MAC — Mandatory Access Control:** Centrally enforced labels and policies govern access, and ordinary owners cannot override them.

## Identity and Access Management (IAM)

- Managing identities, credentials, roles, and permissions throughout their lifecycle.
- Includes creating accounts, changing access when responsibilities change, and removing access when someone leaves.
- **Example:** Disabling an employee’s account and revoking their active sessions and access keys during offboarding.

## Multi-Factor Authentication (MFA)

- Authentication using at least two different factor categories: something you **know**, **have**, or **are**.
- A password plus a physical security key can provide two factors. A password plus a security question is still only one factor category.
- **2FA** means exactly two factors. Different MFA methods offer different protection; manually entered one-time codes can still be stolen through phishing.

## Principle of Least Privilege

- Give each user, process, or service only the permissions needed for its task, for only as long as needed.
- Reduces the harm caused by mistakes or compromised accounts.
- **Example:** A REST API uses a restricted database account rather than a database administrator account.

## Deny by Default

- Reject access unless an explicit rule permits it.
- Makes new endpoints and resources less likely to become accessible accidentally.
- **Example:** A newly added administrative endpoint requires explicit authorization before anyone can use it.

## Defense in Depth

- Use multiple security controls so that one failure does not expose the entire system.
- Controls should address different failure modes rather than simply repeat the same check.
- **Example:** Combine API authorization, restricted database permissions, network restrictions, and security monitoring.

## Zero Trust

- An approach that avoids granting implicit trust solely because a user or device is on an internal network.
- Access decisions use identity, device and resource context, least privilege, and reevaluation as appropriate.
- **Example:** Connecting to the office network does not automatically grant access to production administration tools.

## Vulnerability

- A weakness in software, configuration, design, or procedures that a threat can exploit or trigger.
- Not every bug is a security vulnerability; the bug must have a security consequence.
- **Example:** An API endpoint returns any user’s private records without checking ownership.

## Threat

- A circumstance or event with the potential to harm an asset or organization.
- Threats may be deliberate, accidental, or environmental. Malware attacks, accidental deletion, and power failures can all threaten a system.
- **Distinction:** A vulnerability is a weakness; a threat is something that could cause harm through that weakness or otherwise affect the asset.

## Threat Actor

- A person or group that carries out or attempts harmful activity.
- Examples include criminals, malicious insiders, and state-sponsored groups. Their motives and capabilities differ.
- **Example:** A criminal group targets customer accounts for financial fraud.

## Risk

- The possibility of adverse consequences, considered in terms of their likelihood and impact.
- **Likelihood × impact** is a useful introductory model, not a universally precise formula. Exposure, existing controls, uncertainty, and business context also matter.
- **Example:** A public database containing personal information may create greater risk than an isolated test database containing fictional records.

## Risk Treatment and Residual Risk

- Risk treatment means deciding how to address risk: reduce it, avoid the activity, share or transfer some consequences, or accept it.
- **Residual risk** is what remains after controls and other treatments are applied.
- **Example:** Backups reduce the impact of data loss, but cannot eliminate downtime or the consequences of stolen data. Insurance does not remove a technical vulnerability.

## Security Control

- A measure intended to protect assets or reduce risk. Controls can be technical, administrative, or physical.
- Controls may prevent incidents, detect them, or support correction and recovery.
- **Example:** Authorization checks are preventive controls; login alerts are detective controls; recovery procedures support restoration.

## Exploit

- Code, input, or a sequence of actions that takes advantage of a vulnerability to produce an unintended security effect.
- An exploit does not have to be a complicated program or install malware.
- **Example:** Changing a resource identifier to retrieve another user’s private record can exploit a missing authorization check.

## Payload

- In an attack context, the input, code, or action delivered to produce a particular effect.
- The exploit is the mechanism that takes advantage of a weakness; the payload is what is delivered or executed through that mechanism. The boundary can depend on context.
- **Example:** In an XSS demonstration, a script supplied through a vulnerable input field is a payload.

## Attack Vector

- A route or method by which an attacker reaches a target or carries out an attack.
- Examples include phishing email, an exposed API, a malicious dependency, or a compromised remote-access account.
- **Example:** An email attachment provides a route for malware delivery.

## Attack Surface

- The collection of interfaces, entry points, and exposed components through which a system could be attacked.
- Includes endpoints, network services, user accounts, dependencies, and administrative interfaces.
- **Example:** Removing an unused public management endpoint reduces attack surface.

## Trust Boundary

- A point where data or actions move between different levels of trust or control.
- Crossing a boundary should trigger appropriate validation and security decisions.
- **Example:** Data arriving from a React application crosses into the backend’s trust boundary. The API cannot assume it was produced by the intended frontend or passed its validation.

## Threat Modelling

- A structured way to identify assets, data flows, trust boundaries, possible threats, and suitable controls.
- Often asks: What are we building? What could go wrong? What will we do about it? Did the controls work?
- **Example:** Diagram browser → API → database, then examine where users might impersonate others, change data, or gain unauthorized access.

## Malware

- Software intentionally designed to perform harmful or unauthorized actions.
- **Virus:** Attaches to a host file or program and spreads when that host runs. **Worm:** Can propagate between systems without needing to attach to a host file. **Trojan:** Presents itself as legitimate or desirable software while hiding malicious behavior.
- **Ransomware:** Used for extortion, often by encrypting files and sometimes stealing data. **Spyware:** Secretly gathers information. **Rootkit:** Hides malicious activity or privileged access.
- Malware may steal data, disrupt services, install additional tools, or provide persistent access. These are possible behaviors, not characteristics shared by all malware.

## Backdoor

- A mechanism that bypasses normal authentication or other security controls to provide access.
- It may be deliberately planted in software or installed after a compromise.
- **Example:** An attacker adds an undocumented administrative access mechanism that remains after the original weakness is patched.

## Botnet

- A collection of compromised devices controlled by an operator, commonly through command-and-control infrastructure.
- Botnets can distribute spam, perform fraud, or generate distributed denial-of-service traffic.
- **Example:** Thousands of compromised routers send traffic toward the same target.

## Social Engineering

- Manipulating people into disclosing information or performing actions that undermine security.
- Exploits trust, urgency, fear, authority, or curiosity rather than relying only on a software flaw.
- **Example:** Someone impersonates IT support and asks an employee to approve an unexpected login request.

## Phishing

- Deceptive messages or websites intended to steal information, deliver malware, or prompt harmful actions.
- **Spear phishing** targets a specific person or organization; **smishing** uses text messages; **vishing** uses voice communication.
- **Example:** A fake account-expiry email leads to a counterfeit login page.

## Brute Force, Password Spraying, and Credential Stuffing

- **Brute force:** Tries many possible passwords or keys, often against one target.
- **Password spraying:** Tries a small set of likely passwords across many accounts.
- **Credential stuffing:** Reuses username/password pairs stolen from another service.
- These differ in technique and detection patterns. MFA, breached-password checks, rate limits, and monitoring address different parts of the problem.

## Reconnaissance and Enumeration

- **Reconnaissance:** Gathering information about a target, such as domains, technologies, and exposed services.
- **Enumeration:** Systematically identifying more specific details, such as available endpoints, services, or accounts.
- **Example:** In a lab, identify exposed services and then determine which application routes exist. Passive reconnaissance uses existing information; active reconnaissance interacts with the target.

## Privilege Escalation

- Obtaining permissions or access beyond those originally granted.
- **Vertical escalation:** Moving to a more privileged role, such as administrator. **Horizontal escalation:** Accessing another user’s resources at a similar privilege level.
- **Example:** A normal user changes their own role through an insufficiently protected API endpoint.

## Lateral Movement

- Moving from one compromised account, service, or machine to another within an environment.
- Often exploits reused credentials, excessive permissions, or insufficient network separation.
- **Example:** Credentials found on a compromised application server allow access to a separate database server.

## Persistence

- Techniques that allow an attacker to retain access despite interruptions such as restarts or credential changes.
- May involve unauthorized accounts, scheduled tasks, or modified application components.
- **Example:** A malicious scheduled task restarts an attacker-controlled process after reboot.

## Data Exfiltration

- Unauthorized transfer of data out of a system or organization.
- Can occur through ordinary channels such as HTTPS, email, or cloud storage.
- **Example:** A compromised application sends customer records to an external server.

## Denial of Service (DoS) and Distributed Denial of Service (DDoS)

- **DoS:** Makes a service unavailable or severely degraded by exhausting resources or triggering failures.
- **DDoS:** Uses multiple sources to carry out a denial-of-service attack.
- **Example:** Expensive requests consume all application worker threads. Large traffic volume is only one possible mechanism.

## Offensive Security

- Authorized activities that use an attacker’s perspective to discover weaknesses and demonstrate their consequences.
- Includes penetration testing, adversary simulation, and exploit research in controlled contexts.
- **Example:** Test a lab API to determine whether a normal user can access administrative functions.

## Defensive Security

- Activities that prevent, detect, respond to, and recover from threats and incidents.
- Includes hardening, monitoring, secure design, incident response, and recovery planning.
- **Example:** Restrict an API’s permissions, detect suspicious access attempts, and prepare a recovery procedure.

## Penetration Testing

- An authorized assessment that attempts to exploit weaknesses within an agreed scope to demonstrate security impact.
- Includes planning, testing, evidence collection, reporting, and often retesting after remediation.
- **Distinction:** A vulnerability scan identifies potential weaknesses; a penetration test investigates exploitability and may combine several weaknesses. Neither proves that no other vulnerabilities exist.

## Scope and Rules of Engagement

- **Scope:** Defines the systems, accounts, environments, and activities included in an assessment.
- **Rules of engagement:** Define operational conditions such as test windows, prohibited actions, communication, and stop conditions.
- **Example:** A test allows activity against a staging API but excludes production data and denial-of-service testing.

## Red Teaming

- An authorized, objective-driven exercise that simulates adversary behavior to assess an organization’s defenses.
- Examines how people, processes, and technology prevent, detect, and respond to realistic attack sequences.
- **Distinction:** A red-team exercise is usually focused on achieving agreed objectives and testing defenses rather than producing the broadest possible list of vulnerabilities.

## Blue Teaming

- Work focused on defending systems and improving the ability to prevent, detect, investigate, and respond to attacks.
- Includes detection engineering, monitoring, hardening, and incident handling.
- **Example:** A blue team investigates simulated attacker activity and improves alerts when parts of the exercise go undetected.

## Purple Teaming

- A collaborative approach in which offensive and defensive practitioners work together to improve defenses.
- Often involves executing a technique, checking the resulting telemetry and alerts, making improvements, and repeating the test.
- It does not require a separate permanent “purple team.”

## Security Event, Alert, and Incident

- **Event:** An observable occurrence, such as a login attempt or configuration change.
- **Alert:** A notification that an event or pattern may need investigation.
- **Incident:** An occurrence that actually or potentially jeopardizes security or violates relevant security policies.
- **Example:** A failed login is an event; many suspicious failures may trigger an alert; confirmed unauthorized account access is an incident.

## Incident Response

- Coordinated activities to detect, assess, contain, and resolve security incidents and support recovery.
- Requires preparation, clear responsibilities, communication, evidence handling, and lessons learned.
- **Example:** After an account compromise, revoke sessions, contain affected systems, investigate access, address the cause, and restore normal operation safely.

## Digital Forensics

- Collecting, preserving, examining, and interpreting digital evidence in a disciplined manner.
- Evidence may include disk contents, memory, logs, and network traffic. Documenting handling helps establish its reliability.
- **Example:** Preserve relevant logs and a server snapshot before changes destroy information needed to investigate an intrusion.

## Threat Intelligence

- Analyzed information about threats that supports security decisions.
- May describe attacker motivations, targeting, techniques, infrastructure, and relevant defensive actions.
- **Distinction:** A raw list of suspicious addresses is data; evaluating its reliability and relevance makes it more useful as intelligence.

## Indicators of Compromise (IoCs)

- Observable artifacts that may indicate malicious activity or a compromise.
- Examples include malicious file hashes, suspicious domains, or unexpected persistence mechanisms.
- **Limitation:** An indicator requires context; a shared IP address alone may not establish malicious activity.

## Tactics, Techniques, and Procedures (TTPs)

- **Tactics:** An attacker’s goals, such as gaining initial access or collecting credentials.
- **Techniques:** Methods used to achieve those goals. **Procedures:** Specific ways a technique is implemented in practice.
- Understanding behavior can help detect attacks even when individual file hashes or network addresses change.

## Security Information and Event Management (SIEM)

- A system that collects and analyzes security-relevant logs and events from multiple sources.
- Supports searching, correlation, alerting, investigation, and reporting.
- **Example:** Correlate an unusual login, a new administrator account, and a large data export. Its value depends on available telemetry and well-maintained detection rules.

## Security Operations Center (SOC)

- A team or organizational capability responsible for monitoring, investigating, and responding to security threats.
- Commonly uses SIEM, endpoint tools, threat intelligence, and incident-response procedures.
- **Distinction:** A SIEM is a tool; a SOC combines people, processes, and tools.

## Endpoint Detection and Response (EDR)

- Technology that monitors endpoints such as workstations and servers for suspicious behavior and supports investigation and response.
- Capabilities may include process histories, alerting, isolation, and stopping malicious processes.
- **Example:** Detect a suspicious process chain and isolate the affected laptop while investigating.

## Logging, Monitoring, and Alerting

- **Logging:** Recording what happened. **Monitoring:** Observing and analyzing system behavior. **Alerting:** Notifying someone when defined conditions occur.
- Useful security logs identify relevant actions, outcomes, timestamps, and actors without unnecessarily exposing secrets or personal data.
- **Example:** Log failed authorization attempts, but do not log passwords or complete bearer tokens.

## False Positives and False Negatives

- **False positive:** Harmless activity is incorrectly identified as malicious or vulnerable.
- **False negative:** Malicious activity or a real vulnerability is missed.
- **Example:** A vulnerability scanner may flag a patched component incorrectly, or fail to detect a business-logic flaw. Findings need validation.

## Hardening

- Reducing exposure by configuring systems securely and removing unnecessary capabilities.
- Includes disabling unused services, limiting permissions, protecting administrative access, and choosing secure defaults.
- **Example:** Run an application container without root privileges and expose only required network ports.

## Vulnerability Management

- An ongoing process of identifying, assessing, prioritizing, fixing or mitigating, and tracking vulnerabilities.
- Prioritization considers exploitability, exposure, asset importance, and available controls, not just scanner scores.
- **Example:** Prioritize an actively exploited flaw in a public service, apply a fix, and verify that the affected deployment is actually updated.

## Patch Management

- Managing updates to operating systems, applications, dependencies, and other components.
- Includes identifying affected assets, evaluating updates, deploying them, and checking the result.
- **Distinction:** Patching is one part of vulnerability management. Design flaws and missing authorization checks often require code or architecture changes.

## CVE, CWE, and CVSS

- **CVE — Common Vulnerabilities and Exposures:** Identifies a publicly disclosed vulnerability, such as a flaw in a specific product.
- **CWE — Common Weakness Enumeration:** Classifies types of weakness, such as SQL injection or missing authorization.
- **CVSS — Common Vulnerability Scoring System:** Describes technical severity through defined metrics and scores. It is not a complete measure of business risk.
- **Memory aid:** CVE identifies a particular vulnerability; CWE describes a weakness category; CVSS helps communicate severity.

## Zero-Day Vulnerability

- Commonly, a vulnerability that is not yet known to the responsible vendor or defenders when it is discovered or exploited; usage varies with context.
- A **zero-day exploit** takes advantage of such a vulnerability. A known vulnerability awaiting an organization’s patch deployment is not automatically a zero-day.
- Monitoring, isolation, and least privilege remain useful when a specific patch or signature is unavailable.

## Security Analysis

- Examining systems, designs, code, or evidence to understand security properties, weaknesses, and risk.
- Can involve reviewing logs, inspecting authentication flows, or evaluating whether controls address a threat.
- **Example:** Analyze whether a JWT validation implementation checks signatures and the required claims.

## Security Auditing

- A systematic assessment against defined policies, requirements, or standards, supported by evidence.
- May examine technical configuration, procedures, access records, and governance.
- **Distinction:** Passing an audit shows conformity with the assessed criteria and scope; it does not guarantee that a system cannot be compromised.

## Security Monitoring

- Ongoing observation and analysis of security-relevant activity to identify suspicious behavior and control failures.
- Requires useful telemetry, appropriate detection logic, and a process for acting on findings.
- **Example:** Detect repeated access-denied responses across many resource IDs and investigate possible unauthorized enumeration.

## Security Awareness Training

- Helping people recognize security risks, make informed decisions, and report concerns.
- Topics include phishing, password practices, sensitive-data handling, and incident reporting.
- Effective training complements usable technical controls and clear procedures; it should not make users solely responsible for preventing attacks.

## Encryption

- Transforming readable data into ciphertext using a cryptographic algorithm and key to protect confidentiality.
- **Symmetric encryption** uses a shared secret key. **Asymmetric cryptography** uses a related public/private key pair for operations such as encryption or signatures, depending on the algorithm.
- **Distinction:** Encryption alone does not necessarily detect tampering; authenticated encryption also protects integrity. Key protection is essential.

## Hashing

- Applying a function to produce a digest of input data. Cryptographic hash functions are designed to make reversing the digest and finding collisions computationally infeasible.
- Hashing is not encryption and has no decryption key. Low-entropy inputs can still be discovered by guessing and comparing hashes.
- **Example:** Compare a file’s hash with a trusted expected value. If an attacker can replace both the file and its advertised hash, that comparison does not establish authenticity.

## Password Hashing and Salting

- Passwords should be stored using a purpose-built password-hashing function with suitable parameters, rather than plaintext, reversible encryption, or a fast general-purpose hash alone.
- A **salt** is a unique random value stored with each password hash. It prevents identical passwords from automatically producing identical stored results and undermines precomputed lookup tables.
- Salts are not secret. Password-hashing algorithms such as Argon2id, bcrypt, and scrypt are designed to make guessing more expensive; choice and configuration depend on the environment and current guidance.

## Encoding

- Representing data in a format suitable for storage or transmission, such as Base64 or URL encoding.
- Encoding does not provide confidentiality and generally requires no secret to reverse.
- **Example:** A Base64-encoded password remains readable by anyone who decodes it.

## Digital Signature

- A cryptographic mechanism that uses a private key to sign data and a corresponding public key to verify the signature.
- Helps establish integrity and that the signer controlled the relevant private key. Associating that key with a person or organization requires additional trust mechanisms.
- **Distinction:** Signing does not hide the message contents. Non-repudiation also depends on identity, key custody, and surrounding procedures.

## TLS, HTTPS, and Certificates

- **TLS — Transport Layer Security:** Protects communication in transit, including confidentiality, integrity, and peer authentication as configured.
- **HTTPS:** HTTP carried over TLS. Server certificates help the client validate the server’s identity through an appropriate trust chain and hostname check.
- **Limitation:** HTTPS does not prove that a website is honest or fix application vulnerabilities. Phishing websites can also use HTTPS.

## Session Management

- Maintaining and controlling a user’s authenticated interaction across multiple requests.
- Includes generating session identifiers, expiry, rotation where appropriate, logout, and revocation.
- **Example:** A stolen session identifier may allow account impersonation even if the attacker does not know the password.

## Cookies and Security Attributes

- Cookies are browser-managed values sent with requests according to cookie rules. They often carry session identifiers.
- **Secure:** Restricts transmission to secure connections. **HttpOnly:** Prevents direct access through browser JavaScript. **SameSite:** Restricts sending in certain cross-site contexts.
- These attributes address different risks. HttpOnly reduces direct cookie theft through JavaScript but does not prevent an XSS payload from acting through the victim’s browser.

## JSON Web Token (JWT)

- A format for representing claims, such as identity, issuer, audience, and expiry. JWTs can be signed or encrypted; a common signed JWT is readable by its holder.
- Verification must enforce the intended cryptographic algorithm and key, as well as applicable claims and authorization rules.
- **Distinction:** Decoding a token is not verifying it. A valid token also does not automatically authorize every requested action.

## Secrets Management

- Secure handling of credentials, API keys, signing keys, and other sensitive configuration throughout their lifecycle.
- Includes controlled storage, access restrictions, rotation, revocation, and avoiding exposure in source code or logs.
- **Example:** If a database password is committed to Git, remove the exposure and rotate the credential; deleting it from the latest commit does not make it secret again.

## SQL Injection

- Occurs when untrusted input changes the intended structure or meaning of a SQL statement.
- Prevent it by separating SQL code from data through parameterized queries. Dynamically chosen identifiers require separate design and validation.
- **Example:** Concatenating a request parameter into a SQL or JPQL query can introduce injection. Using an ORM does not automatically make all query construction safe.

## Command Injection

- Occurs when attacker-controlled input changes a command executed by the operating system.
- Avoid invoking a shell where possible; use safer APIs and strictly control executable choices and arguments.
- **Example:** A server-side file-conversion feature constructs a shell command from an uploaded filename.

## Cross-Site Scripting (XSS)

- A flaw that allows attacker-controlled script to execute in a website’s browser context.
- **Stored XSS** persists in application data; **reflected XSS** arrives through a request and response; **DOM-based XSS** involves unsafe client-side handling of data. These labels can overlap.
- Use safe rendering, context-appropriate output encoding, and suitable sanitization when HTML must be accepted. React escapes ordinary rendered strings, but unsafe HTML insertion and other dangerous APIs can still introduce XSS.

## Cross-Site Request Forgery (CSRF)

- Tricks a browser into sending an unwanted request to a site where the victim has an authenticated session, typically relying on automatically attached credentials such as cookies.
- Defenses include properly implemented anti-CSRF tokens, origin checks, and suitable SameSite cookie settings, depending on the application.
- **Distinction:** XSS executes code in the target website’s context; CSRF exploits the browser’s ability to send authenticated requests from another context.

## Server-Side Request Forgery (SSRF)

- Occurs when an attacker can influence a server into sending unintended requests, potentially reaching internal or otherwise restricted destinations.
- **Example:** A URL-preview feature can be directed toward a private administrative service.
- Defenses include constraining destinations and protocols, controlling redirects, and restricting outbound network access. Checking only the initial URL string may be insufficient.

## Insecure Direct Object Reference (IDOR) and Broken Object-Level Authorization (BOLA)

- An authorization failure that lets a user access or modify an object they should not control, often by changing an identifier.
- **Example:** Changing `/api/orders/42` to `/api/orders/43` exposes another customer’s order because the API checks login but not access to that order.
- Enforce authorization for the requested object and action. Replacing sequential IDs with UUIDs does not replace permission checks.

## Path Traversal

- A flaw that lets attacker-controlled paths access files outside an intended directory.
- **Example:** A download endpoint accepts a path that resolves to a sensitive server configuration file.
- Prefer server-controlled file identifiers and enforce safe path resolution within the permitted location rather than relying only on removing suspicious substrings.

## Security Misconfiguration

- An insecure setting or deployment choice that creates unnecessary exposure.
- Examples include default credentials, public administrative interfaces, excessive permissions, and production debug pages.
- **Example:** Detailed error responses reveal internal file paths or database information to unauthenticated users.

## Input Validation, Output Encoding, and Sanitization

- **Input validation:** Checks whether data meets defined requirements, such as type, length, range, and allowed values. Enforce security-relevant checks on the server.
- **Output encoding:** Represents data safely for its destination context, such as HTML text. **Sanitization:** Removes or transforms unsafe content, such as disallowed HTML elements.
- They solve different problems. Generic “cleaning” is not a substitute for parameterized queries, correct authorization, or context-appropriate rendering.

## Same-Origin Policy (SOP) and Cross-Origin Resource Sharing (CORS)

- **SOP:** A browser security model that restricts interaction between different origins. An origin is generally identified by scheme, host, and port.
- **CORS:** An HTTP mechanism through which a server permits specified cross-origin browser access to responses; some requests also require a successful preflight.
- **Distinction:** CORS is not API authentication or authorization. Non-browser clients do not enforce browser CORS rules, and CORS alone is not a general CSRF defense.

## Content Security Policy (CSP)

- A browser-enforced policy that restricts sources and behaviors such as script execution or framing.
- A carefully designed policy can reduce the impact of some injection attacks.
- **Distinction:** CSP adds protection but does not replace safe rendering, encoding, and other measures that remove XSS vulnerabilities.

## Rate Limiting

- Restricting the number or cost of actions over a defined period.
- Helps address abusive automation, password guessing, resource exhaustion, and excessive API use.
- **Example:** Apply appropriate limits to login attempts, password-reset requests, and expensive searches. Limiting by IP address alone has limitations, including shared networks and distributed attackers.

## Firewall and Network Segmentation

- A **firewall** allows or blocks traffic according to defined rules.
- **Network segmentation** separates systems into zones and controls communication between them, reducing exposure and limiting movement after compromise.
- **Example:** A database accepts connections from the application environment rather than directly from the public internet.

## Intrusion Detection and Prevention Systems (IDS/IPS)

- **IDS:** Detects suspicious activity and generates information or alerts.
- **IPS:** Can also take action to block identified activity, typically while positioned in the traffic path.
- These systems depend on visibility and detection quality; encryption can limit what a network sensor can inspect without additional arrangements.

## Web Application Firewall (WAF)

- A control that inspects HTTP traffic to detect or block selected application-layer attack patterns.
- Can provide additional protection or a temporary mitigation while a vulnerability is fixed.
- **Distinction:** A WAF does not replace secure application code and may miss authorization or business-logic flaws.

## Secure Software Development Lifecycle (SSDLC) and DevSecOps

- **SSDLC:** Integrates security into requirements, design, implementation, testing, deployment, and maintenance.
- **DevSecOps:** Integrates security responsibilities and practices into collaborative development and operational workflows.
- **Example:** Add threat modelling, security review, dependency checks, protected deployment credentials, and security regression tests throughout a project rather than only before release.

## SAST, DAST, and Software Composition Analysis (SCA)

- **SAST — Static Application Security Testing:** Examines code or related artifacts without exercising the deployed application.
- **DAST — Dynamic Application Security Testing:** Tests a running application through its interfaces.
- **SCA — Software Composition Analysis:** Identifies third-party components and associated concerns, including known vulnerabilities and sometimes license issues.
- These approaches complement each other and manual review; none reliably finds every security weakness.

## Software Supply Chain Security

- Protecting dependencies, source repositories, build tools, workflows, package registries, and release artifacts from compromise or misuse.
- An application can be affected even when its own code has not intentionally changed.
- **Example:** A compromised dependency or CI workflow exposes a deployment secret. Reviewing dependencies and restricting build permissions reduce different parts of this risk.

## Backup, Disaster Recovery, RPO, and RTO

- **Backup:** A recoverable copy of data. **Disaster recovery:** Restoring systems and services after major disruption.
- **RPO — Recovery Point Objective:** The target maximum data loss expressed as time. **RTO — Recovery Time Objective:** The target time to restore service.
- **Example:** An RPO of one hour and an RTO of four hours mean aiming to lose no more than one hour of data and restore service within four hours. Backups must be protected and restoration tested.

## OWASP Top 10 and ASVS

- **OWASP Top 10:** An awareness resource describing major categories of web application security risk. Its categories depend on the edition.
- **ASVS — Application Security Verification Standard:** Provides more detailed security requirements for designing and assessing applications.
- **Distinction:** The Top 10 is a useful learning map, not an exhaustive testing checklist or proof that an application is secure.

## Security and Privacy

- **Security:** Protects information and systems against unauthorized access, modification, disruption, and related harms.
- **Privacy:** Concerns appropriate handling of information about people, including what is collected, why, how it is used, and how long it is retained.
- **Example:** A well-encrypted application can still create privacy problems if it collects unnecessary personal information or retains it indefinitely.

---

## Important Distinctions at a Glance

| Terms | Main difference |
| --- | --- |
| Authentication / Authorization | Verify identity / Decide permitted actions |
| Threat / Vulnerability / Risk | Potential harm source or event / Weakness / Likelihood and consequences in context |
| Exploit / Payload | Mechanism that uses a weakness / Input, code, or action delivered for an effect |
| Attack vector / Attack surface | A route for attack / The collection of exposed opportunities |
| Hashing / Encryption / Encoding | Produce a digest / Protect confidentiality using a key / Change representation |
| Vulnerability scan / Penetration test | Identify potential weaknesses / Investigate and demonstrate exploitation within scope |
| Penetration test / Red-team exercise | Assess scoped weaknesses / Simulate an adversary to evaluate defenses against objectives |
| Event / Alert / Incident | Observable occurrence / Notification to investigate / Actual or potential security compromise or policy violation |
| SIEM / SOC | A technology platform / An operational capability involving people, processes, and tools |
| CVE / CWE / CVSS | Particular disclosed vulnerability / Weakness category / Technical severity assessment |
| XSS / CSRF / SSRF | Browser executes attacker-controlled script / Browser sends an unwanted authenticated request / Server sends an unintended request |
| CORS / Authorization | Browser cross-origin response-access rules / Permission to act on resources |

## How to Learn the Vocabulary

For each term, try to answer these four questions without reading the definition:

1. What does it mean in my own words?
2. Can I give an example from a web application or TryHackMe lab?
3. Which related term could I confuse it with, and what is the difference?
4. How would I prevent, detect, investigate, or respond to it where applicable?

Start with CIA, authentication, authorization, vulnerabilities, threats, risk, least privilege, HTTP/TLS, and logging. Then connect attack terms to practical labs and corresponding defenses.

## Further Reading

These primary sources provide further detail and context. The explanations above are adapted for introductory learning and are not verbatim standards definitions.

- [NIST CSRC Glossary](https://csrc.nist.gov/glossary) — Definitions from multiple publications; check each definition’s original context.
- [NIST: Small Business Cybersecurity Glossary](https://www.nist.gov/itl/smallbusinesscyber/training/glossary) — Accessible introductions to common security concepts.
- [OWASP Developer Guide: Security Fundamentals](https://devguide.owasp.org/en/02-foundations/01-security-fundamentals/) — Application-security foundations.
- [OWASP Security Terminology Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Security_Terminology_Cheat_Sheet.html) — Distinctions between concepts such as validation, encoding, and sanitization.
- [MDN: Web Security Attacks](https://developer.mozilla.org/en-US/docs/Web/Security/Attacks) — Explanations of common browser and web attacks.

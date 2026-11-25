# Vocabulary

## Core Cybersecurity Concepts

- `Accountability` The ability to trace actions in a system to the user or process that performed them, often using audit logs. Authentication helps establish who performed an action.
- `Asymmetric Encryption` Encryption that uses a mathematically related public and private key. Data encrypted with the public key can be decrypted with the private key; the pair can also be used to create and verify digital signatures.
- `Attack Surface` All the points where an attacker could try to enter a system or access its data.
- `Availability` The assurance that systems and information are accessible to authorized users when needed.
- `Block Cipher` A symmetric encryption method that processes plaintext in fixed-size blocks, such as 128 bits. A mode of operation specifies how to securely encrypt messages longer than one block.
- `Blockchain` A ledger that groups records into blocks linked using cryptographic hashes. Copies may be shared across participants, and changing a past block can be detected; how records are validated depends on the blockchain.
- `Certificate Authority (CA)` An entity responsible for issuing, revoking, and distributing digital certificates. Examples include Verisign, GoDaddy, and Thawte.
- `CIA Triad` The three core information security principles: confidentiality, integrity, and availability.
- `Cipher` An algorithm used to encrypt and decrypt data. The key, rather than the algorithm, is kept secret.
- `Confidentiality` The principle that information should only be accessible to authorized people, systems, or processes.
- `Digital Signature` A cryptographic value created with a private key and checked with the matching public key. It helps verify that data has not changed and that it was signed by the holder of that private key; it does not hide the data.
- `Encryption` Converts readable data (plaintext) into ciphertext using an algorithm and a key. Someone with the appropriate key can decrypt it; encryption protects confidentiality but does not by itself prove who sent the data.
- `Hash` A one-way function that maps data of any length to a fixed-size value. Comparing hashes can help detect changes, but different inputs can produce the same hash, and a hash alone does not prove who created the data.
- `Key Management` The secure generation, storage, distribution, use, rotation, and retirement of cryptographic keys.
- `Message Digest` The fixed-size output of a hash function. A recipient can compare the digest of received data with a trusted digest to check for changes; digests are not guaranteed to be unique.
- `Public Key Infrastructure (PKI)` The people, policies, and technology used to manage public keys and digital certificates. A certificate authority issues certificates that associate a public key with an identity.
- `Salting` Adding random data to a password before hashing it, making identical passwords produce different hashes and reducing the usefulness of precomputed hash attacks.
- `Stream Cipher` Encrypts plaintext one bit or byte at a time using a symmetric key, continuously encrypting data as it flows, making it well-suited for secure wireless communications.
- `Symmetric Encryption` One of the two main types of encryption, also known as conventional encryption, uses a single, shared, secret key.
- `Threat` A potential cause of harm that could exploit a vulnerability and affect systems or information.
- `Threat Modeling` A structured way to identify what a system needs to protect, how it might be attacked, and which safeguards should be prioritized.
- `Vulnerability` A weakness in software, hardware, configuration, or process that could be exploited by a threat.
- `Zero-Day Vulnerability` A software flaw for which no fix is available to affected users when it is discovered or exploited. Attackers may exploit it before a patch is released.

## Identity and Access Management

- `Access Control List (ACL)` A set of rules that specifies which users or systems can access a resource and what actions they may perform. Network ACLs can also allow or block traffic based on rules such as IP addresses and ports.
- `Authentication` The process or method of verifying the identity of a user, device, or system.
- `Authorization` The process of deciding what an authenticated user, device, or system is allowed to access or do. Authentication asks who you are; authorization determines what you can do.
- `Biometrics` Methods of identifying individuals based on unique physical or behavioral traits, such as fingerprints, facial recognition, or iris scans.
- `Effective Permissions` The combined permissions a user or group has on a file or folder, consisting of both explicit and inherited permissions.
- `Explicit Permissions` Permissions applied directly to a specific file or folder.
- `JSON Web Token (JWT)` A compact format for carrying claims between systems. JWTs are often signed but are not necessarily encrypted, so their contents should not be treated as secret.
- `Least Privilege` The principle of giving users and systems only the permissions they need to perform their tasks.
- `Multi-Factor Authentication (MFA)` Authentication using at least two different types of factor: something you know (such as a password), have (such as a security key), or are (such as a fingerprint). Two passwords alone are not two factors.
- `Passphrase` A sequence of characters longer than a password, offering greater security due to its increased complexity.
- `Permissions` Defines the type of access granted to folders, files, and Active Directory objects, such as full control, modify, list folder contents, read, and write.
- `Role-Based Access Control (RBAC)` An access-control model where permissions are assigned to roles, and users receive permissions through the roles they are assigned.
- `Session Management` The process of maintaining and protecting a user's authenticated state between requests, including creating, expiring, and invalidating sessions.
- `Smart Card` Contains information on an embedded chip about the user, often used for secure authentication, access control, and storing sensitive information
- `Two-Factor Authentication (2FA)` A type of MFA that requires exactly two different kinds of authentication factor, such as a password and a security key.

## Network Security

- `A Record` A DNS record that maps a domain name to a 32-bit IPv4 address.
- `AAAA Record` A Quad-A record is used in IPv6 to map a domain name to its IPv6 address.
- `Application Programming Interface (API)` A defined way for software systems to communicate, often by exchanging requests and responses. APIs need authentication and authorization just like other application interfaces.
- `Content Security Policy (CSP)` A browser policy, usually sent in an HTTP response header, that restricts which sources a page can load content from. It can help reduce the impact of some cross-site scripting attacks.
- `Cross-Origin Resource Sharing (CORS)` A browser mechanism that uses server response headers to allow selected websites to read responses from another origin. It is not a replacement for authentication or authorization.
- `Domain Name System (DNS)` A distributed naming system that translates domain names, such as example.com, into IP addresses that computers use to communicate.
- `Firewall` Hardware or software that allows or blocks network traffic according to rules. A firewall can reduce unwanted connections, but it does not prevent every kind of attack.
- `HyperText Transfer Protocol Secure (HTTPS)` HTTP secured with Transport Layer Security (TLS). It helps protect data in transit and lets a browser verify the server's certificate.
- `Intrusion Detection System (IDS)` A system that monitors network or system activity for suspicious behavior and generates alerts.
- `Intrusion Prevention System (IPS)` A system that monitors network or system activity and can block detected malicious behavior.
- `IP Address` A numeric address assigned to a device or network interface so that data can be routed to it over an IP network.
- `Network Time Protocol (NTP)` Used to provide automated time synchronization.
- `Network Segmentation` Dividing a network into separate sections and controlling traffic between them to limit unauthorized access and reduce the spread of an intrusion.
- `Port` A number used by network protocols to direct traffic to a particular service on a device; it is a logical endpoint, not a physical socket.
- `Protocol` An agreed set of rules that devices follow to communicate and exchange data.
- `PTR Record (Pointer Records)` DNS records that map an IP address to a domain name, used primarily for reverse DNS lookups.
- `Reverse Proxy` A server that receives requests from clients and forwards them to backend servers. It can route traffic and may terminate TLS, but it does not replace security checks in the application.
- `Same-Origin Policy (SOP)` A browser security rule that restricts a page's scripts from reading data from a different origin. CORS provides a controlled way for a server to permit some cross-origin access.
- `Secure Shell (SSH)` A protocol for encrypted remote logins, command execution, and file transfers. It commonly uses port 22, though a server can be configured to use another port.
- `Transmission Control Protocol (TCP)` A transport protocol that establishes a connection and delivers data in order and with checks for loss.
- `Transport Layer Security (TLS)` A cryptographic protocol that helps protect data sent between systems, such as a browser and a web server.
- `User Datagram Protocol (UDP)` A transport protocol that sends individual datagrams without setting up a connection or guaranteeing delivery or order.
- `Virtual Private Network (VPN)` A connection that carries network traffic between a device and a VPN endpoint through a tunnel, commonly encrypted to protect it in transit. A VPN does not make a device or its traffic automatically safe.
- `Zero Trust` A security approach that does not automatically trust a user or device because it is inside a network; access is verified and limited to what is needed.

## Email Security

- `DKIM` DomainKeys Identified Mail lets a sending domain sign email with a private key. Receiving mail systems can check the signature using a public key published in the domain's DNS records; this helps detect changes to signed parts of the message.
- `DMARC` Domain-based Message Authentication, Reporting & Conformance tells receiving mail systems how to handle messages that fail SPF or DKIM checks aligned with the visible From domain. It can also provide reports to the domain owner.
- `Email Reputation` A measure of an email sender's trustworthiness, affecting whether emails are delivered to inboxes or marked as spam.
- `MX Record` A type of DNS record that specifies the mail server responsible for receiving and handling email messages for a domain.
- `Sender Policy Framework (SPF)` A domain publishes a DNS record listing mail servers authorized to send mail for it. Receiving servers can check the sending server against the domain used for the email's return path; SPF alone does not verify the visible From address.
- `Simple Mail Transfer Protocol (SMTP)` The protocol used to send email from clients to mail servers and between mail servers. SMTP does not inherently encrypt messages; servers can use TLS to protect a connection.

## Threats and Attacks

- `Broken Object Level Authorization (BOLA/IDOR)` A web or API vulnerability where the application fails to check whether a user is allowed to access a specific object, potentially exposing or allowing changes to another user's data.
- `Brute-Force Attack` Repeatedly trying possible passwords or keys until the correct one is found.
- `Credential Stuffing` Trying username and password pairs exposed in one breach on other services, taking advantage of password reuse.
- `Cross-Site Request Forgery (CSRF)` An attack that tricks a user's browser into sending an unwanted request to a site where the user is signed in. Anti-CSRF tokens and appropriate cookie settings can help prevent it.
- `Cross-Site Scripting (XSS)` A web vulnerability that allows an attacker to make a site deliver untrusted script to visitors' browsers. Sites can reduce the risk by safely handling and encoding user input.
- `Denial-of-Service (DoS)` An attack that overwhelms or disrupts a service so legitimate users cannot access it.
- `Distributed Denial-of-Service (DDoS)` A denial-of-service attack carried out from many systems at once, making the traffic harder to block at a single source.
- `Exploit` A technique or piece of code that takes advantage of a vulnerability to cause unintended behavior.
- `Malware` Software designed to harm, disrupt, spy on, or gain unauthorized access to systems or data; ransomware and rootkits are examples.
- `Man-in-the-Middle (MitM) Attack` An attacker secretly intercepts, and may alter, communication between parties that believe they are communicating directly.
- `Pharming` Redirecting a user to a fraudulent website, often by tampering with DNS or device settings, even when the user enters the expected web address.
- `Phishing` A social engineering attack that tricks people into revealing information or taking unsafe actions, often through deceptive messages or websites.
- `Ransomware` Malware that blocks access to data or systems, often by encrypting files, and demands payment to restore access.
- `Rootkit` Malicious software that enables an unauthorized user to gain control of a computer system.
- `Spoofing` Falsifying information, such as an email sender address or network address, to appear to be a different or trusted source.
- `Social Engineering` The manipulation of people into disclosing information or performing actions that compromise security.
- `Spam` Unsolicited and irrelevant emails that are typically sent to a large number of users, often for advertising, phishing, or spreading malware.
- `SQL Injection` A web vulnerability where untrusted input changes the meaning of a database query. Using parameterized queries helps prevent it.
- `Server-Side Request Forgery (SSRF)` A vulnerability that tricks a server into making requests to destinations chosen by an attacker, which can expose internal services or data. Defenses include strict destination allowlists and blocking unnecessary outbound network access.
- `Trojan Horse` Malware disguised as legitimate software or a file, relying on someone to run or install it.

## Data Protection and Privacy

- `Cookie` A small piece of data a website asks a browser to store and send back with later requests, often to remember preferences or maintain a session. Cookies are not programs, but they can be used for tracking, and stolen session cookies may let someone impersonate a signed-in user.
- `Data at Rest` Data stored on a device or other storage media, rather than being transmitted over a network.
- `Data Breach` An incident in which information is accessed, disclosed, changed, or lost without authorization.
- `Data Classification` Categorizing information by sensitivity or importance so appropriate handling and security controls can be applied.
- `Data Loss Prevention (DLP)` Policies and tools that help detect or prevent sensitive data from being improperly shared, transferred, or removed.
- `Data Minimization` Collecting and keeping only the personal data needed for a specific purpose.
- `Data Retention` Rules for how long information is kept and when it should be securely deleted.
- `Encryption at Rest` Encryption that protects stored data, for example on a disk, if the storage or device is accessed without authorization.
- `Encryption in Transit` Encryption that protects data while it is being sent between devices or services, such as through TLS.
- `Personally Identifiable Information (PII)` Information that identifies, or can reasonably be linked to, a person, such as a name combined with an account number.
- `Session Cookie` A cookie that helps a website recognize a browser's signed-in session. Websites should protect session cookies and invalidate sessions when users sign out.

## Security Operations and Governance

- `Asset` Anything an organization values and needs to protect, such as data, devices, accounts, or services.
- `Backup and recovery` Making separate copies of data and restoring them after data loss or system failure. Restore procedures should be tested to make sure backups can actually be used.
- `Incident Response` The coordinated process of preparing for, identifying, containing, and recovering from security incidents.
- `OS Hardening` Minimizes exposure to threats and involves activities such as using anti-virus and anti-spyware protection, securing email, and conducting periodic risk assessments.
- `Patch Management` The process of identifying, testing, and applying software updates to fix bugs and security vulnerabilities.
- `Penetration Testing` An authorized, scoped test that safely attempts to find and verify security weaknesses in a system.
- `Risk` The chance that a threat will exploit a vulnerability and the resulting impact. Risk assessments consider both how likely an event is and how harmful it would be.
- `Security Awareness Training` Training that helps people recognize common risks, handle information safely, and report suspicious activity.
- `Security Control` A safeguard used to reduce risk. Controls may prevent problems, detect them, or help respond and recover.
- `Security Information and Event Management (SIEM)` A system that collects and correlates security logs from multiple sources to help detect and investigate suspicious activity.
- `Security Policy` A formal set of rules and responsibilities describing how an organization expects its information and systems to be protected.
- `Software Bill of Materials (SBOM)` A structured inventory of the components and dependencies included in a software product, useful for identifying whether a known vulnerability affects it.
- `Software Supply Chain` The people, code, dependencies, tools, and services involved in building and distributing software. A weakness or compromise in one part can affect the final product.
- `Updates` Provide the necessary security patches to software that minimize your exposure to malicious activity.
- `Vulnerability Assessment` A process for identifying and prioritizing weaknesses in systems; unlike a penetration test, it does not necessarily attempt to exploit them.

## OS-Specific Tools and Features

### System Structure and File Systems

- `/bin` A subdirectory that includes common programs shared by the system, the system administrator, and users.
- `/home` A subdirectory that contains the home directories of regular users.
- `/proc` A subdirectory that contains information about system resources and processes.
- `/var` A subdirectory that stores variable data, including logs, temporary files, and other data that changes frequently, often created by both the system and users.
- `ext4` An advanced Linux file system that offers better performance, larger file and volume sizes, and improved reliability.
- `File Allocation Table (FAT)` A simple Windows file system that only supports read-only, hidden, system and archive file attributes.
- `File System` A method used by an OS to manage, store, and organize files on a storage device, controlling how data is stored, accessed, and retrieved.
- `Graphical User Interface (GUI)` A visual interface that allows users to interact with a computer using icons, buttons, and windows.
- `Hardware Abstraction Layer` Software that manages communication between the kernel and the hardware.
- `Kernel` The core of the OS that acts as a translator between hardware and software by managing all input/output requests, memory, and connected peripherals.
- `Kernel Mode` A privileged mode in an OS with full access to hardware and system resources, used for executing critical tasks and managing hardware.
- `Shell` A terminal, console, or command prompt that allows users to interact with the OS by executing text-based commands to manage files, run programs, and automate tasks.
- `User Mode` A processing mode in an OS where applications run with limited privileges, preventing direct access to critical system resources to help ensure system stability and security.

### Accounts, Permissions, and Administration

- `Administrative Shares` Hidden shares in a Microsoft OS, marked by a dollar sign ($), that allow system administrators remote access to all disk volumes on a network-connected system.
- `Administrator Account` An account that is used exclusively for performing tasks requiring elevated permissions, such as installing new software or updates.
- `Inheritance` Permissions granted to a parent folder that automatically extend to its child objects, such as subfolders and files.
- `New Technology File System (NTFS)` An advanced Windows file system offering enhanced security, granular permissions, and support for large volumes.
- `NTFS Permissions` Include full control, modify, read and execute, list folder contents, read, write, and special permissions.
- `Permission Groups` Categories in a Linux system, i.e., Owner, Group, or Other, used to assign permissions to files, folders, and resources based on roles or needs.
- `Runas` A command in Windows that allows users to run a program or command with different user credentials, typically with elevated or administrative privileges.
- `Share Permissions` Controls that determine the level of access users have to shared resources, typically including options like full control, change, read, and no access.
- `User Account Control (UAC)` A security feature in Windows that helps prevent unauthorized changes to the OS by prompting for permission or an administrator password before allowing actions that could affect system settings or files.

### Platform Security Features

- `BitLocker` A full-disk encryption feature in Windows Pro and Enterprise that prevents unauthorized access in case of theft or loss.
- `Controlled Folder Access` A Windows security feature that protects folders from unauthorized access by allowing only trusted applications to make changes.
- `FileVault` A disk encryption feature in macOS that encrypts the entire drive to protect data from unauthorized access.
- `Security Chip` A hardware component used in macOS devices to enhance security by protecting sensitive data, encrypting storage, and securing the boot process.
- `System Restore` This creates restore points when you install a new app, driver, or Windows update, allowing you to roll back if the upgrade causes instability or incompatibility.
- `Windows Defender` The former name for Microsoft's built-in Windows security tools. Microsoft Defender Antivirus provides malware protection; Windows Firewall is a separate component that filters network traffic.
- `Windows Hello` A feature that allows you to use a PIN, facial recognition, or fingerprint to access a Windows OS. To use the biometric options, you will need a compatible device, such as a camera or fingerprint reader.

### Security Utilities

- `Browser Filter` A tool or setting in a web browser that blocks or controls access to specific content, such as inappropriate websites, ads, or trackers, enhancing security.
- `chkrootkit` A shell script that checks system binaries for rootkit modifications and various security issues, helping to prevent malware and rootkits from affecting a Linux host.
- `ClamAV` A versatile, cross-platform, open-source antivirus engine.
- `Rootkit Hunter` A powerful, easy-to-use, and well-known tool for scanning backdoors, rootkits, and local exploits on a Linux system.

### Rootkit Types

- `Kernel Mode Rootkit` Operates at the same level as the operating system and anti-malware software, making it highly invasive and difficult to detect.
- `User Mode Rootkit` Changes applications at a user level, provides backdoor access, disables system and security services, hides processes, and files, and conceals network interfaces.
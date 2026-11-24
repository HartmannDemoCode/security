# Security Tools

Use assessment and testing tools only on systems you own or have explicit permission to assess. Avoid uploading confidential files or data to public analysis services.

## Core Cybersecurity Concepts

- **CyberChef** — A web-based tool for inspecting and transforming data, including calculating hashes and decoding common formats.
- **OpenSSL** — A cryptography toolkit commonly used to inspect certificates and test cryptographic and TLS configurations.

## Identity and Access Management

- **Authenticator apps (Aegis, Google Authenticator, Microsoft Authenticator)** — Mobile apps that generate one-time sign-in codes or approve sign-in requests, depending on the app and account.
- **Bitwarden** — A password manager for creating and storing unique credentials.
- **KeePassXC** — A local password manager that stores credentials in an encrypted database.
- **Microsoft Entra ID** — A cloud identity and access management service for managing accounts, authentication, and access to applications.
- **YubiKey** — A hardware security key that supports phishing-resistant authentication standards such as FIDO2/WebAuthn.

## Network Security

- **OPNsense** — An open-source firewall and routing platform for managing network traffic.
- **Suricata** — A network security monitoring engine that can detect suspicious traffic and, when configured, help prevent it.
- **Wireshark** — A network protocol analyzer for examining captured network traffic during troubleshooting or authorized investigations.
- **WireGuard** — A modern VPN protocol and software used to create encrypted network tunnels.
- **Zeek** — A network security monitoring platform that produces detailed records of network activity.

## Email Security

- **dmarcian** — A service that helps organizations review and understand DMARC reports.
- **MXToolbox** — A web service for checking DNS and email configuration, including mail records and sender-authentication settings.
- **Rspamd** — An email filtering system that helps identify spam and other suspicious messages.

## Threats and Attacks

- **VirusTotal** — A service for checking file and URL reputation using multiple security vendors. Submissions may be shared with security partners or the public, so do not upload confidential material.
- **YARA** — A tool for matching files or memory against rules used in malware research and incident analysis.

## Data Protection and Privacy

- **Cryptomator** — A tool that encrypts files before they are stored with supported cloud-storage services.
- **GnuPG (GPG)** — A tool for encrypting and signing files or messages using public-key cryptography.
- **VeraCrypt** — A tool for creating encrypted containers or volumes to protect stored data.

## Security Operations and Governance

- **Burp Suite** — A web application security testing platform for inspecting application traffic and supporting authorized assessments.
- **DirBuster** — A web application assessment tool that checks for unintentionally exposed paths; use only in authorized tests.
- **Greenbone OpenVAS** — A vulnerability management platform for finding and prioritizing known weaknesses on authorized systems.
- **OWASP ZAP** — An open-source web application security testing tool for finding common issues in applications you are authorized to assess.
- **Trivy** — A scanner for identifying known vulnerabilities and configuration issues in containers, code repositories, and other software artifacts.
- **Wazuh** — A security monitoring platform that collects endpoint data and helps detect suspicious activity.

## OS-Specific Tools and Features

- **ClamAV** — A cross-platform antivirus engine for detecting malicious files.
- **Lynis** — A security auditing tool that checks Unix-like systems for configuration and hardening issues.
- **Microsoft Defender Antivirus** — Built-in Windows antivirus protection that can detect and respond to malware.
- **Rootkit Hunter (rkhunter)** — A Linux utility that checks for indicators of rootkits and other suspicious system changes.
- **Sysinternals Suite** — A collection of Windows utilities for inspecting processes, services, startup activity, and other system behavior.
- **Windows Event Viewer** — A built-in Windows utility for reviewing system, application, and security logs.

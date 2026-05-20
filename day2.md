# Day 2 - Planning and scoping your penetration test

## Forberedelse: (ca. 6 timer)

- https://tryhackme.com/room/pentestingfundamentals
- https://tryhackme.com/room/redteamengagements
- https://tryhackme.com/room/cybergovernanceregulation
- https://tryhackme.com/room/offensivesecurityintro
- https://tryhackme.com/room/defensivesecurityintro
- https://tryhackme.com/room/careersincyber

## Fremgangsmetode til hacking:

[Video](https://cphbusiness.cloud.panopto.eu/Panopto/Pages/Viewer.aspx?id=553eaa5e-333e-4069-810d-b06400de19ed) som forklarer faserne

- Recon & Research
- Scanning
- Exploitation
- POST Exploitation
- Persistence
- CleanUp

## Nyttige kommandoer til dagens rum og fremtiden:

- `nmap -sV -sC <ip>`
- `gobuster dir -u <url> -w <wordlist>`
- `enum4linux <ip>`
- `hydra -l <user> -P <wordlist> <ip> <service>`
- `ssh -i <id_rsa> <user>@<ip>`
- `ssh2john <id_rsa> > <hash_name>`
- `john --wordlist=<wordlist> <hash_name>`
- `cat /etc/passwd`

## Vigtige interessepunkter på en server til dagens rum og fremtiden:

- Findes der er SSH/RSA nøgle? Hvis ja download den og forsøg at logge ind med den via SSH
- Kig efter SSH/RSA nøgler /home/<user>/.ssh/id_rsa

## Dette (disse) TryHackMe rum skal vi sammen hacke: Basic Pentesting

Feedback til dagen: (GAMMEL) [Video](https://cphbusiness.cloud.panopto.eu/Panopto/Pages/Viewer.aspx?id=82b97cce-b845-46d7-8818-b21c009b048c)

Opfølgning på dagens rum: (GAMMEL) [Video](https://cphbusiness.cloud.panopto.eu/Panopto/Pages/Viewer.aspx?id=22af9db9-f513-4015-8814-b064010f3e6c) 

Opfølgning på dagens rum: (Nyeste) [Video](https://cphbusiness.cloud.panopto.eu/Panopto/Pages/Viewer.aspx?id=bcac0d83-5c71-4a73-8425-b27c006a0ff0)

I dag skal vi i gang. Så der vil være en masse information som er fælles.
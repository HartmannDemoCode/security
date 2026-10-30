# Observation af værktøjer

Datamatikerne har allerede arbejdet med Java, og derfor giver det mening at bruge Java til den del, der handler om **secure software development**.

Her kan man eksempelvis arbejde med:

- authentication og authorization
- input validation
- SQL Injection
- password hashing
- secure API'er
- dependency vulnerabilities
- logging
- session/token security
- secure coding

På den måde bruges tiden på security frem for at lære et nyt programmeringssprog først.

## Python

Python kan være relevant som et **værktøj** frem for som et nyt hovedsprog.

Det egner sig godt til mindre scripts som:

- port scanning
- sende HTTP requests
- simple brute-force scripts
- automatisering
- parsing af logs
- sockets
- fuzzing
- behandling af data

Pointen er ikke at lære Python som selvstændigt fag, men at kunne bruge eller tilpasse simple scripts til en security-opgave.

## Linux / Bash

Linux og Bash er også centrale, da mange security-værktøjer og miljøer bygger på Linux.

Relevante værktøjer og kommandoer kan blandt andet være:

```bash
grep
find
curl
ssh
openssl
nc
```

Her handler det om at kunne undersøge et system og bruge eksisterende værktøjer effektivt.

## Burp Suite

Burp Suite er et værktøj til at undersøge sikkerheden i webapplikationer. Det gør HTTP-kommunikationen synlig, så jeg kan undersøge, hvordan serveren reagerer, når en request ændres.

- [Proxy](https://portswigger.net/burp/documentation/desktop/tools/proxy) kan opsnappe og ændre requests mellem browseren og serveren. HTTP history giver overblik over trafikken.
- [Repeater](https://portswigger.net/burp/documentation/desktop/tools/repeater) kan bruges til at ændre og sende den samme request flere gange og undersøge svarene.

Det er relevant til at undersøge path traversal, IDOR og loginfejl ved at ændre parametre og se, hvordan serveren reagerer.

[Community Edition](https://portswigger.net/burp/communitydownload) er gratis og tilstrækkelig til de manuelle øvelser med Proxy og Repeater. Den automatiske sårbarhedsscanner kræver Professional.

Til valgfaget vil jeg starte med at kunne finde en request i HTTP history, sende den til Repeater, ændre én parameter og forklare resultatet. Det giver en konkret forbindelse mellem HTTP, en sårbarhed og test af en rettelse.

## Mulig opdeling

### Java

- secure software development
- finde vulnerabilities i kode
- rette vulnerabilities
- authentication og authorization
- API security

### Linux / Bash

- systemforståelse
- SSH
- filer og permissions
- netværk
- security-værktøjer

### Python

- små security scripts
- automatisering
- scanning
- simple exploits eller tests

### Security-værktøjer

- Burp Suite
- Nmap
- Netcat
- OpenSSL

## Vurdering

Python behøver ikke være et selvstændigt læringsmål.

Et mere relevant mål kunne være:

> Den studerende kan anvende og tilpasse simple scripts til security-relaterede analyser og automatisering.

En mulig opdeling kunne derfor være:

> **Java til at bygge og sikre systemet.**  
> **Linux/Bash til at undersøge systemet.**  
> **Python til små scripts og automatisering.**

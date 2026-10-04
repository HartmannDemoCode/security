# Ressourcer til undersøgelse

Her samler jeg de ressourcer, der virker mest relevante for Security-valgfaget.

Prioriteringen tager udgangspunkt i, at de studerende er **datamatikere på 4. semester**. De er derfor ikke begyndere i programmering, databaser, HTTP, API'er osv., men kan godt være begyndere inden for security.

---

## 1. MOOC.fi – Cyber Security Base

https://cybersecuritybase.mooc.fi/

**Undersøgt**

Indtil videre en af de mest relevante ressourcer.

Det gode er kombinationen af:

- web security
- OWASP vulnerabilities
- secure software development
- vulnerability analysis
- threat analysis
- Linux og netværk
- praktiske øvelser
- projekt med vulnerabilities og fixes

En del af materialet omkring almindelig webudvikling, HTTP og databaser er for grundlæggende for en datamatiker, men security-delen passer godt.

Der findes også Advanced Topics / Cyber Security II, som stadig skal undersøges nærmere.

---

## 2. PortSwigger Web Security Academy

https://portswigger.net/web-security

Meget høj prioritet.

Har teori og praktiske labs omkring blandt andet:

- SQL Injection
- XSS
- Access Control
- Authentication
- CSRF
- SSRF
- API Security
- JWT
- Business Logic
- Race Conditions

Det virker især relevant, fordi man selv skal finde og udnytte vulnerabilities.

---

## 3. OWASP Web Security Testing Guide

https://wstg.owasp.org/

Relevant fordi det ikke kun handler om enkelte attacks, men om:

> Hvordan laver man en systematisk security-test af en applikation?

Det kunne være interessant i forhold til læringsmålet omkring vulnerability analysis.

---

## 4. OWASP Juice Shop

https://owasp.org/www-project-juice-shop/

En bevidst sårbar webapplikation.

Interessant til praktisk arbejde med:

- vulnerability analysis
- exploitation
- OWASP
- dokumentation
- mitigations

Kunne muligvis bruges som en større øvelse eller case.

---

## 5. CS50 – Introduction to Cybersecurity

https://cs50.harvard.edu/cybersecurity/

CS50 er interessant, men jeg er lidt mere i tvivl om niveauet.

Kurset er lavet som en introduktion til cybersecurity for både tekniske og ikke-tekniske deltagere.

### For

- godt struktureret kursus
- gode videoer og undervisningsmaterialer
- giver et bredt overblik over cybersecurity
- Securing Systems virker relevant
- Securing Software virker relevant
- kommer omkring både offensive og defensive emner
- interessant at se hvordan Harvard har opbygget et samlet cybersecurity-kursus

### Imod

- lavet til begyndere inden for både tekniske og ikke-tekniske målgrupper
- nogle emner kan derfor være for grundlæggende for en datamatiker
- vi behøver ikke undervisning i grundlæggende programmering, HTTP eller hvordan almindelige webapplikationer fungerer
- bredt cybersecurity-fokus betyder mindre dybde på nogle områder

Jeg tror derfor ikke nødvendigvis hele CS50-kurset er relevant.

Det interessante er mere at undersøge bestemte dele, især:

- Securing Systems
- Securing Software

De studerende er begyndere inden for security, så introduktion til security-koncepter giver mening.

Men undervisningen bør kunne bygge videre på, at de allerede er softwareudviklere.

---

## 6. OWASP Top 10

https://owasp.org/www-project-top-ten/

Relevant som fælles grundlag for hvilke web vulnerabilities de studerende bør kende.

Ikke nødvendigvis et kursus i sig selv, men en vigtig reference.

---

## 7. OWASP API Security Top 10

https://owasp.org/www-project-api-security/

Virker meget relevant for datamatikere, fordi API'er allerede er en normal del af uddannelsen.

Her bliver spørgsmålet ikke:

> Hvordan laver man et API?

men:

> Hvordan laver og tester man et API sikkert?

---

## 8. OverTheWire – Bandit

https://overthewire.org/wargames/bandit/

**Undersøgt**

God til:

- Linux
- SSH
- permissions
- shell
- services og porte
- problemløsning

Virker især relevant til Linux-delen af valgfaget.

---

## 9. TryHackMe

https://tryhackme.com/

Har mange guidede security-forløb og praktiske labs.

Kunne være interessant som platform til:

- penetration testing
- Linux
- web security
- networking
- security engineering

Skal undersøges for at se, om niveauet og indholdet passer.

---

## 10. pwn.college

https://pwn.college/

Meget omfattende platform med praktiske challenges.

Indeholder blandt andet:

- Linux
- system security
- web security
- program security
- binary exploitation

Interessant, men noget af materialet går sandsynligvis længere ned i binary/system security, end der er behov for på datamatiker.

---

## 11. CTF101

https://ctf101.org/

God som introduktion og opslagsværk til CTF.

Kan bruges til at forstå forskellige typer challenges og områder inden for security.

Jeg ville dog prioritere de praktiske ressourcer højere.

---

# Reference-materiale

## OWASP Cheat Sheet Series

https://cheatsheetseries.owasp.org/

God til den defensive side:

> Hvordan implementerer man det sikkert?

---

## OWASP ASVS

https://owasp.org/www-project-application-security-verification-standard/

Kan bruges som inspiration til sikkerhedskrav og vurdering af applikationer.

---

## OWASP Threat Modeling

https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html

Relevant til:

- assets
- data flows
- trust boundaries
- threats
- mitigations

---

# Jura og etik

## Dansk lovgivning

https://www.retsinformation.dk/

Skal undersøges separat i forhold til:

- penetration testing
- tilladelse og scope
- uberettiget adgang
- ansvarlig disclosure

Dette er direkte en del af læringsmålene.
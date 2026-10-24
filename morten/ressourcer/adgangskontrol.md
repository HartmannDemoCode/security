# Materiale til login, sessioner og adgangskontrol

Status: Skal undersøges.

Her undersøger jeg, hvilket materiale der giver mening at bruge til valgfaget. Jeg vurderer forklaringer, niveau, øvelser og opsætning.

[Planen](../forloeb/01-adgangskontrol.md) beskriver arbejdet. Mine forklaringer af selve sårbarhederne og resultaterne fra øvelserne ligger i [de faglige noter](../emner/adgangskontrol.md).

## OWASP Top 10 – A01 og A07

[OWASP A01:2025](https://top10.owasp.org/2025/A01_2025-Broken_Access_Control/index.html)

[OWASP A07:2025](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)

Jeg vil læse Description, How to prevent og Example attack scenarios. Jeg vil vurdere, om teksten giver et tilstrækkeligt overblik, og hvilke dele der skal uddybes med øvelser eller andre forklaringer.

Min vurdering efter læsningen:

## PortSwigger Web Security Academy

- [Access control](https://portswigger.net/web-security/access-control)
- [IDOR](https://portswigger.net/web-security/access-control/idor)
- [Lab: User ID controlled by request parameter](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter)
- [Authentication](https://portswigger.net/web-security/authentication)
- [Lab: 2FA simple bypass](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass)

Jeg starter med IDOR og 2FA simple bypass, så jeg får et eksempel fra hver kategori. Hvis jeg vil prøve flere varianter af adgangskontrol, kan jeg bruge:

- [User role controlled by request parameter](https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter)
- [Multi-step process with no access control on one step](https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step)

Jeg vil undersøge, om forklaringerne og labben passer til vores niveau. Jeg noterer især, hvor meget vejledning jeg får brug for, og om øvelsen hjælper mig med at forklare årsagen til fejlen.

Min vurdering efter afprøvningen:

## TryHackMe som alternativ

[IDOR-forløbet](https://tryhackme.com/room/idor) har opgaver om blandt andet kodede, hashede og uforudsigelige ID'er. Jeg kan bruge det til at sammenligne vejledning og sværhedsgrad med PortSwigger. Det er et alternativ, jeg endnu ikke har afprøvet.

Min vurdering, hvis jeg afprøver det:

## Pluralsight

[Secure Coding: Preventing Broken Access Control](https://www.pluralsight.com/courses/secure-coding-preventing-broken-access-control) af Gavin Johnson-Lynn.

Jeg vil starte med Finding Insecure Direct Object References (IDOR) og Forced Browsing to Find Hidden Functionality. Kurset er sidst opdateret i 2020, så jeg bruger OWASP 2025 som reference for kategorien. Jeg har ikke vurderet videoerne endnu.

Jeg vil undersøge, om videoerne forklarer noget bedre end teksterne, og hvilke dele der er relevante, når man allerede kender authorization.

Min vurdering efter at have set afsnittene:

## WebGoat

[OWASP WebGoat](https://owasp.org/projects/webgoat)

Jeg vil finde og prøve en relevant øvelse om adgangskontrol. Det er interessant at se emnet i en Java-applikation. Jeg vil især undersøge opsætning, vejledning og muligheden for at forbinde øvelsen med kode og rettelser.

Valgt øvelse:

Min vurdering efter afprøvningen:

## OWASP Authorization Cheat Sheet

[Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)

Jeg bruger den til at undersøge, hvordan fejlen forebygges. Jeg vil vurdere, hvilke afsnit der er nyttige som reference til vores Java-eksempel.

Min vurdering efter brug:

## Hvad ville jeg vælge til undervisningen?

Skrives efter afprøvningen. Jeg vil begrunde valget ud fra forklaringerne, opsætningen, sværhedsgraden og forbindelsen mellem at finde og rette fejlen.

## Ekstra læsning

I [ekstra materiale](ekstra-materiale.md) har jeg samlet forslag til uddybning. Til dette forløb er det især OWASPs IDOR- og sessionstekster samt udvalgte kapitler fra API Security in Action.

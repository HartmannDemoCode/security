# Materiale til login, sessioner og adgangskontrol

Status: I gang. OWASP A01 og A07 er læst, Pluralsight-kurset OWASP: Broken Access Control er gennemført, og PortSwiggers afsnit om adgangskontrol er læst med de 13 tilhørende labs løst.

Her samler jeg mine noter og vurderinger af materialer om login, sessioner og adgangskontrol.

[Planen](../forloeb/01-adgangskontrol.md) beskriver arbejdet. Mine forklaringer af selve sårbarhederne og resultaterne fra øvelserne ligger i [de faglige noter](../emner/adgangskontrol.md).

## OWASP Top 10 – A01 og A07

[OWASP A01:2025](https://top10.owasp.org/2025/A01_2025-Broken_Access_Control/index.html)

[OWASP A07:2025](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)

Jeg har læst OWASP A01 og A07, herunder Description, How to prevent og Example attack scenarios. Min vurdering af teksterne og behovet for uddybning med øvelser mangler endnu at blive skrevet.

Min vurdering efter læsningen:

## PortSwigger Web Security Academy

- [Access control](https://portswigger.net/web-security/access-control)
- [IDOR](https://portswigger.net/web-security/access-control/idor)
- [Authentication](https://portswigger.net/web-security/authentication)

Jeg har læst **Access control vulnerabilities and privilege escalation** og løst de 13 labs, der er vist i afsnittet. De dækker blandt andet ubeskyttede adminfunktioner, ændring af roller og bruger-ID'er, forskelle i URL- og metodekontrol samt manglende kontrol i et trin af et forløb.

Det var fin træning i broken access control og i at bruge Burp Suite til at ændre requests og undersøge svarene.

Det er stadig uklart for mig, hvordan jeg overfører eksemplerne til mine egne Java-projekter og vurderer, om de har tilsvarende huller. Nogle af eksemplerne virker konstruerede, og jeg er i tvivl om, hvor ofte de opstår i praksis. Jeg mangler at forbinde øvelserne med den kode og opsætning, jeg selv bruger.

Næste skridt er at undersøge én konkret funktion i et eget projekt med to brugere og forskellige roller. Det skal vise, hvor adgangskontrollen faktisk sker. Authentication og 2FA simple bypass mangler endnu at blive gennemført.

## Systematisk adgangskontroltest i eget projekt

Jeg vil bruge disse vejledninger til at planlægge tests af FOG Carport med forskellige brugere, roller og ejerskab af data:

- [OWASP WSTG – Testing for Bypassing Authorization Schema](https://owasp.github.io/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema): kontrol af adgang uden login, efter logout og med forskellige roller.
- [OWASP WSTG – Insecure Direct Object References](https://wstg.owasp.org/latest/4-Web_Application_Security_Testing/05-Authorization/04-Insecure_Direct_Object_References/): kortlægning af objektreferencer og test med flere brugere, der ejer forskellige data.
- [PortSwigger – Burp Repeater](https://portswigger.net/burp/documentation/desktop/tools/repeater): ændring og gentagen afsendelse af requests, så jeg kan sammenligne den tilladte og den afviste adgang.

[Min testopskrift og afgrænsning](../emner/adgangskontrol.md#systematisk-test-af-adgangskontrol-i-fog-carport) ligger i de faglige noter sammen med [Excel-skabelonen](../emner/adgangskontrol/fog-adgangskontrol.xlsx). Regnearket adskiller forventede adgangsregler fra konkrete forsøg og testresultater. Der er endnu ikke udført tests fra matrixen eller skrevet en vurdering af vejledningerne efter praktisk brug i FOG.

## TryHackMe som alternativ

[IDOR-forløbet](https://tryhackme.com/room/idor) har opgaver om blandt andet kodede, hashede og uforudsigelige ID'er. Jeg kan bruge det til at sammenligne vejledning og sværhedsgrad med PortSwigger. Det er et alternativ, jeg endnu ikke har afprøvet.

Min vurdering, hvis jeg afprøver det:

## Pluralsight

### OWASP: Broken Access Control

Jeg har gennemført **OWASP: Broken Access Control** og synes, det er rigtig godt, især afsnittene om forsvar mod adgangskontrolfejl og de mere avancerede forsvarsprincipper.

[Kurset er af Gavin Johnson-Lynn](https://www.pluralsight.com/courses/owasp-broken-access-control), varer cirka 42 minutter og er på Intermediate-niveau. Det er senest opdateret 23. december 2025. Den offentlige kursusoversigt beskriver fire dele:

- Hvordan adgangskontrol fejler, og hvordan requests kan manipuleres.
- Horisontal adgangskontrol: adgang til andre brugeres ressourcer og forsvar mod det.
- Vertikal adgangskontrol: adgang til funktioner, der kræver flere rettigheder, og forsvar mod det.
- Forsvarsprincipper og mere avanceret adgangskontrol.

Kurset giver mig en god sammenhæng mellem, hvordan adgangskontrol kan omgås, og hvordan fejlene kan forebygges.

Fra videoerne tager jeg især deny by default og princippet om at kontrollere adgang ved hver handling med mig. De avancerede afsnit kommer også ind på rate limiting, automatiserede tests, rettigheder i databasen, kontekst og logging af afviste adgangsforsøg. Mine faglige noter fra videoerne og skærmbillederne ligger under [forsvar mod adgangskontrolfejl](../emner/adgangskontrol.md#forsvar-mod-adgangskontrolfejl).

### Secure Coding: Preventing Broken Access Control

[Secure Coding: Preventing Broken Access Control](https://www.pluralsight.com/courses/secure-coding-preventing-broken-access-control) af Gavin Johnson-Lynn.

Jeg vil starte med Finding Insecure Direct Object References (IDOR) og Forced Browsing to Find Hidden Functionality. Kurset er sidst opdateret i 2020, så jeg bruger OWASP 2025 som reference for kategorien. Jeg har ikke vurderet videoerne endnu.

Jeg vil undersøge, om videoerne forklarer noget bedre end teksterne, og hvilke dele der er relevante, når man allerede kender authorization.

Min vurdering efter at have set afsnittene:

Rigtig godt til at få en grund forståelse af, hvad adgangskontrol er, og hvordan man kan finde og rette fejl. Jeg vil især bruge videoerne til at forklare årsagen til fejlen.

A:11 next steps -> undersøg nemmere, om der er andre relevante afsnit.

## WebGoat

[OWASP WebGoat](https://owasp.org/projects/webgoat)

Jeg vil finde og prøve en relevant øvelse om adgangskontrol. Det er interessant at se emnet i en Java-applikation. Jeg vil især undersøge opsætning, vejledning og muligheden for at forbinde øvelsen med kode og rettelser.

Valgt øvelse:

Min vurdering efter afprøvningen:

## OWASP Authorization Cheat Sheet

[Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)

Jeg bruger den til at undersøge, hvordan fejlen forebygges. Jeg vil vurdere, hvilke afsnit der er nyttige som reference til mit Java-eksempel.

Min vurdering efter brug:

## Min samlede vurdering

Skrives efter afprøvningen. Hvilke materialer hjalp mig bedst med at forstå, finde og rette fejlene?

## Ekstra læsning

I [ekstra materiale](ekstra-materiale.md) har jeg samlet forslag til uddybning. Til dette forløb er det især OWASPs IDOR- og sessionstekster samt udvalgte kapitler fra API Security in Action.

# Ugentlig Overblik Morten

*Nyeste uge øverst*

## Uge 47
**A07 Authentication Failures**  
Jeg har læst OWASP A07:2025 og samlet de typiske fejl i fire grupper: gæt og automatiserede angreb, svage adgangskoder og opbevaring, loginforløb samt sessioner og tokens. Jeg har også set Pluralsight-kurset OWASP: Authentication Failures. Det jeg tager med: MFA, credential stuffing ved genbrugte adgangskoder, rate limiting, CIA, statisk kodeanalyse (SonarQube, Semgrep, CodeQL), tjek af adgangskoder mod Pwned Passwords og nyt session-ID ved login. Jeg har lavet en liste over, hvad jeg vil undersøge i miseOS' login og JWT-håndtering. PortSwiggers authentication-labs og 2FA simple bypass mangler endnu.  
[A07 – Authentication Failures](emner/authentication.md)  
[OWASP A07:2025](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)  
[Pluralsight – OWASP: Authentication Failures](https://www.pluralsight.com/courses/owasp-authentication-failures)

**Java4Shell (CVE-2026-114938)**  
Ny fejl i OpenJDK's Base64-dekoder, offentliggjort 17. november. Den kan give RCE uden login, men kun på CPU'er med AVX-512 VBMI. Jeg har undersøgt min droplet: CPU'en har kun `bmi2` og mangler AVX-512, og `UseBASE64Intrinsics` er `false` i miseOS. Fejlen ser derfor ikke ud til at kunne udnyttes på dropletten lige nu, men Java skal stadig opdateres. Java-versionen i containerne  mangler at blive kontrolleret.  
[Java4Shell – undersøgelse af min droplet](emner/java4shell.md)  
[Tenable – Java4Shell FAQ](https://www.tenable.com/blog/java4shell-cve-2026-114938-frequently-asked-questions-about-openjdk-rce-vulnerability)

Java4Shell hører under A03 (sårbar komponent), ikke A06 Insecure Design. Pointen er, at et sikkert design ikke forhindrer en fejl i runtime'en, men kan begrænse skaden (ikke-root container, mindst mulige rettigheder).

**CIA – fortrolighed, integritet og tilgængelighed**  
CIA nævnes i næsten alle kurserne, så jeg bruger det til at vurdere konsekvensen af en fejl:
- **Fortrolighed (Confidentiality):** Kun de rigtige kan se data. Det brydes fx, når en bruger kan se en anden brugers kurv eller dokument (A01).
- **Integritet (Integrity):** Data kan kun ændres af de rigtige og på den rigtige måde. Det brydes fx med en falsk anmeldelse i en andens navn eller ved RCE.
- **Tilgængelighed (Availability):** Systemet virker, når det skal bruges. Det kan brydes af et angreb, men også af forsvaret, fx en kontolås, der låser den rigtige bruger ude.

Java4Shell rammer alle tre: hukommelsen kan læses (C), kode kan køres (I), og et mislykket forsøg kan få JVM'en til at gå ned (A).  
[CIA i A07-noterne](emner/authentication.md#cia)

**Idé: tjek mod Pwned Passwords ved oprettelse**  
Ud fra Pluralsight-modulet om password-politik har jeg skrevet en idé til en Java-klasse til miseOS. Den afviser adgangskoder fra kendte læk via Have I Been Pwned's range-API ved oprettelse og ved skift af adgangskode. Den er ikke afprøvet endnu.  
[Kodeidé og password-politik](emner/authentication.md#kodeidé-tjek-mod-pwned-passwords-ved-oprettelse)


## Uge 44–46
*Samlet tilbageblik på de første uger med A01 Broken Access Control.*

**Teori**  
Jeg har læst OWASP A01 og A07. Det jeg især tog med: deny by default, adgang kontrolleres ved hver request på serveren, og et login er ikke i sig selv tilladelse til andres data.  
[Login, sessioner og adgangskontrol](emner/adgangskontrol.md)

**Pluralsight**  
Jeg har arbejdet i en [learning path](https://app.pluralsight.com/learning-paths/78b4f3d0-3a96-4e41-9473-7b69553b5266) og set:
- [OWASP Top 10: The Big Picture](https://www.pluralsight.com/courses/owasp-secure-coding-big-picture) (Hampton Paulk, ca. 1½ time) – overblik over hele listen
- [OWASP: Broken Access Control](https://www.pluralsight.com/courses/owasp-broken-access-control) (Gavin Johnson-Lynn) – det bedste af dem til A01
- [OWASP: Injection](https://www.pluralsight.com/courses/owasp-injection) (Gavin Johnson-Lynn, 40 min.) – SQL, NoSQL, command, template injection og XSS. A05 er ikke en del af mine forløb endnu.

**PortSwigger**  
Jeg har løst de 13 labs i afsnittet om adgangskontrol. Det gav god træning i Burp Suite og i at ændre requests. Det jeg lærte af IDOR-labben: GUID'er gør ID'er sværere at gætte, men de erstatter ikke adgangskontrol.

**Campfire**  
Jeg har gennemført A01 - Broken Access Control (100 XP), basic-forløbet og den grundlæggende sikkerhedsgennemgang.

**Juice Shop**  
Jeg har arbejdet med opgaverne i kategorien Broken Access Control og er nået til 4 stjerner. Opgaverne i kategorien op til 4 stjerner:
- ⭐ Web3 Sandbox
- ⭐⭐ Admin Section, AI Debugging, Five-Star Feedback, View Basket
- ⭐⭐⭐ CSRF, Forged Feedback, Forged Review, Manipulate Basket, Product Tampering
- ⭐⭐⭐⭐ Easter Egg

Mange af dem er samme fejl som i PortSwigger: serveren tjekker ikke, om brugeren ejer kurven, anmeldelsen eller feedbacken.  
[Juice Shop – Broken Access Control](https://pwning.owasp-juice.shop/companion-guide/latest/part2/broken-access-control.html)

**WebGoat**  
Jeg er gået i gang med A01. Den første opgave var rigtig svær: at gennemskue, hvordan session-ID'et bliver genereret, så man kan forudsige og overtage en anden brugers session. Jeg er nu i gang med de resterende 3 opgaver. Opgaven hænger også sammen med A07, fordi forudsigelige session-ID'er er en fejl i sessionhåndteringen.

**Path traversal og container som root**  
Jeg har løst PortSwiggers lab om path traversal og prøvet det samme i FOG. Jeg fandt ikke et hul, men jeg opdagede, at containeren kørte som root. Jeg lærte, at applikationens rettigheder afgør, hvor stor skaden bliver, hvis der er et hul.  
[Undersøgelse af FOG-containeren](emner/afhaengigheder.md#undersøgelse-af-min-fog-container)

**miseOS**  
Jeg har forberedt en testmatrix med 12 starttests til adgangskontrollen. Testene er ikke kørt endnu.  
[Testmatrix til miseOS](emner/adgangskontrol/miseos-adgangskontrol.xlsx)


## Uge XX (Skabelon)
**Emne X**  
Her beskriver jeg hvad jeg har undersøgt / arbejdet på siden sidste uge  
[filnavn](link-til-internt-dokument)

**Emne Y**  
Her beskriver jeg hvad jeg har undersøgt / arbejdet på siden sidste uge  
[filnavn](link-til-internt-dokument)  
[ressource](url)

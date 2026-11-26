# Ugentlig Overblik Morten

*Nyeste uge øverst*

## Uge 48
**PortSwigger – Authentication vulnerabilities**  
Jeg har løst labs i PortSwiggers afsnit om authentication. Det jeg især tager med er, at login er et forløb og ikke ét tjek. Hvert trin skal kunne holde til, at angriberen springer over, gentager eller ændrer det. Brugernavne kan findes ud fra fejlbeskeden, men også ud fra små forskelle i teksten eller svartiden. Ved 2FA simple bypass var jeg allerede logget ind efter adgangskoden, så jeg kunne gå direkte til kontosiden uden kode. Når jeg selv bygger login, skal svaret være ens (tekst, statuskode og tid), der skal være rate limiting, og 2FA-status skal gemmes på serveren. Tokens til "husk mig" og nulstilling skal være tilfældige.  
[PortSwigger-noter under A07](emner/authentication.md#portswigger--authentication-vulnerabilities)  
[PortSwigger – Authentication](https://portswigger.net/web-security/authentication)

**PortSwigger – JWT attacks**  
Jeg har læst JWT-afsnittet og løst labs. Et JWT er kun kodet og ikke krypteret, så alle kan læse payloaden. Det er kun signaturen, der beskytter det. Alle angrebene går ud på, at serveren stoler på noget i tokenet, som angriberen selv bestemmer: `alg: none`, en nøgle i `jwk`/`jku`, en `kid` brugt som filsti eller en algoritme valgt ud fra headeren. Til web development tager jeg med: verificér altid signaturen, lås algoritmen på serveren, hent nøglen fra serverens konfiguration, brug en lang tilfældig nøgle og tjek `exp` og `iss`. JWT hører mest under A07, fordi tokenet beviser, hvem brugeren er. Der er overlap med A01 (ændring af `role`) og A04 (svag nøgle).  
[JWT – JSON Web Tokens](emner/jwt.md)  
[PortSwigger – JWT attacks](https://portswigger.net/web-security/jwt)

**Nimbus JOSE+JWT i miseOS**  
Jeg har undersøgt, hvor Nimbus kan gøre et program svagt: `SignedJWT.parse()` verificerer ikke selv, `JWTParser` kan returnere usignerede tokens, `verify()` tjekker ikke `exp` og `iss`, og header-værdier som `jwk` og `kid` er input fra angriberen. Derefter har jeg gennemgået min egen JWT-kode i miseOS og tjekket angrebene fra labs mod den. Ingen af dem virker: signaturen verificeres før claims læses, `alg: none` afvises, nøglen kommer kun fra serverens miljøvariabel (32 tilfældige tegn), og headerværdier som `jwk`, `jku` og `kid` bruges ikke. Mine egne valg har konsekvenser, som jeg har beskrevet: tokenet gælder i 30 minutter, rollen ligger i tokenet, logout sker kun på klienten, og tokenet ligger i `localStorage`, hvor et XSS-hul vil kunne stjæle det. Jeg har også tjekket for hardkodede hemmeligheder: koden er ren, men en fil med loginoplysninger til mine testbrugere og JWT'er lå i den offentlige git-historik i knap en måned. Den er fjernet nu, men ligger stadig i historikken, så adgangskoderne skal skiftes.  
[miseOS – min egen analyse af JWT-koden](emner/jwt.md#miseos--min-egen-analyse-af-jwt-koden)

**Brugernavne og brute force i miseOS**  
Fra PortSwigger har jeg samlet alle de tegn, der kan afsløre et rigtigt brugernavn eller en rigtig adgangskode: fejlbesked, små forskelle i teksten, svarets længde, statuskode, svartid, kontolås og cookies. Jeg har også skrevet, hvordan en angriber finder brugernavne, før brute force går i gang. Derefter har jeg holdt mine egne endpoints op mod de samme tricks. Login giver samme fejlbesked og statuskode, men svartiden afslører, om e-mailen findes, fordi BCrypt kun køres for rigtige brugere. Registreringen afslører det direkte med `409 Conflict`. Og da miseOS ikke har rate limiting, er login sårbart over for brute force. Jeg har lavet en plan for at rette det, og næste skridt er at vise fundene med Burp.  
[Brugernavne, brute force og miseOS](emner/authentication.md#hvordan-finder-en-angriber-brugernavne-og-adgangskoder)

**Juice Shop**  
Jeg er i gang med Broken Authentication og Injection. Hver opgave dokumenterer jeg ud fra min skabelon, så jeg kan se, hvordan jeg løste den, og bruge fremgangsmåden igen. Injection er ikke relevant for miseOS, som bruger Hibernate med parameteriserede queries.

Et eksempel, der gav meget: I *Bjoern's Favorite Pet* nulstillede jeg en adgangskode med svaret på et sikkerhedsspørgsmål, som jeg fandt i en video på nettet. Opgaven viser, hvor farlig offentlig information (OSINT) er, og den fik mig til at se på, hvad der ligger offentligt om miseOS, fx e-mails i git-historikken.  
[Bjoern's Favorite Pet](ovelser/juice-shop/broken-authentication/bjoerns-favorite-pet.md)  
[Min metode og skabelon](ovelser/juice-shop/README.md)  
[Mine Juice Shop-noter](ovelser/juice-shop/)


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

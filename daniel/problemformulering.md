# Problemformulering

### Problemstilling

Autentificering og adgangskontrol er fundamentet for beskyttelsen af brugerdata i webapplikationer. Broken Access Control er placeret som nr. 1 på OWASP Top 10:2025, og Authentication Failures som nr. 7. Svagheder på disse områder kan give uautoriserede aktører adgang til andre brugeres data eller fuld kontrol over deres konti.

Sheet Herder er en webapplikation til administration af karakterark til tabletop-rollespil (TTRPG). Systemet består af en React-baseret single-page application og et REST API udviklet i Java med Javalin, JPA/Hibernate og PostgreSQL. Det driftes i Docker-containere bag Caddy som reverse proxy. Brugere opretter en konto og vedligeholder private ressourcer, som kun ejeren må have adgang til.

Brugeren autentificerer sig med e-mail og password, hvorefter API'et udsteder et JWT. Klienten opbevarer tokenet og sender det med ved efterfølgende requests. Autorisation er placeret i rollebaserede filtre og ejerskabskontrol i API'ets servicelag og suppleres af kontroller i klienten.

Systemet er udviklet med fokus på funktionalitet. Den eksisterende testsuite verificerer tilsigtet brug, men ikke modstandsdygtighed over for misbrug. Desuden er sikkerhedskontrollerne fordelt mellem klient og server. Det rejser spørgsmålet, om de håndhæves der, hvor en angriber ikke kan omgå dem. Systemet behandler personoplysninger i form af e-mailadresser, så svagheder i adgangskontrollen kan få konsekvenser for brugernes privatliv og ikke kun for systemet.

Der er derfor behov for en systematisk sårbarhedsanalyse af, om systemets autentificering og adgangskontrol beskytter brugernes konti og data, og hvordan beskyttelsen kan styrkes og verificeres.

### Problemformulering

> Med udgangspunkt i OWASP Top 10:2025 vil vi gennem en sårbarhedsanalyse undersøge, i hvilket omfang Sheet Herder beskytter brugerkonti og tilhørende data mod Broken Access Control (A01) og Authentication Failures (A07), og hvordan beskyttelsen kan styrkes og verificeres.

### Underspørgsmål

1. Hvilke aktiver, trusselsaktører og misbrugsscenarier er relevante for systemets autentificering og adgangskontrol, og hvilken sikkerhedsadfærd skal systemet derfor udvise? *(KRAV)*
2. Hvilke etiske og juridiske rammer gælder for sårbarhedsanalysen, og hvordan etableres et testmiljø, der overholder dem og gør analysen reproducerbar? *(RAMMER)*
3. I hvilket omfang udviser API'et den forventede sikkerhedsadfærd, og hvordan håndhæves autentificering og adgangskontrol, når klienten omgås? *(TEST API)*
4. I hvilket omfang udviser klienten den forventede sikkerhedsadfærd, og hvordan bidrager dens håndtering af sessioner og adgang til systemets samlede beskyttelse? *(TEST KLIENT)*
5. Hvilke tiltag kan ud fra analysens resultater styrke systemets autentificering og adgangskontrol, og hvordan kan det verificeres, at beskyttelsen virker uden at påvirke systemets normale brug? *(FORBEDRING)*

### Afgrænsning

Analysen omfatter Sheet Herders autentificering og adgangskontrol. Det gælder API'ets endpoints og CORS-konfiguration samt klientens håndtering af tokens, logout, beskyttede ruter og bekræftelse af password ved kontoændringer. Hashing af passwords og signering og validering af JWT indgår, i det omfang de har betydning for autentificeringen. Analysen gennemføres på bestemte commits af begge repositories, som angives i bilag. Den foregår udelukkende i et lokalt testmiljø, og produktionsmiljøet testes ikke.

Følgende er ikke omfattet:
- **Øvrige OWASP-kategorier**, herunder A02 Security Misconfiguration, A03 Software Supply Chain Failures, A04 Cryptographic Failures (ud over det, der er nævnt ovenfor) og A05 Injection. XSS vurderes kun i det omfang, det er relevant for kompromittering af tokens.
- **Infrastruktur og hosting** (Caddy, DigitalOcean), herunder HTTP-sikkerhedsheadere. API'ets egen CORS-konfiguration er dog omfattet, da den hører under A01.
- **Denial-of-service**, herunder belastningstest.
- **Social engineering**, herunder phishing og andre angreb, der retter sig mod brugerne frem for systemet.
- **Ikke-implementeret funktionalitet**, herunder campaigns.
- **Eksterne tjenester**, som API'et henter data fra, da der ikke er tilladelse til at teste dem.

Observationer uden for afgrænsningen noteres som anbefalinger.
# Problemformulering

### Problemstilling

Autentificering og adgangskontrol er fundamentet for beskyttelsen af brugerdata i webapplikationer. Broken Access Control er placeret som nr. 1 på OWASP Top 10:2025, og Authentication Failures som nr. 7. Svagheder på disse områder kan give uautoriserede aktører adgang til andre brugeres data eller fuld kontrol over deres konti.

Sheet Herder er en webapplikation til administration af karakterark til tabletop-rollespil (TTRPG). Systemet består af en React-baseret single-page application og et REST API udviklet i Java med Javalin, JPA/Hibernate og PostgreSQL. Det driftes i Docker-containere bag Caddy som reverse proxy. Brugere opretter en konto og vedligeholder private ressourcer, som kun ejeren må have adgang til.

Brugeren autentificerer sig med e-mail og password, hvorefter API'et udsteder et JWT. Klienten opbevarer tokenet og sender det med ved efterfølgende requests. Autorisation håndhæves af rollebaserede filtre og ejerskabskontrol i API'ets servicelag og suppleres af kontroller i klienten.

Systemet er udviklet med fokus på funktionalitet. Den eksisterende testsuite verificerer tilsigtet brug, men ikke modstandsdygtighed over for misbrug. Desuden er sikkerhedskontrollerne fordelt mellem klient og server. Det rejser spørgsmålet, om de håndhæves der, hvor en angriber ikke kan omgå dem. Systemet behandler personoplysninger i form af e-mailadresser. Derfor er utilstrækkelig adgangskontrol ikke kun en teknisk risiko, men også relevant i forhold til kravet om passende sikkerhed i GDPR artikel 32.

Der er derfor behov for en systematisk sårbarhedsanalyse af, om systemets autentificering og adgangskontrol beskytter brugernes konti og data, og hvordan eventuelle svagheder kan afhjælpes.

### Problemformulering

> Med udgangspunkt i OWASP Top 10:2025 vil vi gennem en sårbarhedsanalyse undersøge, i hvilket omfang Sheet Herder beskytter brugerkonti og tilhørende data mod Broken Access Control (A01) og Authentication Failures (A07), og hvordan beskyttelsen kan styrkes og verificeres.

### Underspørgsmål

1. Hvilke aktiver, trusselsaktører og misbrugsscenarier er relevante for systemets autentificering og adgangskontrol, og hvilken sikkerhedsadfærd skal systemet derfor udvise?
2. Hvilke etiske og juridiske rammer gælder for sårbarhedsanalysen, og hvordan etableres et autoriseret og reproducerbart testmiljø?
3. I hvilket omfang overholder systemet den forventede sikkerhedsadfærd, når autentificering, sessionshåndtering, kontoadministration og ejerskabskontrol testes både via klienten og direkte mod API'et? **RET DETTE OG ADSKILD API OG FRONTEND**
4. Hvilke afhjælpende tiltag bør prioriteres, og hvordan kan automatiserede tests dokumentere, at de virker uden at forringe legitim funktionalitet? **OMFORMULER DENNE**

### Afgrænsning

Analysen omfatter Sheet Herders autentificering og adgangskontrol. Det gælder API'ets endpoints under `/auth`, `/users` og `/character-sheets` samt klientens håndtering af tokens, logout, beskyttede ruter og bekræftelse af password ved kontoændringer. Analysen gennemføres på bestemte commits af begge repositories, som angives i bilag. Den foregår udelukkende i et lokalt testmiljø, og produktionsmiljøet testes ikke. Analysen afgrænses til 3–5 problemstillinger, der behandles i dybden.

Følgende er ikke omfattet:
- **Offentlige reference-endpoints**, fordi de kun udstiller data, der kan læses, og som ikke er knyttet til brugere.
- **Øvrige OWASP-kategorier**, herunder A02 Security Misconfiguration, A03 Software Supply Chain Failures og A05 Injection. XSS vurderes kun i det omfang, det er relevant for kompromittering af tokens. Observationer uden for scope noteres som anbefalinger.
- **Infrastruktur og hosting** (Caddy, DigitalOcean), herunder HTTP-sikkerhedsheadere.
- **Denial-of-service, social engineering og funktionalitet, der ikke er implementeret** (campaigns).

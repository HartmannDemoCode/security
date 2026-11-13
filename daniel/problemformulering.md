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
3. I hvilket omfang overholder systemet den forventede sikkerhedsadfærd, når autentificering, sessionshåndtering, kontoadministration og ejerskabskontrol testes både via klienten og direkte mod API'et?
4. Hvilke afhjælpende tiltag bør prioriteres, og hvordan kan automatiserede tests dokumentere, at de virker uden at forringe legitim funktionalitet?

### Afgrænsning

Analysen omfatter Sheet Herders autentificering og adgangskontrol. Det gælder API'ets endpoints under `/auth`, `/users` og `/character-sheets` samt klientens håndtering af tokens, logout, beskyttede ruter og bekræftelse af password ved kontoændringer. Analysen gennemføres på bestemte commits af begge repositories, som angives i bilag. Den foregår udelukkende i et lokalt testmiljø, og produktionsmiljøet testes ikke. Analysen afgrænses til 3–5 problemstillinger, der behandles i dybden.

Følgende er ikke omfattet:
- **Offentlige reference-endpoints**, fordi de kun udstiller data, der kan læses, og som ikke er knyttet til brugere.
- **Øvrige OWASP-kategorier**, herunder A02 Security Misconfiguration, A03 Software Supply Chain Failures og A05 Injection. XSS vurderes kun i det omfang, det er relevant for kompromittering af tokens. Observationer uden for scope noteres som anbefalinger.
- **Infrastruktur og hosting** (Caddy, DigitalOcean), herunder HTTP-sikkerhedsheadere.
- **Denial-of-service, social engineering og funktionalitet, der ikke er implementeret** (campaigns).

--- 

## Udkast til problemformulering

**1. Åbent spørgsmål** (●○○○○)
> Hvor godt beskytter Sheet Herder sine brugeres konti og data, og hvordan kan beskyttelsen forbedres?

Den er let at læse, men har ingen faglig ramme. OWASP, metode og verifikation skal alt sammen komme fra underspørgsmålene. Den er for løs til en sårbarhedsanalyse.

**2. Traditionelt spørgsmål med fagligt grundlag** (●●○○○)
> I hvilket omfang beskytter webapplikationen Sheet Herder brugerkonti og tilhørende data mod sårbarheder omfattet af OWASP Top 10:2025-kategorierne A01 Broken Access Control og A07 Authentication Failures, og hvordan kan beskyttelsen styrkes og effekten heraf verificeres?

Den dækker hvad og kontekst, men ikke hvordan. Det er netop den kritik, du har fået.

**3. "Vi vil" i to sætninger** (●●●○○)
> Vi vil undersøge, i hvilket omfang Sheet Herder beskytter brugerkonti og tilhørende data mod Broken Access Control og Authentication Failures (OWASP Top 10:2025, A01 og A07), og hvordan beskyttelsen kan styrkes. Undersøgelsen gennemføres som en sårbarhedsanalyse af systemets kode og adfærd, hvor afhjælpende tiltag verificeres med automatiserede tests.

Den dækker alle tre led og læses naturligt. Til gengæld er den lidt lang, og metoden står for sig selv i anden sætning.

**4. "Vi vil" i én flydende sætning** (●●●●○)
> Med udgangspunkt i OWASP Top 10:2025 vil vi gennem en sårbarhedsanalyse undersøge, i hvilket omfang Sheet Herder beskytter brugerkonti og tilhørende data mod Broken Access Control (A01) og Authentication Failures (A07), og hvordan beskyttelsen kan styrkes og verificeres.

Den dækker alle tre led i én sætning:
- **Grundlag:** "med udgangspunkt i OWASP".
- **Hvordan:** "gennem en sårbarhedsanalyse".
- **Hvad og kontekst:** resten af sætningen.

Strukturen er den samme som i skabelonen, men formuleret, så den ikke lyder som en formular.

**5. Det strenge format** (●●●●●)
> Vi vil, baseret på OWASP Top 10:2025-kategorierne A01 Broken Access Control og A07 Authentication Failures og ud fra en white-box-sårbarhedsanalyse, undersøge i hvilket omfang webapplikationen Sheet Herder beskytter brugerkonti og tilhørende data, samt hvordan beskyttelsen kan styrkes og verificeres.

Den er præcis, men tung. Indskuddet mellem "Vi vil" og "undersøge" er så langt, at læseren skal holde tråden i næsten 30 ord.

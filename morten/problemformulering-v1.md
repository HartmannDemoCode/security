# Oplæg til problemformulering (v1)

**Emne:** Sikkerhedsforbedringer i miseOS – autentifikation og adgangskontrol

**Foreløbigt rapportfokus til drøftelse:** A01:2025 Broken Access Control og A07:2025 Authentication Failures, svarende til tema 1 (*Authorization and data ownership*) og tema 2 (*Authentication and sessions*) i [rapportvejledningen](../report_content.md). Jeg arbejder fortsat med alle ti Top 10-kategorier i mit læringsforløb, og fokusvalget kvalificeres gennem det arbejde.

## Problemstilling

### miseOS og dets brugere

miseOS er en platform til planlægning i professionelle kantinekøkkener, som jeg udviklede som eksamensprojekt på 3. semester. Systemet erstatter papir, Word-dokumenter og mundtlige aftaler med én digital arbejdsgang: Kokke foreslår retter og bestiller råvarer, køkkenledelsen godkender forslag og anmodninger og bygger ugemenuen, godkendte råvareanmodninger samles til indkøbslister, og menuen publiceres til gæsterne. Kunder kan desuden bestille takeaway.

Adgangen styres af fire roller: `CUSTOMER`, `LINE_COOK`, `SOUS_CHEF` og `HEAD_CHEF`. Systemet er deployet offentligt og bliver ifølge projektets dokumentation brugt og testet i et rigtigt køkken. Sikkerheden har altså betydning for rigtige brugere og ikke kun for et skoleprojekt.

### Sådan er sikkerheden bygget i dag

Backenden er et REST API i Java 17 med Javalin, Hibernate og PostgreSQL. Frontenden er en React-applikation. Brugeren logger ind med email og password, som er hashet med bcrypt, og får et JWT, som frontenden gemmer i browseren og sender med hvert kald. På serveren kontrollerer et filter tokenet, før kaldet når controlleren. Hver route har et rollekrav, og visse services kontrollerer desuden, om brugeren ejer den ressource, der bliver efterspurgt.

Frontenden skjuler knapper og sider, som en rolle ikke skal bruge. Det beskytter ikke noget i sig selv, fordi API'et kan kaldes direkte. Den reelle beskyttelse ligger derfor i backendens kontrol af token, rolle og ejerskab.

### Hvorfor det er et problem

Da jeg byggede miseOS, var fokus på funktionalitet og arkitektur. Projektet har integrationstests, som også kontrollerer en del afvisninger med 401 og 403. De er dog skrevet ud fra mine egne antagelser om, hvad der skulle virke, og ikke ud fra en systematisk gennemgang af, hvordan nogen kunne misbruge systemet. Jeg ved derfor ikke, om kontrollerne holder, når en bruger bevidst ændrer et id i et kald, genbruger et gammelt token eller kalder en funktion, som rollen ikke burde have adgang til. Mine noter viser også steder, hvor dokumentationen og koden ikke er enige om, hvem der må hvad.

Konsekvenserne kan være konkrete. En kok, der kan godkende sine egne råvareanmodninger, påvirker køkkenets indkøb. En menukladde, der er offentlig for tidligt, kan give gæsterne forkerte oplysninger om retter og allergener. En bruger, der kan ændre andres profil eller rolle, kan i værste fald overtage en anden konto. Når frontenden logger en bruger ud, sletter den kun tokenet i browseren. Hvad serveren accepterer bagefter, har jeg ikke undersøgt.

Problemet er relevant ud over miseOS. Broken Access Control er placeret som nummer ét i OWASP Top 10:2025, og fejl i autentifikation er en selvstændig kategori (A07). Mange webapplikationer bygges som miseOS, med en frontend, der skjuler funktioner, og et API, der skal håndhæve reglerne. Derfor er det en typisk risiko, at reglerne kun bliver håndhævet i brugergrænsefladen.

Jeg vil derfor bruge den viden, jeg får i valgfaget, til at undersøge miseOS' eksisterende kontroller systematisk. På det grundlag vil jeg gennemføre og efterprøve udvalgte forbedringer.

## Problemformulering

**I hvilket omfang forhindrer miseOS' autentifikation og adgangskontrol uautoriseret adgang til udvalgte data og funktioner, og hvordan kan udvalgte forbedringer implementeres og efterprøves, så de styrker beskyttelsen uden at forringe funktionaliteten for applikationens brugere?**

## Underspørgsmål

| # | Underspørgsmål | Besvares i rapportens |
| --- | --- | --- |
| 1 | Hvilke data og funktioner skal beskyttes i de valgte arbejdsgange, hvem skal have adgang, og hvilke misbrugsscenarier er relevante? | Kap. 2 – begreber og trusselsmodel |
| 2 | Hvordan håndhæver miseOS i dag autentifikation samt ejerskabs- og rollekrav, og i hvilket omfang svarer den faktiske adfærd til den forventede? | Kap. 4 – fund |
| 3 | Hvilke risici medfører eventuelle afvigelser for miseOS, og hvordan bør de prioriteres? | Kap. 4 – risiko og prioritering |
| 4 | Hvilke forbedringer kan implementeres, og hvilke afvejninger mellem sikkerhed og brugervenlighed indebærer de? | Kap. 5 – design og implementering |

**Perspektivering:** Hvad viser gentest af misbrugsscenarier og test af tilladt brug om forbedringernes virkning, og hvilke risici og begrænsninger står tilbage? (Kap. 5–6 – verifikation og diskussion)

## Metode

**Forberedelse.** Jeg gennemgår OWASP Top 10 én kategori ad gangen (A01–A10). For hver kategori:

1. læser jeg teorien: hvad svagheden går ud på, og hvilke betingelser der skal være til stede,
2. løser jeg relevante pentest-labs for at se, hvordan svagheden findes og udnyttes i praksis,
3. undersøger jeg, hvordan man beskytter sig mod den, og hvordan en rettelse kan efterprøves,
4. afprøver jeg min viden i OWASP Juice Shop og WebGoat.

Erfaringerne samler jeg i [min fremgangsmåde til sikkerhedsundersøgelser](emner/fremgangsmaade.md), som jeg bruger, når jeg vælger tests og dokumenterer resultater i miseOS. Labs og øvelser er træning og indgår ikke som resultater i rapporten.

**Trusselsmodel.** Jeg opstiller en enkel trusselsmodel med værdier, angribere, indgangspunkter og tillidsgrænser. De forventede adgangsregler begrundes i systemets tilsigtede brug og afklares, hvor dokumentation og kode er forskellige – koden er det, jeg undersøger, og kan ikke alene afgøre, hvad der bør være tilladt. En medstuderende udfordrer modellen, og jeg noterer, hvad feedbacken ændrer.

**Undersøgelse.** Top 10 bruges til at vælge og forklare fokus. De konkrete tests vælges fra OWASP Web Security Testing Guide (fx WSTG-ATHZ-02) og eventuelt enkelte krav fra OWASP ASVS, med versioner noteret. Jeg kører udvalgte tests fra min forberedte testmatrix med Burp Suite på tværs af brugere og roller og sammenholder resultaterne med en gennemgang af backend-koden. For hvert resultat dokumenterer jeg forventet og faktisk adfærd, forudsætninger og evidens. Kontroller, der virker, dokumenteres også.

**Testmatrixen i rapporten.** En kort adgangsregeltabel med de udvalgte handlinger og roller indgår i trusselsmodellen som forventet adfærd. Resultaterne af de udvalgte tests vises i fund- og verifikationsafsnittet som før/efter-tabeller. Den fulde matrix med alle tests ligger i et bilag, så andre kan gentage undersøgelsen.

**Forbedring og verifikation.** For de svagheder, jeg retter, viser jeg koden før og efter, gentager angrebet og kontrollerer, at tilladt brug stadig virker. Succeskriterierne fastlægges før testene, fx:

- En `LINE_COOK` kan læse sin egen råvareanmodning, mens en anden `LINE_COOK` og en bruger uden login afvises – både i svaret og i data.
- Et gyldigt token accepteres, mens et udløbet token eller et token med ugyldig signatur afvises.

Foreslåede, men ikke implementerede rettelser markeres tydeligt som sådan.

## Afgrænsning

- **A01 – adgangskontrol:** ejerskab ved læsning og ændring af råvareanmodninger samt rollekrav ved godkendelse. Mulig udvidelse: beskyttelse af menukladder og HEAD_CHEFs adgang til andres profiloplysninger.
- **A07 – autentifikation:** login med korrekte og forkerte oplysninger samt JWT-validering (udløb og ugyldig signatur) på udvalgte beskyttede ruter. Mulig udvidelse: hvad serveren accepterer efter logout og efter ændringer i brugerens konto. miseOS har ingen logout-route på serveren, så den ønskede adfærd skal afklares først.
- **Uden for fokus:** WebSocket, SSE, eksterne tjenester, afhængigheder og fuld gennemgang af serveropsætning. Andre Top 10-kategorier kan indgå som perspektivering.

**Fund uden for fokus.** Gennemgangen af alle ti kategorier kan afsløre andre svagheder i miseOS. De registreres i en fundliste med kategori, kort evidens og foreløbig vurdering. Et fund tages kun ind i rapportens undersøgelse, hvis det har betydning for problemformuleringen, fx fordi det påvirker autentifikation eller adgangskontrol. Ellers nævnes alvorlige fund i diskussionen og handlingsplanen som anbefalinger til systemejeren. Ændrer et fund mit syn på, hvad der er vigtigst, tager jeg det op med Thomas, før fokus ændres.

Testene udføres kun i et lokalt testmiljø med egne testdata. Jeg registrerer backend- og frontendversion, opsætning og testkonti, så undersøgelsen kan gentages, og beskriver tilladelse samt etiske og juridiske rammer med kilder. Tokens, passwords og personoplysninger fjernes fra delt evidens. Finder jeg ingen fejl i en test, beskriver jeg, hvad jeg har undersøgt, og hvad jeg stadig ikke ved.

## Det vil jeg drøfte med min lærer

- Vejledningen nævner både "one or two" og "choose one" tema – er A01 + A07 (tema 1 og 2) passende inden for 8–12 normalsider?
- Hvordan skal fund fra andre Top 10-kategorier behandles – som perspektivering, i handlingsplanen eller slet ikke?
- Hvordan skal "tre til fem sikkerhedsproblemer" i rapportformatet forstås i forhold til mine fem–seks testscenarier?
- Hvilken implementeret forbedring forventes, hvis de undersøgte kontroller allerede virker? Er fx bedre automatiseret verifikation (integrationstests af adgangsregler) nok?
- Hvordan dokumenterer jeg Linux, etik/jura og samarbejde til eksamen, og hvad skal med i selve rapporten?
- Hvilken vejledning er styrende for strukturen – `report_content.md` eller `report_format.md` (fx kravet om et resumé til systemejeren)?

## Grundlag for udkastet

- [Rapportens indhold](../report_content.md) og [mulige rapportformater](../report_format.md).
- [Kildegrundlag om OWASP og testmetode](ressourcer/problemformulering-kildegrundlag.md).
- [Testmatrix og noter om adgangskontrol i miseOS](emner/adgangskontrol.md).
- Kildekode: [miseOS backend](https://github.com/mortenjenne/miseos) og [miseOS web](https://github.com/mortenjenne/miseos-web). Det undersøgte commit fastlåses, før testene går i gang.

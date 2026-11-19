# Login, sessioner og adgangskontrol

Status: I gang. PortSwiggers afsnit om adgangskontrol er læst, og de 13 labs i afsnittet er løst. Path traversal-labben er også løst. Campfires A01 - Broken Access Control, basic-forløb og grundlæggende sikkerhedsgennemgang er gennemført. Afprøvning af adgangskontrollen i egen Java-kode mangler endnu. A07-delen har nu sin egen plan: [Login og sessioner – A07](01-authentication.md).

OWASP Top 10:2025: A01 Broken Access Control og A07 Authentication Failures.

Jeg arbejder først med teori og øvelser i træningsmiljøer. Til sidst anvender jeg det, jeg har lært, i miseOS. Jeg behøver ikke nå hele forløbet på én gang.

## Det jeg vil finde ud af

Hvordan kan fejl i login, sessioner og adgangskontrol give adgang til en konto eller data, man ikke burde have adgang til? Kan vi bruge samme applikation til at undersøge begge kategorier og vise rettelserne i Java?

Jeg har styr på authorization i forvejen. Jeg vil bruge tiden på fejlene, konsekvenserne og den kode, der skal forhindre dem.

Jeg holder forskellen tydelig: A07 handler her om, hvorvidt systemet kan stole på brugerens identitet og loginforløb. A01 handler om, hvad den bruger må få adgang til. En bruger kan godt være korrekt logget ind og stadig kunne hente en anden brugers dokument.

Jeg startede med begge kategorier her, men A07 fyldte mere, end jeg regnede med. Derfor har jeg flyttet login, adgangskoder, 2FA og sessioner over i [en plan for sig](01-authentication.md) og [egne noter](../emner/authentication.md). Her fortsætter jeg med A01 og testene i miseOS.

Jeg skriver vurderinger af materialet i [ressourceundersøgelsen](../ressourcer/adgangskontrol.md). Forklaringer, requests, kode og testresultater samler jeg i [mine faglige noter](../emner/adgangskontrol.md).

## Læse om fejlene

- [x] Læse Description, How to prevent og Example attack scenarios i OWASP A01:2025.
- [x] Læse de samme afsnit i OWASP A07:2025 med fokus på loginforløb og sessioner.
- [x] Læse PortSwiggers afsnit Access control vulnerabilities and privilege escalation, herunder forklaringen af IDOR.
- [x] Undersøge adgang til andre brugeres data og adgang til funktioner for andre roller i PortSwiggers labs.
- [x] Undersøge, hvordan et manglende trin i et loginforløb kan give adgang, før brugeren er færdig med at logge ind.
- [x] Læse de relevante dele af OWASP Authorization Cheat Sheet om kontrol af rettigheder på serveren.
- [ ] Skrive med egne ord, hvordan fejlene kan opstå i en applikation, vi selv kunne have bygget.

Jeg vil have en forklaring af fejlene og nogle spørgsmål, jeg kan prøve af i øvelserne.

## Undersøge materialerne

- [x] Se hele Pluralsight-kurset OWASP: Broken Access Control. Jeg vurderer det som rigtig godt, især afsnittene om forsvar mod adgangskontrolfejl og de mere avancerede forsvar.
- [x] Gennemføre Campfires A01 - Broken Access Control (Completed, 100 XP).
- [x] Gennemføre Campfires basic-forløb og grundlæggende sikkerhedsgennemgang.
- [x] Se de relevante dele af Pluralsight-kurset om IDOR og forced browsing.
- [x] Undersøge WebGoats opsætning og finde en relevant øvelse om adgangskontrol.
- [x] Sammenholde forklaringerne fra OWASP, PortSwigger, Pluralsight og Campfire.
- [x] Notere, hvad der kræver forhåndsviden, og hvor en studerende kunne få brug for hjælp.

Her vurderer jeg materialerne. Selve løsningen af øvelserne hører til næste del. Jeg skriver tydeligt, hvad jeg har læst, set og selv afprøvet.

## Afprøve øvelserne

- [x] Løse en simpel path traversal-lab og undersøge egen Java-applikation. Jeg fandt ikke et hul i mine forsøg, men undersøgelsen førte til opdagelsen af root som containerens standardbruger.
- [x] Løse PortSwigger-labben User ID controlled by request parameter, with unpredictable user IDs. GUID'er gør ID'er sværere at gætte, men erstatter ikke adgangskontrol.
- [x] Løse de øvrige labs i adgangskontrolafsnittet, i alt 13. Min vurdering af træningen og overførslen til egne projekter ligger i ressourcenoterne.
- [ ] Gemme relevante requests og resultater, så jeg kan vise fejlene igen.
- [ ] Forklare, hvilken kontrol der mangler, og hvad konsekvensen er.
- [ ] Notere tidsforbrug, problemer med opsætning og den hjælp, jeg bruger.

Jeg har nu prøvet flere varianter af adgangskontrolfejl. Næste skridt er at undersøge WebGoat og afprøve en relevant adgangskontroløvelse. 2FA simple bypass og resten af loginøvelserne ligger nu i [A07-planen](01-authentication.md).

Jeg bruger mit lokale øvelsesmiljø eller platformens labs til afprøvningen.

### Afgrænset undersøgelse af miseOS

Til sidst bruger jeg [testopskriften i de faglige noter](../emner/adgangskontrol.md#systematisk-test-af-adgangskontrol-i-miseos) og [Excel-skabelonen](../emner/adgangskontrol/miseos-adgangskontrol.xlsx) til at overføre teorien og øvelserne til mit eget projekt. Matrixen er tilpasset miseOS API-dokumentation og lokal kode ved commit `8162c94`. Den har 94 ruter og 55 foreslåede tests. Når jeg når til miseOS, starter jeg med de 12 Start-tests; alle krav skal bekræftes, og ingen tests er endnu kørt.

- [ ] Udfylde testmiljø, version eller commit og afgrænsning i regnearket.
- [ ] Bekræfte adgangsreglerne for profiler, råvareanmodninger, roller og menuers DRAFT/PUBLISHED-tilstand. Afklare forskelle mellem dokumentation og kode.
- [ ] Oprette to LINE_COOK-brugere med hver deres råvareanmodninger samt særskilte SOUS_CHEF- og HEAD_CHEF-konti.
- [ ] Kontrollere miseOS-testversionens requests i Burp og udfylde pladsholdere, ID'er, datoer og bodies i matrixen.
- [ ] Gennemføre de 12 Start-tests og dokumentere både tilladt adgang, afvisninger og beskyttelse af DRAFT-data.
- [ ] Gemme anonymiseret evidens og forbinde resultater og eventuelle fund med MISE-test-ID'er.
- [ ] Genteste rettelser og beskrive, hvad undersøgelsen dækker, og hvad der stadig mangler.

## Lave og teste en forbedring i miseOS

- [ ] Bruge miseOS med to testbrugere og råvareanmodninger, der tilhører hver sin bruger.
- [ ] Dokumentere forventet og faktisk adgang med konkrete requests. Også dokumentere kontroller, der fungerer.
- [ ] Vælge en begrundet forbedring ud fra undersøgelsen og vise relevant kode før og efter. Ved en ejerskabsfejl rette kontrollen på serveren.
- [ ] Teste, at ejeren har adgang, mens en anden bruger bliver afvist.
- [ ] Teste, at en request uden en gyldig bruger bliver afvist.
- [ ] Gemme resultatet før og efter rettelsen.

Jeg bruger miseOS som afsluttende Java-projekt efter teori og øvelser og holder de valgte funktioner afgrænsede. Jeg behøver ikke en ny frontend eller et nyt login-system. FOG og platformenes labs kan bruges som øvelser undervejs. Hvis noget kun bliver en skitse, skriver jeg, at det ikke er afprøvet.

## Samle fund og vurdere materialerne

- [ ] Skrive en kort sårbarhedsanalyse med fejl, fremgangsmåde, konsekvens, rettelse og testresultat.
- [ ] Vurdere materialernes forklaringer, niveau, opsætning og øvelser ud fra det, jeg selv har prøvet.
- [ ] Vælge det materiale, jeg ville bruge i undervisningen, og begrunde valget.
- [ ] Beskrive, hvilken forhåndsviden og hjælp øvelsen kræver.
- [ ] Knytte mine fund til læringsmålene om sårbarhedsanalyse, forebyggelse, sikker kode og dokumentation.

Analysen ligger i de faglige noter. Vurderingen af materialerne ligger i ressourceundersøgelsen.

## Møde og opsamling

- [ ] Forberede et kort eksempel, jeg kan vise og forklare til mødet.
- [ ] Tage min vurdering af materialerne og mine åbne spørgsmål med.
- [ ] Drøfte, om arbejdsformen passer til valgfaget.
- [ ] Skrive, hvad vi blev enige om, og vælge næste undersøgelse.

## Det jeg vil stå med bagefter

- En dokumenteret adgangskontrolfejl og en afprøvet rettelse, eller en tydelig beskrivelse af det, der mangler.
- Et eksempel på en fejl i loginforløbet og en forklaring af, hvordan den adskiller sig fra adgangskontrolfejlen.
- En vurdering af materialerne baseret på min egen læsning og afprøvning.
- Et forslag til en øvelse i valgfaget med en begrundelse ud fra læringsmålene.

## Opsamling fra møder

Dato:

Hvor langt er jeg:

Det vi blev enige om:

Det jeg arbejder videre med:

## OWASP-kapitler

- [A01:2025 – Broken Access Control](https://top10.owasp.org/2025/A01_2025-Broken_Access_Control/)
- [A07:2025 – Authentication Failures](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)

Øvelser og supplerende materiale ligger i [ressourceundersøgelsen](../ressourcer/adgangskontrol.md).

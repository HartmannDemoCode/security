# Login, sessioner og adgangskontrol

Status: I gang. miseOS er valgt til den løbende systemundersøgelse. Testmatrixen er tilpasset API-dokumentation og lokal kode, men testene er endnu ikke kørt. Tidligere FOG-forsøg er bevaret nedenfor.

Her samler jeg det, jeg lærer om A01 Broken Access Control og A07 Authentication Failures, og det, jeg selv prøver af. Planen ligger i [forløbet](../forloeb/01-adgangskontrol.md), og vurderingen af materialerne ligger under [ressourcer](../ressourcer/adgangskontrol.md).

## Identitet og adgang

Mine forklaringer af loginforløb, sessioner og adgang til konkrete ressourcer. For hvert eksempel vil jeg skrive, hvilken kontrol der fejler, og om jeg undersøger A01 eller A07.

## Min forklaring af fejlen

Skrives efter læsning og afprøvning. Jeg vil forklare, hvilken adgang brugeren burde have, og hvad der går galt i applikationen.

## Forsvar mod adgangskontrolfejl

Noter fra Pluralsight-kurset **OWASP: Broken Access Control**, baseret på de sete videoer, mine stikord og skærmbilleder fra kurset.

### Grundprincipper

- **Deny by default:** Adgang afvises, medmindre den udtrykkeligt er tilladt. En bruger får kun de nødvendige beføjelser, eksempelvis til at administrere sin egen profil eller en begrænset basisrolle. Flere beføjelser tildeles bevidst; nye funktioner giver ikke automatisk adgang. At kunne redigere sin profil giver heller ikke ret til at ændre sin egen rolle.
- **Stol ikke på brugerinput:** Et bruger-ID, en rolle eller et ressource-ID i en request er ikke bevis for adgang. Serveren skal bruge den verificerede identitet og kontrollere, om brugeren må udføre handlingen på den konkrete ressource.
- **Complete mediation:** Adgangen kontrolleres ved hver adgang til en beskyttet ressource. Kurset fremhæver at undgå caching af adgangskontrolafgørelser, så en tidligere tilladelse ikke fortsat giver adgang efter en ændring af rettighederne.

### Flere lag af forsvar

| Forsvar fra kurset | Det jeg tager med |
| --- | --- |
| Rate limiting | Begræns antallet af requests for at modvirke brute force og automatiserede forsøg. Det supplerer kontrollen af rettigheder. |
| Unit- og integrationstests | Test både horisontal og vertikal adgangskontrol. En bruger uden rettigheder skal afvises fra beskyttede funktioner, og nye funktioner skal omfattes af testene. Test også den adgang, der skal være tilladt. |
| Kontrol uden for applikationskoden | Kurset viser separate databasebrugere til forskellige roller med tilpassede databaserettigheder som et muligt ekstra lag. Det knytter sig til least privilege, som jeg også undersøger i min container. |
| Kontekstbaseret kontrol | Placering, enhed, tidspunkt og IP-adresse kan indgå som ekstra betingelser for adgang. De erstatter ikke kontrollen af brugerens rettigheder. |
| Logging af afviste adgangsforsøg | Registrér afvisninger, så gentagne forsøg kan opdages og undersøges. Mange afvisninger kan være tegn på brute force eller andre angreb, især hvis de efterfølges af en vellykket request. En vellykket request skal undersøges i sammenhæng med forsøgene; den beviser ikke alene, at et angreb lykkedes. |

Det, jeg især tager med, er, at adgang skal tildeles bevidst og kontrolleres løbende. Et login er ikke i sig selv tilladelse til alle funktioner eller andre brugeres data. Jeg har endnu ikke afprøvet disse forsvar i et samlet kodeeksempel.

## Systematisk test af adgangskontrol i miseOS

miseOS er valgt som system til min undersøgelse og den senere rapport. Jeg vil undersøge det løbende, mens jeg lærer nye emner de næste syv uger. FOG er fortsat et tidligere øvelsesprojekt; mine allerede dokumenterede FOG-forsøg nedenfor er historik.

Jeg bruger en adgangsmatrix til at beskrive rettigheder og en testlog til at dokumentere konkrete forsøg i Burp Suite. Sammenhængen er krav → test → evidens → vurdering → eventuel forbedring → gentest. Jeg kan også dokumentere kontroller, der fungerer korrekt.

[Åbn Excel-matrixen til miseOS](adgangskontrol/miseos-adgangskontrol.xlsx).

Regnearket har fire faner:

| Fane | Indhold |
| --- | --- |
| Afgrænsning | Testmiljø, version, konti, testdata, arbejdsopskrift og statusoversigt. |
| Adgangsmatrix | 94 registrerede HTTP-, SSE- og WebSocket-ruter fra lokal kode med rollekolonner og plads til bekræftelse af krav. |
| Testlog | 55 foreslåede tests. De første 12 har prioriteten **Start**, resten **Næste** eller **Udvidelse**. |
| Kilder og afklaringer | API-dokumentation, kodegrundlag og syv spørgsmål til forskelle mellem dokumentation, kode og testmiljø. |

Der er endnu ikke udført sikkerhedstests fra dette ark. Metoder og stier kommer fra lokal kode, men jeg skal indsætte faktiske test-ID'er og bekræfte ønsket adfærd. Rollekolonnerne er ikke en uafhængig godkendelse af kodens regler.

### Grundlag og afgrænsning

Planen er tilpasset [miseOS API-dokumentationen](https://corral.dk/docs/miseos-api-doc/) og den lokale Java-kode ved commit `8162c94` fra 5. juni 2026. Koden blev fundet under `Dokumenter/Development/School/Semester3/Portfolio/miseOS` på min skole-OneDrive. Den fulde lokale sti er noteret i regnearkets kildefane. Dokumentationen er læst den 30. oktober 2026; den kørende applikation kan være en anden version.

Jeg starter lokalt med egne testdata og afgrænser den første undersøgelse til:

- Egen brugerprofil og udvalgte forskelle mellem køkkenroller.
- Læsning, ændring og godkendelse af råvareanmodninger (`ingredient-requests`).
- Offentlig adgang til publicerede menuer og beskyttelse af kladder (`weekly-menus`).

Retforslag, indkøbslister, takeaway og notifikationer er mulige udvidelser. At ruterne står i arket betyder ikke, at jeg har testet dem eller har valgt dem alle til rapporten. SQL injection, belastningstest, afhængigheder og deploymentkonfiguration indgår ikke i denne adgangskontrolmatrix.

Jeg noterer faktisk base-URL inklusive `/api/v1`, version eller commit og testdato før forsøgene. Dokumentationens index angiver `https://miseos-api.corral.dk/api/v1`, mens flere eksempler bruger `https://miseos.corral.dk/api/v1`. Jeg skal vælge mit faktiske testmiljø; ingen af de publicerede API-hosts er testet som led i denne planlægning.

### Roller, ejerskab og tilstand

miseOS har fire brugerroller: `CUSTOMER`, `LINE_COOK`, `SOUS_CHEF` og `HEAD_CHEF`. `KITCHEN_STAFF` er en gruppe for de tre køkkenroller, og `ANYONE` betyder offentlig adgang. Nyregistrering giver ifølge dokumentation og lokal service rollen `CUSTOMER`.

Jeg må ikke behandle SOUS_CHEF og HEAD_CHEF som samme rolle. Eksempelvis er brugerlisten og rolleændringer forbeholdt HEAD_CHEF i de læste ruter, mens begge ledelsesroller kan godkende råvareanmodninger.

| Handling | Uden login | CUSTOMER | LINE_COOK | SOUS_CHEF | HEAD_CHEF |
| --- | --- | --- | --- | --- | --- |
| `GET /users/me` | Afvis | Afklar dokumentationsforskel | Egen profil | Egen profil | Egen profil |
| `GET /users` | Afvis | Afvis | Afvis | Afvis | Tillad |
| `GET /ingredient-requests/{id}` | Afvis | Afvis | Kun egne | Tillad | Tillad |
| `PUT /ingredient-requests/{id}` | Afvis | Afvis | Egne, i tilladt tilstand | Tilladt tilstand | Tilladt tilstand |
| `PATCH /ingredient-requests/{id}/approve` | Afvis | Afvis | Afvis | Tilladt tilstand | Tilladt tilstand |
| `PATCH /users/{id}/role` | Afvis | Afvis | Afvis | Afvis | Tillad |
| `GET /weekly-menus/by-week` | Kun PUBLISHED | Kun PUBLISHED | Kun PUBLISHED | Afklar brug af adminroute | Afklar brug af adminroute |
| `GET /weekly-menus/by-week/admin` | Afvis | Afvis | Afvis | DRAFT og PUBLISHED | DRAFT og PUBLISHED |

Tabellen er et arbejdsgrundlag ud fra læste kilder. Jeg bekræfter de tilsigtede regler, før jeg bedømmer tests. En rolle, der må kalde en route, må ikke nødvendigvis arbejde på alle objekter eller i alle tilstande.

Jeg opretter to LINE_COOK-testbrugere, A og B, med hver deres råvareanmodninger samt én SOUS_CHEF og én HEAD_CHEF. Til stationstests bruger jeg to stationer med hver sin aktive ret. CUSTOMER-konti og takeaway-ordrer oprettes først, hvis jeg vælger den udvidelse.

### Første 12 tests

Jeg filtrerer Testlog på **Prioritet = Start**. Eksempelvis:

| Test-ID | Bruger | Request relativt til base-URL | Forventning efter bekræftelse af krav |
| --- | --- | --- | --- |
| MISE-01 | LINE_COOK A | `GET /users/me` | Egen profil vises. |
| MISE-03 | LINE_COOK A | `GET /ingredient-requests/<REQUEST_A>` | A's egen anmodning vises. |
| MISE-04 | LINE_COOK A | `GET /ingredient-requests/<REQUEST_B>` | B's anmodning udleveres ikke. |
| MISE-08 | LINE_COOK A | `PATCH /ingredient-requests/<REQUEST_A>/approve` | Afvis; anmodningen bliver ikke godkendt. |
| MISE-10 | SOUS_CHEF | `PATCH /users/<COOK_A>/role` | Afvis; brugerens rolle ændres ikke. |
| MISE-12 | Ingen | `GET /weekly-menus/by-week?week=<DRAFT_WEEK>&year=<YEAR>` | Kladdens data udleveres ikke. |

Pladsholdere som `<REQUEST_A>` erstattes med kendte ID'er fra mine testdata. Body-eksempler og datoer tilpasses også. Testloggen indeholder metode, sti/query, body eller ændring, forudsætninger, forventning, faktisk resultat, evidens og gentest. Alle tests begynder som **Ikke kørt**, og ingen krav er endnu markeret bekræftede.

### Opskrift i Burp: A forsøger at læse B's råvareanmodning

1. Opret en anmodning for A og en for B. Bekræft, at begge kan læse deres egen. Notér de faktiske ID'er som REQUEST_A og REQUEST_B.
2. Log ind som A. Find `GET /ingredient-requests/<REQUEST_A>` i **Proxy → HTTP history**, og vælg **Send to Repeater**.
3. Kontrollér, at requesten har A's `Authorization: Bearer <token>`. Send den uændret, og dokumentér den tilladte adgang som MISE-03.
4. Behold A's token, og skift kun anmodningens ID til REQUEST_B. Send requesten igen.
5. Undersøg svaret for B's beskyttede oplysninger. Dokumentér faktisk status, indhold, dato, version og evidens under MISE-04.
6. Ved en fejl undersøger jeg kontrollen i miseOS-koden. En eventuel rettelse gentestes med både ejerens tilladte adgang og den anden brugers afviste adgang.

Ved ændringstests gemmer jeg også ressourcens tilstand før og efter. Jeg bruger gyldige datoer, en gyldig body og en passende tilstand, så en valideringsfejl ikke skjuler kontrollen af rolle eller ejerskab. Separate disponible testressourcer hindrer, at godkendelse eller sletning ødelægger forudsætningerne for næste forsøg.

Ved test uden login fjerner jeg alle autentifikationsoplysninger. Udløbne JWT'er testes særskilt med deres faktiske udløbstid. Jeg har ikke fundet en logout-route i den læste SecurityRoute; en fremtidig logout-test kræver afklaring af, om kravet handler om klientens token eller serverinvalidering.

### Afklaringer før jeg konkluderer

Kilder og afklaringer indeholder blandt andet:

- **CUSTOMER og egen profil:** Dokumentationens endpointtabel angiver KITCHEN_STAFF, mens lokal `UserRoute` også tillader CUSTOMER på `/users/me`.
- **Andres email og password:** Dokumentationen siger kun egne felter, mens `UserService.validateOwnershipOrAdmin` også accepterer HEAD_CHEF. Passwordændring kræver stadig aktuelt password. Den ønskede administratoradgang skal afklares.
- **Menuer:** Lokal kode har en særskilt `/weekly-menus/by-week/admin`. Den offentlige route springer autentifikation over i den læste SecurityController. Jeg tester offentlig beskyttelse af DRAFT og ledelsesadgang hver for sig.
- **Indkøbslister:** Dokumentationen viser `PATCH /shopping-lists/{id}/items/ordered`, mens lokal route bruger `PUT /shopping-lists/{id}/items/order-all`.
- **WebSocket:** Dokumentationen kræver KITCHEN_STAFF, mens den lokale WS-route bruger manuel JWT-kontrol uden en Role-deklaration. Jeg skal afklare og eventuelt teste kunders adgang og levering til den rigtige modtager.

Disse observationer er ikke påviste sårbarheder. Jeg fastlægger den ønskede adfærd og testversionen, før jeg vurderer et resultat som en fejl.

### Evidens og begrænsninger

Regnearket ligger i `adgangskontrol/miseos-adgangskontrol.xlsx`. Jeg kan placere anonymiseret evidens i eksempelvis `adgangskontrol/evidence/MISE-04/`. Tokens, cookies, passwords og personoplysninger fjernes fra materiale, der deles eller afleveres.

Jeg bruger **Bestået**, når resultatet matcher et bekræftet krav, **Fejlet**, når det ikke matcher, og **Blokeret**, når forudsætningerne mangler. Uafklarede krav og en forkert route giver ikke en bestået adgangstest. HTTP-status alene er heller ikke tilstrækkelig: Jeg undersøger både indhold og faktisk effekt.

For hvert fund forbinder jeg test-ID med konsekvens, årsag i koden, eventuel rettelse og gentest. Jeg bevarer det oprindelige resultat, når jeg udfylder gentestens dato, version og resultat. Senere kan relevante kontroller blive automatiserede integrationstests.

Min konklusion skal beskrive de faktisk testede roller, ressourcer, handlinger og versioner. At de udvalgte tests består viser ikke, at hele miseOS er fri for adgangskontrolfejl. Der er endnu ikke udført tests fra denne matrix.

Metodens kilder ligger under [materiale til systematisk adgangskontroltest](../ressourcer/adgangskontrol.md#systematisk-adgangskontroltest-i-eget-projekt).

## Afprøvninger

For hver øvelse skriver jeg miljø, fremgangsmåde, relevante requests og resultat. Jeg noterer også, hvis jeg bruger hints eller en løsning.

Jeg holder resultaterne fra IDOR og loginøvelsen adskilt, selv om de indgår i samme forløb.

### IDOR med uforudsigelige bruger-ID'er

Jeg har løst PortSwigger-labben **User ID controlled by request parameter, with unpredictable user IDs**. Den viser en horisontal adgangskontrolfejl, hvor et GUID kan bruges til at få adgang til en anden brugers konto og API-nøgle.

GUID'er gør bruger-ID'er sværere at gætte, men gør ikke i sig selv adgangen sikker. Hvis en anden brugers GUID bliver kendt, skal serveren stadig kontrollere, om den aktuelle bruger må tilgå kontoen. Uforudsigelige ID'er erstatter ikke adgangskontrol.

### Path traversal – fra øvelse til egen applikation

Jeg har løst PortSwiggers lab om simpel path traversal, hvor en billedfunktion kan bruges til at hente filer uden for den tilladte mappe. Det fik mig til at undersøge, om noget tilsvarende kunne lade sig gøre i min egen Java-applikation, FOG.

Jeg kunne ikke finde et path traversal-hul i min applikation med de forsøg, jeg lavede. Det fik mig videre til at undersøge, hvilke filer og rettigheder applikationen ville have adgang til, hvis der fandtes et hul. Her opdagede jeg, at containerens standardbruger var root.

Det vigtigste, jeg tog med fra undersøgelsen, var, at den godt kan føre til en relevant sikkerhedsforbedring, selv om jeg ikke finder den sårbarhed, jeg begyndte med at lede efter. Det gjorde least privilege konkret for mig: Applikationens rettigheder har betydning for, hvor stor skaden kan blive ved en fejl. [Min undersøgelse af containeren](afhaengigheder.md#undersøgelse-af-min-fog-container) ligger under sikker opsætning.

Jeg har ikke påvist path traversal i FOG. Mine forsøg er heller ikke nok til at konkludere, at applikationen er fri for den type fejl.

## Sårbarhedsanalyse

### Fund og konsekvens

Hvad kan brugeren gøre, og hvilke data eller funktioner bliver berørt?

### Sådan viser jeg fejlen

Forudsætninger, request og resultat.

### Årsagen i koden

Hvilken kontrol mangler eller virker forkert?

### Rettelse i Java

Den relevante kode før og efter rettelsen og min forklaring af ændringen.

### Test af rettelsen

Hvad har jeg kørt, hvad forventede jeg, og hvad blev resultatet? Jeg skelner mellem det, jeg har afprøvet, og det, jeg kun har beskrevet.

## Det jeg stadig er i tvivl om

Efter at have læst PortSwiggers adgangskontrolafsnit og løst de 13 labs er jeg blevet mere fortrolig med Burp Suite og med at undersøge broken access control. Jeg er stadig usikker på, hvordan jeg afgør, om mine egne Java-projekter har tilsvarende fejl. Nogle eksempler virker konstruerede, og jeg mangler at se forbindelsen til min egen kode.

### Næste undersøgelse i mit eget projekt

Jeg begynder med de 12 Start-tests i miseOS-matrixen. Min første milepæl er at dokumentere, om en LINE_COOK kan læse sin egen råvareanmodning, bliver afvist fra en anden brugers og bliver afvist uden login. Jeg undersøger derefter kontrollen i `IngredientRequestRoute`, `IngredientRequestService` og autentifikationslaget.

Undersøgelsen er planlagt, men ikke udført. De øvrige tests er kandidater, jeg kan vælge, mens jeg lærer videre. Et gyldigt login eller en rollekontrol er ikke i sig selv kontrol af ejerskab, station eller tilstand.

# Login, sessioner og adgangskontrol

Status: I gang. Arbejdet med path traversal har ført til en undersøgelse af min egen Java-applikation og dens container.

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

## Systematisk test af adgangskontrol i FOG Carport

Jeg bruger en adgangsmatrix til at beskrive, hvem der må gøre hvad, og en testlog til at dokumentere mine forsøg i Burp Suite. Det gør undersøgelsen afgrænset og mulig at gentage. Et Excel-ark er tilstrækkeligt til dette projekt; det afgørende er sammenhængen mellem krav, test, dokumentation og eventuel rettelse.

[Download Excel-skabelonen til adgangskontrol i FOG](adgangskontrol/fog-adgangskontrol.xlsx).

Skabelonen har tre faner: **Afgrænsning**, **Adgangsmatrix** og **Testlog**. Den indeholder forslag til regler og tests, men ingen udførte testresultater. Jeg skal først bekræfte reglerne og tilpasse requests til min applikation.

### Afgrænsning og testdata

Min første undersøgelse handler om brugeroplysninger og ordrer i FOG Carport. Jeg tester adgang uden login, adgang til en anden kundes data og adgang til administratorfunktioner. SQL injection, belastningstest og andre sårbarhedstyper indgår ikke i denne undersøgelse. Path traversal og containerrettigheder er beskrevet særskilt nedenfor og i noterne om sikker opsætning.

Jeg begynder i mit lokale miljø med egne testdata. Jeg noterer miljøets adresse, version eller commit og testdato i fanen Afgrænsning. Hvis jeg bruger `fog.corral.dk` som testmiljø, skal netop det miljø indgå i afgrænsningen. Adressen er ikke undersøgt som led i denne dokumentation.

Jeg opretter to testkunder, A og B, med hver sin profil og ordre samt en administratorkonto, hvis den rolle findes i projektet. Jeg noterer de faktiske ID'er i arket. To kunder er nødvendige for at kontrollere ejerskab; én kunde og en administrator viser ikke alene, om kunder kan tilgå hinandens data.

### Adgangsmatrix: de forventede regler

Matrixen beskriver det, systemet skal tillade, før jeg undersøger, hvad det faktisk tillader. Her er et forslag, som skal bekræftes mod projektets krav:

| Handling | Ikke logget ind | Kunde, egne data | Kunde, andres data | Administrator |
| --- | --- | --- | --- | --- |
| Se kundeprofil | Afvis | Tillad | Afvis | Tillad, hvis rollen har dette ansvar |
| Redigere kundeprofil | Afvis | Tillad for tilladte profilfelter | Afvis | Afklar tilladte felter og ansvar |
| Se en ordre | Afvis | Tillad | Afvis | Tillad, hvis rollen har dette ansvar |
| Se ordreoversigt | Afvis | Kun egne ordrer | Ingen andres ordrer | Afklar, om rollen må se alle ordrer |
| Ændre ordrestatus | Afvis | Afklar kundens tilladte handlinger | Afvis | Afklar tilladte statusskift |
| Ændre brugerrolle | Afvis | Afvis | Afvis | Afklar, hvem der må administrere roller |

Jeg skelner mellem tre typer kontrol:

- **Uden login:** Kan en besøgende hente beskyttede data eller udføre en beskyttet handling?
- **Horisontal adgangskontrol:** Kan kunde A hente eller ændre kunde B's data?
- **Vertikal adgangskontrol:** Kan en kunde udføre en handling, der kræver en anden rolle?

Jeg tester også den tilladte adgang. Hvis en request fejler for alle brugere, viser det ikke alene, at adgangskontrollen virker.

### Testlog: én konkret test pr. række

En adresse som `/users/1` hører til i testloggen sammen med HTTP-metode, bruger, forventet resultat og faktisk resultat. Samme sti kan have forskellige adgangsregler for læsning og ændring. Jeg finder de faktiske requests i Burps HTTP history ved at bruge applikationen normalt; jeg antager ikke, at eksemplernes stier findes i FOG.

Eksempel med kunde A som bruger 1 og ejer af ordre 101 samt kunde B som bruger 2 og ejer af ordre 102:

| Test-ID | Logget ind som | Request | Forventet resultat | Status |
| --- | --- | --- | --- | --- |
| BAC-01 | Kunde A | `GET /users/1` | Egen profil vises | Ikke kørt |
| BAC-02 | Kunde A | `GET /users/2` | Adgang til B's profil afvises | Ikke kørt |
| BAC-03 | Kunde A | `GET /orders/101` | Egen ordre vises | Ikke kørt |
| BAC-04 | Kunde A | `GET /orders/102` | Adgang til B's ordre afvises | Ikke kørt |
| BAC-05 | Ingen | `GET /orders/101` | Adgang afvises uden gyldig session | Ikke kørt |

ID'erne og stierne er eksempler. Excel-arket indeholder også forslag til test af ændringer, ordreoversigter, administratorfunktioner og adgang efter logout. Ændringstests kræver, at jeg først finder den rigtige request og afklarer den forventede adgang.

Jeg udfylder faktisk resultat, statuskode, evidens, testdato, testet version, eventuel fejlreference og gentest. Jeg bruger status **Bestået**, når resultatet matcher det bekræftede krav, **Fejlet**, når det ikke matcher, og **Blokeret**, når forudsætningerne mangler. Et ubekræftet krav kan ikke bruges til at erklære en test bestået.

### Opskrift i Burp Suite: kunde A forsøger at læse B's ordre

1. Opret de to testkunder og deres ordrer. Bekræft, at B kan se sin egen ordre.
2. Log ind som A, og åbn A's egen ordre. Find requesten i **Proxy → HTTP history**, og vælg **Send to Repeater**.
3. Kontrollér, at requesten bruger A's session eller token. Send den uændret, og bekræft, at A kan læse sin egen ordre.
4. Behold A's session, og skift kun ordre-ID'et til B's kendte testordre. ID'et kan ligge i stien, query-parametre eller requestens body.
5. Send requesten, og undersøg, om B's beskyttede ordreoplysninger bliver returneret.
6. Dokumentér forsøget under BAC-04 med faktisk request, response, dato og version. Jeg kan navngive Repeater-fanen med samme test-ID.
7. Ved en fejl undersøger jeg kontrollen i Java-koden, retter den og gentester både den afviste og den tilladte adgang.

Til test uden login fjerner jeg alle oplysninger, der autentificerer requesten. Ved test efter logout genbruger jeg den gamle session efter et faktisk logout. Hvis en request afvises på grund af eksempelvis et ugyldigt CSRF-token, har jeg endnu ikke isoleret kontrollen af rolle eller ejerskab.

Jeg vurderer både indhold og effekt. En `200` kan være en login-side, og en fejlstatus er ikke i sig selv bevis for, at ingen data er ændret. Ved ændringstests kontrollerer jeg derfor også ressourcens tilstand bagefter. En afvist handling må hverken udlevere beskyttede data eller gennemføre ændringen.

### Placering af dokumentation og begrænsninger

Regnearket ligger sammen med disse noter i `adgangskontrol/fog-adgangskontrol.xlsx`. Jeg kan gemme anonymiseret evidens i eksempelvis `adgangskontrol/evidence/BAC-04/` og skrive stien i testloggen. Jeg fjerner sessionscookies, tokens og personoplysninger fra materiale, jeg deler eller afleverer.

For hvert fund forbinder jeg test-ID'et med sårbarhedsanalysen nedenfor: forventet adgang, faktisk adgang, konsekvens, årsag i Java-koden, rettelse og gentest. Senere kan relevante kontroller blive automatiserede integrationstests.

Når undersøgelsen er færdig, beskriver jeg de konkrete funktioner, roller og handlinger, jeg har testet. Beståede tests i den afgrænsede matrix er ikke en konklusion om, at hele applikationen er fri for adgangskontrolfejl. Der er endnu ikke kørt tests fra denne matrix.

Metodens kilder og vejledninger ligger under [materiale til systematisk adgangskontroltest](../ressourcer/adgangskontrol.md#systematisk-adgangskontroltest-i-eget-projekt).

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

Jeg vil vælge én funktion, eksempelvis visning eller ændring af en ordre, og undersøge den i et lokalt miljø med egne testdata:

- Kan bruger A hente eller ændre bruger B's ordre ved at udskifte et ID i requesten?
- Kan en almindelig bruger kalde en adminfunktion direkte, selv om knappen ikke vises?
- Kan en profilopdatering ændre en rolle eller andre felter, brugeren ikke bør kunne ændre?
- Hvor kontrollerer Java-koden den aktuelle bruger, rollen og ejerskabet af ressourcen?

Det er forslag til næste afprøvning. Jeg har endnu ikke dokumenteret resultater fra disse kontroller i mine egne projekter. Et gyldigt login eller en rollekontrol er ikke i sig selv en kontrol af, hvem der ejer den konkrete ordre.

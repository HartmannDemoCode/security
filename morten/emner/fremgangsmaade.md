# Min fremgangsmåde til sikkerhedsundersøgelser

En generel huskeliste og samling af forsøg bygget på mine øvelsesnoter om Juice Shop, PortSwigger, JWT, SPA-ruter, feedback, reviews og JSON-input.

Jeg bruger den i egne applikationer og autoriserede testmiljøer. Endpoints, feltnavne og ID'er nedenfor er illustrative: Jeg erstatter dem med det, jeg faktisk finder i den valgte app. Jeg bruger kendte testobjekter og disponible data ved ændring og sletning.

Java-udsnittene er forenklede skitser til kodegennemgang, ikke komplette applikationer. Hjælpefunktioner som `requireAuthenticatedUser`, `forbidden` og `notFound` skal tilpasses det anvendte framework. Identiteten skal komme fra en verificeret session eller et verificeret token.

**Forstå funktionen → find requesten → fastlæg forventningen → ændr én ting → kontrollér effekten → forklar → ret og gentest.**

## Vælg et sted at begynde

Jeg vælger en funktion som profil, kurv, ordre, feedback eller anmeldelse og følger dens requests. Jeg kan arbejde gennem disse spor og udvide efter, hvad jeg observerer:

| Prioritet | Det jeg gør | Det jeg skal kunne vise |
| --- | --- | --- |
| Start | Test tilladt adgang, adgang uden login, en anden ejer og en forkert rolle. Undersøg almindeligt login og tokenvalidering. | En forventet adgangsregel, en fungerende normal request og dokumentation for hvert forsøg. |
| Byg videre | Undersøg relevante grænsetilfælde, forklar årsagen i koden, vurder risiko og afprøv en begrundet forbedring. | Kode før/efter, gentest og test af tilladt brug. |
| Flere spor | Manipulér forfatter og ekstra felter, prøv dublerede JSON-nøgler, undersøg JWT, og brug Hashcat/Hydra, hvor de er relevante. Følg også input til SQL, HTML eller filadgang. | Hvilken antagelse forsøget afprøver, og hvad der faktisk sker. |

Jeg ændrer én ting ad gangen, så jeg kan forklare forskellen mellem det normale resultat og mit forsøg.

## 1. Forstå funktionen og adgangsreglen

- Hvad gør funktionen, og hvilke oplysninger eller handlinger skal beskyttes?
- Hvem må læse, oprette, ændre, godkende eller slette?
- Afhænger adgangen af rolle, ejerskab, organisation eller objektets tilstand?
- Hvilket miljø og hvilken version undersøger jeg?

Jeg skriver forventningen, før jeg tester. Kode og dokumentation hjælper med at finde reglerne, men uenighed mellem dem skal afklares. En administrator kan eksempelvis godt have legitim adgang til andre brugeres ordrer.

### Lav en enkel trusselsmodel

Jeg beskriver fire ting for den valgte funktion:

| Spørgsmål | Generelt eksempel |
| --- | --- |
| Hvad beskytter jeg? | Private ordreoplysninger og retten til at ændre eller slette en ordre. |
| Hvem kan misbruge funktionen? | En bruger uden login eller en almindelig bruger med et gyldigt login. |
| Hvor skifter tilliden? | Browser → API: klienten kan ændre input. API → database: ændringer skal følge de bekræftede adgangsregler. |
| Hvad vil jeg efterprøve? | Om A kan læse B's private ordre eller slette data, som kun administratorer må slette. |

Eksemplerne er hypoteser. Jeg beder gerne en medstuderende udfordre en antagelse og noterer, hvad det ændrer i testen. Det er en enkel anvendelse af [OWASPs trusselsmodellering](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html).

Jeg giver forsøget et test-ID. Jeg kan supplere med relevante krav fra [ASVS](https://asvs.owasp.org/) eller testvejledning fra [WSTG v4.2](https://wstg.owasp.org/v4.2/4-Web_Application_Security_Testing/).

## 2. Find den request, som udfører handlingen

Jeg udfører en normal handling og følger trafikken i **DevTools → Network** eller **Burp → Proxy → HTTP history**. Jeg sender den relevante request til **Repeater** og gemmer en uændret udgave som sammenligningsgrundlag.

Jeg bruger separate browserprofiler til A og B eller kontrollerer brugerens identitet ved hvert tokenskift. Jeg indstiller Burps scope til mit valgte testmiljø, og jeg noterer base-URL, commit og relevant konfiguration. Testdata skal kunne genskabes; rigtige test-ID'er og gyldige datoer indsættes før forsøgene.

| Hvor kigger jeg? | Hvad noterer jeg? |
| --- | --- |
| HTTP-metode og sti | Hvilken handling og hvilket objekt rammer requesten? |
| Query og JSON-body | ID'er og felter som `userId`, `ownerId`, `author`, `role` og `BasketId`. |
| Headers og cookies | Hvordan følger brugerens identitet med? Sendes der flere tokens? |
| Svaret | Hvilke beskyttede data returneres, og hvem tilhører de? |
| Frontend-kode | API-kald og spor som `admin`, `guard`, `debug` og `sandbox`. |

En frontend-rute er et spor til funktionalitet. Jeg undersøger serverens API-kald for at vurdere adgangen. URL-fragmentet efter `#` sendes ikke som en del af HTTP-requestens sti. Se [MDN om URI-fragmenter](https://developer.mozilla.org/en-US/docs/Web/URI/Reference/Fragment).

## 3. Vælg, hvad jeg vil undersøge

**Adgangskontrol:** Jeg bruger et gyldigt token og undersøger, hvad den identificerede bruger må gøre. **Autentifikation:** Jeg undersøger login og serverens kontrol af tokenet. Jeg laver dem som separate forsøg, så et ugyldigt token ikke skjuler en fejl i ejerskabskontrollen. Se [PortSwiggers forklaring af identitet og adgang](https://portswigger.net/web-security/access-control#what-is-access-control).

| Testspor | Hvad leder jeg efter? | Kontrolleret forsøg |
| --- | --- | --- |
| Ejerskab — IDOR/BOLA | Kan A tilgå B's beskyttede objekt? | Behold A's gyldige token, og skift kun til et kendt objekt-ID fra testbruger B. |
| Roller | Kan en almindelig bruger udføre en ledelseshandling? | Gentag en kendt gyldig request med en bruger, der ifølge kravet skal afvises. |
| Identitet i input | Stoler serveren på en indsendt forfatter eller ejer? | Ændr kun `UserId`, `author` eller et tilsvarende felt til en anden testbrugers identitet. Kontrollér, hvem resultatet registreres på. |
| Handling og tilstand | Er læsning og ændring beskyttet hver for sig? | Test relevante faktiske GET-, POST-, PUT-, PATCH- og DELETE-ruter samt tilladte og beskyttede tilstande med separate testobjekter. |
| Ekstra JSON-felter | Kan jeg tildele mig selv en rolle eller ejer? | Tilføj ét felt ad gangen, og kontrollér den lagrede effekt og eventuelt adgang efter nyt login. |
| Dublerede JSON-nøgler | Anvendes forskellige værdier ved kontrol og lagring? | Send en rå JSON-body med to ens feltnavne og forskellige test-ID'er. Gentag med omvendt rækkefølge. |
| JWT | Accepteres et token, der burde afvises? | Sammenlign et gyldigt token med et udløbet token og en kopi med ugyldig signatur, ét forsøg ad gangen. |
| Udviklerværktøjer | Er interne funktioner utilsigtet tilgængelige? | Undersøg adgang og eventuelt ufarligt testindhold uden login og med relevante roller. |

Sporene kommer fra mine øvelsesnoter. Jeg vælger dem efter den konkrete funktion og følger eksemplerne nedenfor.

### IDOR/BOLA — prøv et andet objekt med dit eget token

**Se efter:** ID'er i sti, query eller body: `/orders/101`, `?userId=12`, `basketId`, `documentId`. Opret to brugere A og B med hver deres objekt. Bekræft først, at begge kan læse deres eget.

Normal request som A, der ejer ordre 101:

```http
GET /api/orders/101 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <A_GYLDIG_TOKEN>
```

Skift kun til B's kendte ordre 202:

```http
GET /api/orders/202 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <A_GYLDIG_TOKEN>
```

**Kontrollér:** Indeholder svaret faktisk B's private ordre? Gentag med relevante ændringsruter og kontrollér data bagefter. Kendte UUID'er kan testes på samme måde; deres kompleksitet erstatter ikke en adgangsregel.

**Se efter i Java:** Bliver objektet hentet uden kontrol af den aktuelle bruger?

```java
// Potentiel fejl: Hvis der ikke er en anden adgangskontrol,
// kan enhver bruger, der når hertil, få den valgte ordre.
Order order = orderDao.findById(orderId);
return toResponse(order);
```

En forenklet rettelse, når reglen er, at kun ejeren må læse:

```java
User currentUser = requireAuthenticatedUser(ctx);
Order order = orderDao.findById(orderId);
if (order == null) {
    throw notFound();
}
if (!order.ownerId().equals(currentUser.id())) {
    throw forbidden();
}
return toResponse(order);
```

Jeg tester både ejerens og den anden brugers adgang efter ændringen. Eventuelle administratorrettigheder skal indgå i den tilsigtede regel. Se [OWASP om IDOR](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html).

### Roller — prøv en administratorhandling som almindelig bruger

**Se efter:** Sletteknapper, brugeradministration og admin-requests i JavaScript eller trafik fra en autoriseret testadministrator.

```http
DELETE /api/feedback/17 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <ALMINDELIG_BRUGERS_GYLDIGE_TOKEN>
```

Jeg bruger feedback, som kun administratoren må slette efter den fastlagte regel. Først bekræfter jeg, at handlingen virker som administrator med et separat testobjekt. Efter forsøget som almindelig bruger kontrollerer jeg, om data stadig findes.

Denne frontend-kode viser kun, hvem der får en knap:

```jsx
{user.role === 'ADMIN' && <button onClick={deleteFeedback}>Slet</button>}
```

Serveren skal også kontrollere handlingen. Forenklet Java-eksempel:

```java
User currentUser = requireAuthenticatedUser(ctx);
if (!currentUser.role().equals("ADMIN")) {
    throw forbidden();
}
feedbackDao.delete(feedbackId);
```

Jeg undersøger den faktiske rute, middleware og service. At en guard mangler ét sted, beviser ikke alene, at der ikke findes en fungerende kontrol et andet sted. Se [PortSwigger om rollebaseret adgang](https://portswigger.net/web-security/access-control#vertical-access-controls).

### Falsk forfatter — ændr `UserId` eller `author`

**Se efter:** Identitet i en oprettelsesrequest, fx feedback eller anmeldelser:

```json
{"UserId":12,"comment":"LAB_FEEDBACK_01","rating":3}
```

Behold A's gyldige token, og skift kun `UserId` til B's kendte ID. I en anden funktion kan det tilsvarende være:

```json
{"message":"LAB_REVIEW_01","author":"b@example.test"}
```

**Kontrollér:** Hvem registreres som forfatter efter oprettelsen? At feltet accepteres i requesten er ikke nok; det kan være ignoreret. Læs resultatet igen eller kontrollér den lokale testdatabase.

Hvis reglen er, at brugeren skriver som sig selv, kan serveren vælge identiteten:

```java
public record CreateReviewRequest(String message) {}

// Inde i den relevante handler/service:
User currentUser = requireAuthenticatedUser(ctx);
CreateReviewRequest input = parseAndValidateRequest(ctx);
Review review = new Review(currentUser.id(), input.message());
reviewDao.save(review);
```

Om ekstra felter afvises eller ignoreres afhænger af parserens konfiguration. Det afgørende her er, at inputtet ikke kan overtage kontrollen med forfatteridentiteten.

### Ekstra felter — prøv mass assignment

**Se efter:** Profilopdateringer og registrering, hvor JSON bindes direkte til en brugerentity.

Normal body:

```json
{"displayName":"Testbruger A"}
```

Prøv derefter ét ekstra felt ad gangen:

```json
{"displayName":"Testbruger A","role":"ADMIN"}
```

Andre relevante felter kan være `isAdmin`, `ownerId` eller `accountId`, hvis de faktisk bruges af applikationen. Jeg kontrollerer gemte data og eventuelt et nyt login og en beskyttet handling. Et gammelt token kan indeholde den tidligere rolle.

I kodegennemgangen ser jeg efter automatisk kopiering af alle klientfelter til en entity. En afgrænset inputtype kan eksempelvis være:

```java
public record UpdateProfileRequest(String displayName) {}
```

Serveren opdaterer kun de tilladte felter og kontrollerer adgangen til profilen. En DTO alene fastlægger hverken parserens opførsel eller autorisationen. Se [OWASP om mass assignment](https://cheatsheetseries.owasp.org/cheatsheets/Mass_Assignment_Cheat_Sheet.html).

### Grundlæggende login og fejlsvar

Jeg begynder autentifikationsdelen med disse få tilfælde og fastlægger forventningen først:

| Forsøg | Det jeg undersøger |
| --- | --- |
| Kendt testbruger og korrekt password | Login virker, og et udstedt token giver den tilsigtede adgang. |
| Kendt testbruger og forkert password | Ingen brugbar autentifikation udstedes. |
| Ikke-eksisterende testbruger | Fejltekst og status afslører ikke unødigt, hvilke konti der findes. |
| Gyldigt, udløbet og ugyldigt signeret token | Beskyttet adgang følger den fastlagte tokenpolitik; testene udføres hver for sig. |

Ens fejltekst alene udelukker ikke andre forskelle. Jeg beskriver de signaler, jeg faktisk har undersøgt. Se [OWASP om autentifikationsfejl](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#authentication-and-error-messages).

Hvis ændring af password, rolle eller kontostatus indgår i mit udvalg, afklarer jeg også, hvad der skal ske med allerede udstedte tokens. Det er et særskilt krav og forsøg.

### Særligt om JWT og tokenopbevaring

Jeg begynder med et gyldigt token og en request, der giver beskyttede data. Jeg laver en separat kopi til hvert forsøg:

| Ændring | Det jeg afprøver |
| --- | --- |
| Fjern alle tokens og sessionscookies | Om den beskyttede funktion kan bruges anonymt. |
| Ændr payloadens `role` eller `sub`, men behold signaturen | Om signaturkontrollen faktisk forhindrer manipulation. |
| Fjern signaturen og angiv `alg: none` i en lokal JWT-øvelse | Om serveren accepterer et usigneret token, hvor et signeret token kræves. |
| Brug et reelt udløbet token | Om udløbstiden håndhæves. Jeg noterer faktisk `exp`, servertid og eventuel tilladt tidsafvigelse. |
| Log ud, og gentag en request med den gemte tokenkopi | Om adfærden følger den aftalte logout-/tilbagekaldelsespolitik. |

Hvis jeg ændrer `exp` i et token uden at gensignere, tester jeg ugyldig signatur, ikke udløb alene. Hvis både cookie og Bearer-header sendes, afklarer jeg, hvilken der bestemmer identiteten.

- Jeg undersøger signatur, tilladte algoritmer og relevante claims i serverens validering. `alg: none`, algoritmeforveksling og claims som `iss`, `aud` og `nbf` er mulige udvidelser, når de passer til systemet. At kunne læse en signeret JWT giver ikke mulighed for at fremstille en gyldig signatur. Se [PortSwigger om JWT](https://portswigger.net/web-security/jwt).
- Jeg noterer tokenets placering, levetid og eventuel eksponering i URL'er eller logs. JavaScript kan læse localStorage; det giver en risiko ved XSS, men tokenets placering alene dokumenterer ikke konto-overtagelse. Se [OWASP om lokal opbevaring](https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html#local-storage).
- Ved cookies undersøger jeg relevante sikkerhedsflag og CSRF-beskyttelse. Logout og tilbagekaldelse vurderes ud fra systemets fastlagte politik. Jeg skelner mellem sletning i browseren og serverens accept af et tidligere udstedt token.

I DevTools → Application kan jeg se lagringsstedet. I min egen apps konsol kan jeg undersøge tilstedeværelse uden at udskrive tokens:

```javascript
['token', 'access_token', 'refresh_token'].map(name => ({
  name,
  inLocalStorage: localStorage.getItem(name) !== null,
  inSessionStorage: sessionStorage.getItem(name) !== null
}));
```

Feltnavnene varierer mellem apps. HttpOnly-cookies undersøges i browserens cookieoversigt og headers. At kunne læse browserens eget token er en observation om opbevaring; en faktisk XSS-vej kræver et særskilt forsøg.

### Hashcat og Hydra

Jeg vælger værktøj efter spørgsmålet: **Hashcat** afprøver kandidater til et HS256-signeringssecret offline ud fra et token. **Hydra** sender loginforsøg for at undersøge passwords og serverens begrænsning af forsøg. API-rate limiting påvirker onlineforsøgene; det beskytter ikke et svagt JWT-secret mod offline-gætning. Et stærkt tilfældigt secret er i praksis urealistisk at gætte. Se [PortSwigger om JWT-secret-gætning](https://portswigger.net/web-security/jwt#brute-forcing-secret-keys).

[Opskrifter med Hashcat og Hydra](adgangskontrol/hashcat-og-hydra.md) indeholder lokale eksempler, forudsætninger, vurdering af resultater og rettelse/gentest. De er valgfrie testspor, jeg bruger, når de passer til undersøgelsen.

### Særligt om JSON

Dublerede nøgler kan blive behandlet forskelligt af parsere. Jeg ændrer den rå request-body, så et hjælpeværktøj ikke fjerner dubletterne først. Se [RFC 8259, afsnit 4](https://www.rfc-editor.org/rfc/rfc8259#section-4).

I kodegennemgangen følger jeg input gennem parsing, validering, adgangskontrol og lagring. DTO'er og types er ikke i sig selv ejerskabskontrol. Jeg undersøger også, om kontrol og databaseoperation bruger samme objekt-ID. Afvisning af dubletter erstatter ikke adgangskontrollen.

Eksempel i en lokal kurveøvelse, hvor A ejer kurv 6 og B ejer kurv 3:

```json
{"ProductId":9,"BasketId":6,"BasketId":3,"quantity":1}
```

Jeg sender bodyen som rå tekst fra Repeater, prøver også omvendt rækkefølge og kontrollerer begge kurve bagefter. Jeg sammenligner med enkeltfeltsversionerne, én med kurv 6 og én med kurv 3. Et fund kræver, at data faktisk ændres i strid med adgangsreglen. En afvist request er ikke et hul, og accept af dubletter alene beviser ikke en omgåelse.

### Udviklerværktøjer og sandboxes

Jeg søger i JavaScript efter `debug`, `sandbox`, `developer`, `internal`, `admin` og `execute`, og følger eventuelle API-kald. Eksempel på en fundet rute:

```http
GET /api/internal/debug/status HTTP/1.1
Host: localhost:3000
```

Jeg prøver uden login, som almindelig bruger og som autoriseret administrator. Jeg noterer, om der vises interne oplysninger eller gives adgang til en funktion. En tom frontend-side eller navnet `sandbox` beviser ikke en svaghed. Ved en kodekørselsfunktion bruger jeg kun en dokumenteret ufarlig handling i det lokale øvelsesmiljø og vurderer funktionens adgang og rettigheder.

### Flere inputspor — SQL, HTML og filadgang

Disse forsøg er relevante, når appen har den tilsvarende funktion. Jeg starter med et let genkendeligt input og følger det gennem request, backend og resultat.

**SQL:** I et søgefelt sammenligner jeg normal søgning med et input som `LAB_SQL_'`. En databasefejl er et spor, som kræver undersøgelse af årsagen. I en lokal bevidst sårbar søgeøvelse kan jeg sammenligne et sandt og et falsk udtryk, fx `x' OR '1'='1'-- ` og `x' OR '1'='2'-- `. Syntaksen afhænger af query og database. Ændrede søgeresultater skal kunne forklares og gentages; én fejlbesked er ikke tilstrækkelig dokumentation.

Jeg ser efter SQL, der sammensættes af brugerinput:

```java
// Usikker sammensætning, hvis input indsættes direkte:
String sql = "SELECT id, name FROM products WHERE name = '" + input + "'";
```

Parameterbinding i JDBC holder data adskilt fra queryens struktur:

```java
try (PreparedStatement statement = connection.prepareStatement(
        "SELECT id, name FROM products WHERE name = ?")) {
    statement.setString(1, input);
    try (ResultSet results = statement.executeQuery()) {
        // Behandl resultatet.
    }
}
```

Se [OWASP om SQL injection og parameterbinding](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html).

**HTML/XSS:** Jeg indtaster først `LAB_HTML_123` og derefter `<b>LAB_HTML_123</b>` i en kommentar eller anmeldelse. Jeg undersøger, hvor inputtet ender, og om det gengives som tekst eller HTML. HTML-gengivelse alene beviser ikke scriptkørsel. I min lokale test kan en ufarlig markør til en HTML-kontekst være:

```html
<img src="/missing-lab-image" onerror="console.log('LAB_XSS_123')">
```

Hvis markøren kører, dokumenterer jeg inputvejen, browserkonteksten og de nødvendige handlinger. Jeg skriver også, om input kun blev ændret i min egen konsol, eller om det faktisk kan komme gennem applikationens input. Jeg rydder lagrede testdata op bagefter.

Ved almindelig tekstvisning kan kodeeksemplet være:

```javascript
// Kan fortolke upålideligt input som HTML:
element.innerHTML = userInput;

// Til en funktion, som kun skal vise tekst:
element.textContent = userInput;
```

Hvis funktionen skal tillade HTML, kræves en passende sanitiseringsløsning til den kontekst. Se [OWASP om XSS-forebyggelse](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html).

**Filadgang/path traversal:** I en lokal downloadøvelse placerer jeg en harmløs fil `private/lab-canary.txt` uden for den tilladte downloadmappe. Jeg tester en normal fil og derefter traversal mod netop testfilen:

```http
GET /download?file=../private/lab-canary.txt HTTP/1.1
Host: localhost:3000
```

Hvis filens kendte indhold udleveres i strid med reglen, dokumenterer jeg det. Jeg undersøger i koden, hvordan stien opløses, hvordan adgang begrænses til den tilladte mappe, og om symlinks påvirker kontrollen. Et fil-ID med serverstyret mapping kan være et alternativ til klientvalgte stier. Se [PortSwigger om path traversal](https://portswigger.net/web-security/file-path-traversal).

## 4. Ændr én ting, og sammenlign

1. Bekræft, at den tilladte handling virker med gyldigt input og den rigtige bruger.
2. Notér objektets ejer og tilstand. Brug kendte egne testdata.
3. Skift én relevant betingelse: objekt-ID, bruger, rolle, token eller et inputfelt.
4. Send requesten, og gem svaret sammen med den præcise ændring.
5. Ved ændring eller sletning kontrollerer jeg data bagefter som ejer, ledelse eller i testdatabasen.

Jeg holder token, body og tilstand gyldige i en adgangskontroltest. En valideringsfejl eller forkert route kan ellers forhindre handlingen, før adgangskontrollen undersøges. Ændrende tests bruger disponible objekter, så ét forsøg ikke ødelægger det næste.

Jeg vælger relevante grænsetilfælde, eksempelvis et manglende felt, forkert datatype eller et objekt i en beskyttet tilstand. Her undersøger jeg, om fejlen håndteres uden beskyttede data eller utilsigtede ændringer. Sådanne inputtests registreres særskilt fra ejerskabstesten.

## 5. Afgør, hvad resultatet viser

Jeg sammenholder **forventning, svarindhold og faktisk effekt**. En statuskode, en åben admin-side eller en fundet sandbox er ikke alene bevis for uberettiget adgang.

Jeg registrerer resultatet som fungerende kontrol, påvist svaghed eller uafklaret/blokeret forsøg. Jeg forklarer, hvad evidensen viser, og hvad der mangler. Jeg konkluderer kun om de undersøgte tilfælde.

### Vurder risikoen i den undersøgte funktion

For en påvist svaghed skriver jeg kort:

- Hvilken adgang kræver udnyttelsen, og hvor let er den at gentage?
- Hvilke oplysninger eller handlinger berøres, og hvor mange brugere eller objekter kan være påvirket?
- Hvad er konsekvensen for fortrolighed, korrekte data eller tilgængelighed?
- Hvilken rettelse prioriterer jeg, og hvorfor?

Jeg kan bruge lav, middel eller høj som en begrundet vurdering i applikationens kontekst. Et værktøjs mærkat eller OWASP-kategoriens nummer er ikke i sig selv en risikovurdering af mit fund.

Jeg ser også efter relevante loghændelser: Kan forsøget knyttes til tidspunkt, request og en passende testbrugerreference? Logs supplerer request, response og datakontrol. De må ikke indeholde passwords eller fulde adgangstokens. Se [OWASP om autorisationslogging](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html#implement-appropriate-logging).

## 6. Find årsagen, ret og gentest

Ved en påvist svaghed følger jeg i egen kode requesten gennem tokenkontrol, rute/rollekontrol, service, domæneregler og databaseoperation. Hvor skulle handlingen være afvist, og hvorfor skete det ikke?

For en valgt rettelse gemmer jeg relevant kode før og efter og forklarer ændringen. Jeg gentager det oprindelige angrebsforsøg og tester også den tilladte handling. Jeg bruger samme testforudsætninger eller genskaber tilsvarende testdata, så forskellen kan forklares med rettelsen. Rettigheder skal kontrolleres på serveren for den konkrete handling og ressource; se [OWASP om autorisation](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html).

Hvis rettelsen kun er foreslået, markerer jeg den som **ikke implementeret og ikke gentestet**.

### Gør den vigtige test automatisk

Når jeg har et reproducerbart problem og retter sikkerhedskoden, omsætter jeg så vidt muligt forsøget til en API-/integrationstest. Ved en ejerskabsfejl opretter testen to brugere og kendte objekter, logger ind som A og forsøger at tilgå B's objekt. Jeg kontrollerer svarets indhold og ved ændringer også den gemte tilstand.

Testen skal kunne afsløre den oprindelige fejl og bestå med rettelsen. En ledsagende test sikrer, at ejeren stadig kan udføre den tilladte handling. Jeg bruger projektets eksisterende testværktøjer og beholder Burp-evidensen som forklaring af det manuelle forsøg. Se [OWASP om tests af autorisationslogik](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html#create-unit-and-integration-test-cases-for-authorization-logic).

## 7. Skriv en kort testnote

```text
Test-ID, dato og miljø/version:
Funktion og forventet adgangsregel:
Krav-/metodereference og version, hvis anvendt:
Testbrugere, roller, objektejere og tilstand:
Hypotese — hvad forsøger jeg at undersøge?
Tilladt request og resultat:
Den ene ændring i forsøget:
Faktisk svar og effekt på data:
Evidens — maskeret request/response og eventuel tilstand før/efter:
Vurdering, konsekvens og begrundet prioritet:
Årsag i koden, hvis undersøgt:
Rettelse — foreslået eller implementeret, med kodereference:
Gentest — version, angrebsforsøg og tilladt brug:
Begrænsninger / næste skridt:
Ved øvelser: brugte hints eller løsning:
```

Jeg fjerner tokens, passwords og personoplysninger fra delt evidens. Bruger jeg en testmatrix, kobler jeg noten til dens test-ID.

### Hvornår er et scenarie dokumenteret?

- [ ] Jeg kan forklare den tilsigtede regel og begrunde testvalget.
- [ ] Jeg har dokumenteret miljø, bruger, input og faktisk resultat, så en anden kan gentage forsøget.
- [ ] Jeg kan skelne mellem min observation, en påvist svaghed og en antagelse.
- [ ] Ved en rettelse viser jeg årsag, kodeændring, gentest og tilladt brug.
- [ ] Jeg har begrundet risikoen og beskrevet undersøgelsens begrænsning.

## Det jeg tager med fra øvelserne

| Øvelsesnote | Det generelle spørgsmål, jeg tager med |
| --- | --- |
| Skjult admin-side i en SPA | Beskytter serveren de tilhørende API-kald? |
| Kurv/ordre og IDOR | Kontrolleres ejerskab, selv om et andet ID er kendt? |
| Five-Star Feedback | Hvem må slette, og kontrolleres det ved selve handlingen? |
| Forged Feedback / Forged Review | Hvem bestemmer den registrerede identitet: klientinput eller verificeret bruger? |
| JSON Parameter Pollution | Er det samme ID, der bliver valideret, autoriseret og anvendt? |
| JWT og tokenopbevaring | Hvilken identitet stoler serveren på, og hvordan beskyttes den? |
| Sandboxes og debugværktøjer | Hvem har adgang, og hvilke data og rettigheder kan funktionen bruge? |
| Path traversal | Findes der en relevant filadgang, og hvilke rettigheder begrænser konsekvensen? |

Jeg udvider listen, når jeg lærer et nyt mønster. Et gennemført øvelseseksempel giver en testidé til en anden app; resultatet skal undersøges særskilt i hvert miljø.

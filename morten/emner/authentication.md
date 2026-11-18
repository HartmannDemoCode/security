# A07:2025 – Authentication Failures

Status: OWASP-siden er læst, og Pluralsight-kurset OWASP: Authentication Failures er set. PortSwiggers authentication-afsnit og 2FA simple bypass mangler endnu. Intet er afprøvet i egen kode.

Det her er mine noter til A07. A01 og den samlede plan ligger i [login, sessioner og adgangskontrol](adgangskontrol.md) og [forløbet](../forloeb/01-adgangskontrol.md).

## Hvad dækker kategorien?

A07 handler om, hvorvidt systemet kan stole på, hvem brugeren er. Fejlen sker, når systemet kan narres til at behandle en forkert bruger som den rigtige. A01 handler om, hvad en korrekt logget ind bruger må. En bruger kan altså være korrekt logget ind (A07 i orden) og stadig hente en anden brugers data (A01-fejl).

Kategorien er stadig nummer 7, som i 2021. Den hed før *Identification and Authentication Failures* og har fået nyt navn, så det passer bedre med de 36 CWE'er, den dækker.

| Tal fra OWASP | |
|---|---|
| CWE'er | 36 |
| Gennemsnitlig forekomst | 2,92 % |
| Maks. forekomst | 15,80 % |
| Forekomster i alt | 1.120.673 |
| CVE'er i alt | 7.147 |

## Typiske fejl

Jeg har delt OWASPs liste op i fire grupper:

**Gæt og automatiserede angreb**
- Credential stuffing: lækkede brugernavne og adgangskoder fra andre sider prøves af.
- Password spraying: få almindelige adgangskoder prøves mod mange konti.
- Brute force, der ikke bliver stoppet eller forsinket.

**Svage adgangskoder og opbevaring**
- Standardadgangskoder og standardadministratorkonti.
- Systemet accepterer adgangskoder, der er kendt fra læk.
- Adgangskoder gemt i klartekst, krypteret i stedet for hashet, eller med en svag hash.
- Hardkodede adgangskoder i koden (CWE-259, CWE-798).

**Loginforløb**
- Manglende eller dårlig MFA, eller en svag reserveløsning når MFA ikke er tilgængelig.
- Svag gendannelse af konto, fx sikkerhedsspørgsmål.
- Forskellige fejlbeskeder, så man kan finde ud af, om en konto findes.

**Sessioner og tokens**
- Session-ID i URL'en, i skjulte felter eller andre steder klienten kan se.
- Samme session-ID før og efter login (session fixation, CWE-384).
- Sessioner og tokens, der ikke bliver ugyldige ved logout eller inaktivitet, især ved SSO.
- Tokens hvor `aud`, `iss` og scopes ikke bliver kontrolleret.

## Forebyggelse

- MFA hvor det er muligt.
- Ingen standardadgangskoder, især ikke til administratorer.
- Kontrollere nye adgangskoder mod lister over svage og lækkede adgangskoder.
- Følge NIST SP 800-63B om længde og kompleksitet. Ikke tvinge brugere til at skifte adgangskode med faste mellemrum; kun når der er mistanke om et læk.
- Samme fejlbesked ved alle udfald af login og gendannelse.
- Begrænse eller forsinke mislykkede loginforsøg, uden at en angriber kan bruge det til at låse alle andre ude. Logge og give besked ved mistænkelig aktivitet.
- Sessionshåndtering på serveren: nyt tilfældigt session-ID efter login, ikke i URL'en, sikre cookies og udløb ved logout, inaktivitet og efter en fast maksimal tid.
- Bruge et gennemprøvet login- og identitetssystem frem for at bygge sit eget.
- Validere JWT-claims som `aud`, `iss` og scopes.

## OWASPs eksempler på angreb

1. **Credential stuffing og password spraying.** Uden forsvar mod automatiserede angreb kan login-endpointet bruges til at teste, om en adgangskode er rigtig.
2. **Kun adgangskode.** Systemer, der kun bruger adgangskode, er sårbare. MFA anbefales til vigtige systemer.
3. **Sessionen afsluttes ikke.** En bruger lukker browseren i stedet for at logge ud, eller en SSO-session bliver ved med at være aktiv efter logout. Den næste ved computeren kommer ind på kontoen.

## Pluralsight – OWASP: Authentication Failures

Jeg har set [kurset](https://www.pluralsight.com/courses/owasp-authentication-failures) (ca. 1 time). Her er det, jeg tager med, og hvor jeg har uddybet eller rettet det mod OWASP og NIST.

### 2FA/MFA

Kurset siger, at 2FA er et must for alle websites. OWASP er lidt mere forsigtig: MFA "wherever possible". Pointer:

- MFA er ikke lige stærkt. SMS-koder kan opsnappes eller overtages med SIM-swap. En authenticator-app er bedre, og passkeys/WebAuthn er bedst, fordi de ikke kan phishes.
- Det skal være implementeret rigtigt. Hvis serveren ikke kontrollerer, at trin 2 er gennemført, kan man springe det over. Det er præcis det, PortSwiggers *2FA simple bypass* handler om.
- Reserveløsningen er en del af sikkerheden. Hvis gendannelse af konto eller "glemt 2FA" er svag, bliver hele MFA ikke stærkere end den.

### Genbrug af adgangskoder

Brugere genbruger den samme adgangskode på mange sider. Det kaldes **credential stuffing**: lækkede brugernavne og adgangskoder fra én side prøves af på en anden. Det er noget andet end brute force, hvor man gætter mange adgangskoder. Ved credential stuffing er gættet ofte rigtigt i første forsøg, så en grænse for forsøg pr. konto hjælper mindre end mod brute force.

### Tjek mod lækkede adgangskoder

Kurset har et test-repo med en funktion, der måler adgangskodens styrke og kalder [Have I Been Pwned – Pwned Passwords](https://haveibeenpwned.com/API/v3#PwnedPasswords). Så kan applikationen afvise adgangskoder, der er kendt fra læk. Det anbefaler både OWASP og NIST.

- API'et bruger *k-anonymity*: man sender kun de første 5 tegn af SHA-1-hashen af adgangskoden og får en liste med de hashes, der starter sådan. Selve adgangskoden og den fulde hash forlader aldrig serveren.
- Rettelse til mine noter: listen består af **lækkede adgangskoder**, ikke et rainbow table. Et rainbow table er en forudberegnet tabel, der bruges til at knække hashes uden salt. Mod rainbow tables beskytter man ved at gemme adgangskoder med salt og en langsom hashfunktion (Argon2id, bcrypt eller PBKDF2). Det er to forskellige forsvar:
  - Pwned Passwords forhindrer brugeren i at **vælge** en kendt adgangskode.
  - Salt og langsom hash beskytter adgangskoderne, hvis **databasen bliver lækket**.
- Kursets demoapp er en lille ReactJS-app i én fil. Den bruger [zxcvbn](https://github.com/dropbox/zxcvbn) til at vurdere styrken, tjekker Pwned Passwords og viser, hvor lang tid det vil tage at knække adgangskoden. Den kan hentes under exercise files på [kursets side](https://www.pluralsight.com/courses/owasp-authentication-failures) og ligger ikke offentligt på GitHub. [zxcvbn-ts](https://github.com/zxcvbn-ts/zxcvbn) er en nyere TypeScript-udgave med indbygget Pwned-tjek.
- Min idé til miseOS er at lave samme tjek på serveren i Java. Se [Kodeidé: tjek mod Pwned Passwords ved oprettelse](#kodeidé-tjek-mod-pwned-passwords-ved-oprettelse).

### Den rigtige password-politik

Fra kurset, modulet *Enforcing the Right Password Policy*:

- **Brugere vælger simple adgangskoder og genbruger dem**, uanset hvilke regler man viser dem. Den bedste løsning er at blive mindre afhængig af adgangskoder (passwordless, passkeys). Kurset tager det op i næste modul.
- **Styrke, ikke kompleksitet.** Længde gør en adgangskode stærkere, fordi antallet af muligheder vokser eksponentielt med hvert tegn. Krav om store og små bogstaver, tal og symboler hjælper næsten ikke. Alle gør det samme: stort forbogstav, `a` → `@`, `e` → `3`, et tal og et udråbstegn til sidst. Lækkede adgangskoder viser mønsteret, og knækkeværktøjer kender det.
- **Kun en minimumslængde.** En lav maksimumlængde er et advarselstegn. En hash har samme længde, uanset om adgangskoden er 8 eller 256 tegn. En kort grænse kan tyde på, at adgangskoden gemmes usikkert eller i gammel teknologi. *Min tilføjelse:* En meget høj grænse (fx 64–128 tegn) er fornuftig. bcrypt bruger kun de første 72 bytes, og meget lange input kan bruges til at belaste serveren.
- **Bloker ikke kopier/indsæt eller autofyld.** Det stopper password managers. Scriptede angreb stoppes med rate limiting og ikke med tricks i brugerfladen. Malware, der kan læse udklipsholderen, kan sikkert også læse tastetryk.
- **Ingen tvungen udløb.** Brugere lægger bare 1 til tallet til sidst. Når en adgangskode er lækket, bliver den udnyttet hurtigt, så et skift hver tredje måned hjælper ikke. Skift kun ved mistanke om læk.
- **Hvad virker:** Vis brugeren en styrkemåler, mens der tastes (zxcvbn), og afvis adgangskoder fra kendte læk (Pwned Passwords).
- **I en virksomhed:** Drop tvungent udløb, fjern meningsløse kompleksitetskrav, hæv minimumslængden lidt og opfordr til passphrases. En password-politik skal også formidles og trænes, fx med øvelser og phishing-simuleringer.

Det stemmer med NIST SP 800-63B og OWASP A07 ("Do not enforce composition rules", "avoid forced periodic password rotation").

### Rate limiting

Begræns antal loginforsøg, både pr. IP og pr. konto. Pas på, at en fast kontolås ikke bliver et nyt problem: en angriber kan låse andres konti ved bevidst at taste forkert. OWASP anbefaler derfor at forsinke forsøgene stigende i stedet for at låse kontoen, og at logge og give besked ved mistænkelig aktivitet. Rate limiting gælder også for gendannelse af adgangskode og indtastning af 2FA-koder. En 6-cifret kode kan gættes, hvis der ikke er en grænse.

### CIA

Fortrolighed (Confidentiality), integritet (Integrity) og tilgængelighed (Availability). For A07:

- **Fortrolighed:** En overtaget konto giver adgang til brugerens data.
- **Integritet:** Angriberen kan ændre data og handle i brugerens navn.
- **Tilgængelighed:** Kontolås og hårde rate limits kan gøre login utilgængeligt for den rigtige bruger.

Forsvar mod A07 kan altså svække tilgængeligheden, hvis det laves forkert.

### Statisk kodeanalyse (SAST)

Kurset nævner SonarQube. Andre værktøjer til Java:

- [Semgrep](https://semgrep.dev/) – regler for blandt andet hardkodede hemmeligheder og svag kryptografi
- [GitHub CodeQL / code scanning](https://docs.github.com/en/code-security/code-scanning) – gratis til offentlige repos og kan køre i GitHub Actions ligesom Dependabot
- [SpotBugs med Find Security Bugs](https://find-sec-bugs.github.io/) – plugin til Maven/Gradle
- [Gitleaks](https://github.com/gitleaks/gitleaks) – leder efter adgangskoder og nøgler i koden og git-historikken

Begrænsning: SAST finder typisk hardkodede adgangskoder (CWE-798), svage hashfunktioner og usikre tilfældige tal. Det finder sjældent logiske fejl, fx at trin 2 i loginforløbet kan springes over. Det kræver manuel test.

### Nyt session-ID ved login

Serveren skal altid udstede et nyt session-ID efter login. Det beskytter mod **session fixation** (CWE-384), hvor angriberen giver offeret et session-ID, han allerede kender, og bruger det, når offeret er logget ind.

Rettelse til mine noter: Et nyt ID ved login beskytter ikke mod, at et ID bliver **stjålet**, efter at brugeren er logget ind. Det gør i stedet:

- Cookies med `HttpOnly` (kan ikke læses af JavaScript, fx ved XSS), `Secure` (kun over HTTPS) og `SameSite`.
- Ikke at gemme session-ID eller token i `localStorage`, hvor al JavaScript på siden kan læse det.
- Kort levetid og udløb ved logout og inaktivitet.

For miseOS, der bruger JWT: Login giver altid et nyt token, så session fixation er mindre relevant. De vigtige spørgsmål er i stedet, hvor klienten gemmer tokenet, hvor længe det gælder, og om det kan gøres ugyldigt ved logout.

### Flere pointer fra OWASP og NIST

- **Samme fejlbesked:** "Forkert brugernavn eller adgangskode" i stedet for "brugeren findes ikke". Ellers kan man finde ud af, hvilke konti der findes. Det gælder også svartiden og "glemt adgangskode".
- **Længde frem for regler:** NIST SP 800-63B anbefaler lange adgangskoder og at tillade alle tegn og password managers. Revision 4 kræver mindst 15 tegn, når adgangskoden er eneste faktor, og mindst 8 tegn, når den indgår i MFA. Det skal jeg kontrollere i standarden. Der skal ikke stilles krav om specialtegn, og adgangskoder skal ikke skiftes med faste mellemrum.
- **Brug et færdigt system:** OWASP anbefaler gennemprøvede login- og identitetssystemer frem for at bygge sit eget.

## Kodeidé: tjek mod Pwned Passwords ved oprettelse

Idé til miseOS, ikke afprøvet. Brugeren må ikke oprette sig eller skifte adgangskode, hvis adgangskoden findes i Have I Been Pwned. Tjekket ligger på serveren, så det ikke kan omgås ved at kalde API'et direkte uden om frontend.

### Sådan virker range-API'et

1. Lav en SHA-1-hash af adgangskoden, fx `5BAA61E4C9B93F3F0682250B6CF8331B7EE68FD8` for `password`.
2. Send kun de første 5 tegn: `GET https://api.pwnedpasswords.com/range/5BAA6`
3. Svaret er en liste med linjer `SUFFIX:ANTAL` for alle hashes, der starter med `5BAA6`.
4. Find resten af hashen (`1E4C9B93...`) i listen. Står den der, er adgangskoden lækket.

Headeren `Add-Padding: true` fylder svaret op med falske linjer med antal 0. Så kan man ikke se på svarets størrelse, hvilket prefix der blev slået op.

SHA-1 bruges **kun** til opslaget, fordi det er det format, API'et kræver. Adgangskoden gemmes stadig med bcrypt eller Argon2. En SAST-scanner vil måske markere SHA-1, og så skal jeg kunne forklare hvorfor.

### Java (17+)

```java
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.StandardCharsets;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.time.Duration;
import java.util.HexFormat;

public class PwnedPasswordChecker {

    private static final String RANGE_URL = "https://api.pwnedpasswords.com/range/";

    private final HttpClient client = HttpClient.newBuilder()
            .connectTimeout(Duration.ofSeconds(3))
            .build();

    /** Returnerer hvor mange gange adgangskoden findes i kendte læk (0 = ikke fundet). */
    public int timesPwned(String password) throws IOException, InterruptedException {
        String hash = sha1Hex(password);
        String prefix = hash.substring(0, 5);
        String suffix = hash.substring(5);

        HttpRequest request = HttpRequest.newBuilder(URI.create(RANGE_URL + prefix))
                .header("Add-Padding", "true")
                .header("User-Agent", "miseOS-password-check")
                .timeout(Duration.ofSeconds(3))
                .GET()
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        if (response.statusCode() != 200) {
            throw new IOException("Pwned Passwords svarede " + response.statusCode());
        }

        return response.body().lines()
                .map(line -> line.split(":"))
                .filter(parts -> parts[0].equalsIgnoreCase(suffix))
                .mapToInt(parts -> Integer.parseInt(parts[1].trim()))
                .findFirst()
                .orElse(0);
    }

    private static String sha1Hex(String value) {
        try {
            byte[] digest = MessageDigest.getInstance("SHA-1")
                    .digest(value.getBytes(StandardCharsets.UTF_8));
            return HexFormat.of().withUpperCase().formatHex(digest);
        } catch (NoSuchAlgorithmException e) {
            throw new IllegalStateException("SHA-1 findes ikke i JVM'en", e);
        }
    }
}
```

### Brug ved oprettelse

```java
if (password.length() < 12 || password.length() > 128) {
    throw new ValidationException("Adgangskoden skal være mellem 12 og 128 tegn");
}
if (pwnedChecker.timesPwned(password) > 0) {
    throw new ValidationException(
        "Adgangskoden findes i kendte datalæk. Vælg en anden – gerne en lang sætning.");
}
String hash = BCrypt.hashpw(password, BCrypt.gensalt(12)); // eller Argon2id
```

Samme tjek skal også ske ved skift og nulstilling af adgangskode, ikke kun ved oprettelse.

### Det skal jeg tage stilling til

- **Hvis API'et er nede:** Skal oprettelsen afvises (fail closed) eller tillades (fail open)? Fail open med logning er nok mest realistisk for miseOS, så login ikke afhænger af en ekstern tjeneste. Det er et designvalg, jeg skal kunne begrunde.
- **Grænse:** Afvise ved `> 0`, eller kun over et vist antal? NIST siger, at adgangskoder fra kendte læk skal afvises, så `> 0` er det sikre valg.
- **Fejlbesked:** Fortæl brugeren hvorfor, uden at gentage adgangskoden.
- **Logning:** Log aldrig adgangskoden eller hashen, kun at tjekket fejlede.
- **Test:** En unit test, hvor `password` skal give et antal over 0 og en tilfældig UUID skal give 0. HTTP-kaldet kan mockes, så testen ikke afhænger af nettet.

## Det jeg vil undersøge i miseOS

miseOS bruger JWT. Ud fra listen ovenfor vil jeg undersøge:

- [ ] Om registrering og skift af adgangskode kunne tjekke mod Pwned Passwords.
- [ ] Køre SonarQube eller Semgrep på miseOS og se, hvad der findes af hardkodede hemmeligheder og svag kryptografi.
- [ ] Hvor klienten gemmer JWT'en (`localStorage` eller cookie).

- [ ] Hvordan adgangskoder gemmes, og med hvilken hashfunktion.
- [ ] Om login giver samme fejlbesked ved forkert brugernavn og forkert adgangskode.
- [ ] Om der er en grænse for mislykkede loginforsøg.
- [ ] Hvilke claims der valideres i JWT'en, og hvor lang udløbstiden er.
- [ ] Hvad logout betyder, når der bruges JWT. Jeg har ikke fundet en logout-route; se [noterne om miseOS](adgangskontrol.md).
- [ ] Om der er hardkodede nøgler eller adgangskoder i koden eller konfigurationen.

## Øvelser

- [ ] PortSwigger: Authentication vulnerabilities-afsnittet.
- [ ] PortSwigger: 2FA simple bypass. Forklare hvilken kontrol i loginforløbet der mangler.

## Forbindelse til Java4Shell

[Java4Shell](java4shell.md) viser, at login også kan være en vej ind på et helt andet niveau. Basic authentication-headeren og JWT'er er Base64-kodede og dekodes før brugeren er logget ind. Det er derfor, fejlen kan udnyttes uden login i fx Keycloak. Selve fejlen hører under A03, ikke A07, men den viser, at loginkoden er noget af det første, en angriber kan nå.

## Kilder

- [OWASP Top 10:2025 – A07 Authentication Failures](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)
- [NIST SP 800-63B](https://pages.nist.gov/800-63-4/sp800-63b.html)
- [PortSwigger – Authentication vulnerabilities](https://portswigger.net/web-security/authentication)
- [Pluralsight – OWASP: Authentication Failures](https://www.pluralsight.com/courses/owasp-authentication-failures)
- [Have I Been Pwned – Pwned Passwords API](https://haveibeenpwned.com/API/v3#PwnedPasswords)
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)

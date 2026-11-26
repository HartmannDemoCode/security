# JWT – JSON Web Tokens

Status: PortSwiggers JWT-afsnit er læst, og labs er løst. Jeg har analyseret miseOS' JWT-kode ud fra angrebene i labs.

Noterne hører under [A07 – Authentication Failures](authentication.md), fordi JWT'en i miseOS er det, der beviser, hvem brugeren er, efter login.

## Hører JWT under A07?

Ja, mest. Et JWT er et sessionstoken, og A07 dækker sessioner og tokens, herunder at `aud`, `iss` og udløb ikke bliver kontrolleret. Hvis jeg kan forfalske et token, kan jeg blive en anden bruger, og så kan systemet ikke stole på, hvem brugeren er.

Der er overlap med andre kategorier:
- **A01 Broken Access Control:** OWASP nævner selv "manipulation af metadata, fx et JWT" under A01. Det er tilfældet, når jeg ændrer `role` til `ADMIN` i tokenet.
- **A04 Cryptographic Failures:** En svag signeringsnøgle eller et forkert valg af algoritme er en kryptografisk fejl.

Jeg placerer det under A07, fordi konsekvensen er den samme: serveren tror, jeg er en anden.

## Hvad er et JWT?

Tre Base64url-kodede dele adskilt af punktum: `header.payload.signatur`.

- **Header:** hvilken algoritme der er brugt (`alg`) og evt. hvor nøglen findes (`kid`, `jwk`, `jku`).
- **Payload:** claims, fx `sub`, `exp`, `iss` og egne claims som `role`.
- **Signatur:** beviser, at header og payload ikke er ændret.

Det vigtigste jeg tog med: **payloaden er ikke krypteret, kun kodet.** Alle kan læse den på jwt.io. Der må ikke ligge hemmeligheder i den. Det eneste der beskytter tokenet, er signaturen, og den er kun noget værd, hvis serveren faktisk tjekker den korrekt.

## Det jeg tager med fra PortSwigger-labs

Alle angrebene går ud på det samme: **serveren stoler på noget i tokenet, som angriberen selv bestemmer.**

| Lab | Fejlen på serveren | Det skal jeg huske |
|---|---|---|
| Unverified signature | Serveren dekoder tokenet uden at tjekke signaturen. | `decode` er ikke det samme som `verify`. |
| Flawed signature verification | Serveren accepterer `"alg": "none"`. | Afvis usignerede tokens. |
| Weak signing key | HMAC-nøglen kan knækkes offline med hashcat og en ordliste. | Nøglen skal være lang og tilfældig, ikke en sætning. |
| JWK header injection | Serveren bruger den nøgle, der står i tokenets `jwk`-header. | Nøglen skal komme fra serveren, aldrig fra tokenet. |
| JKU header injection | Serveren henter nøglen fra den URL, der står i `jku`. | Samme fejl: angriberen peger på sin egen nøgle. |
| Kid header path traversal | `kid` bruges som filsti, fx `../../../dev/null`, så nøglen bliver tom. | `kid` er input fra brugeren og skal valideres. |
| Algorithm confusion | Serveren lader `alg` i headeren bestemme algoritmen. Den offentlige RSA-nøgle bruges som HMAC-hemmelighed. | Lås algoritmen fast på serveren. |

Hashcat-delen hænger sammen med mine noter om [Hashcat og Hydra](adgangskontrol/hashcat-og-hydra.md). Et JWT med HS256 er en hash, man kan knække offline, uden at serveren opdager noget.

## Når jeg selv laver web development

1. **Verificér altid signaturen**, før jeg læser en eneste claim.
2. **Bestem algoritmen på serveren.** Headeren er input fra angriberen.
3. **Nøglen kommer fra serverens konfiguration**, aldrig fra `jwk`, `jku`, `x5u` eller en fri `kid`.
4. **HMAC-nøglen skal være mindst 256 bit og tilfældig**, fx `openssl rand -base64 32`. Ikke et ord eller en sætning.
5. **Tjek claims:** `exp` skal findes og være i fremtiden, `iss` (og `aud` hvis der er flere tjenester) skal passe.
6. **Kort levetid.** Et JWT kan ikke trækkes tilbage, så det gælder, til det udløber, også efter logout eller ændret rolle.
7. **Ingen følsomme data i payloaden.**
8. **Brug et kendt bibliotek og hold det opdateret** i stedet for at skrive JWT-håndtering selv.

## Nimbus JOSE+JWT – hvad kan gøre programmet svagt?

Nimbus er et godt bibliotek, men det tillader også at bruge det forkert. Det jeg har fundet:

- **`SignedJWT.parse()` verificerer ikke.** Den dekoder bare. Kalder man `getJWTClaimsSet()` uden at have kaldt `verify()` først, er man i "Unverified signature"-labben.
- **`JWTParser.parse()` kan returnere et `PlainJWT`** (`alg: none`). Bruger man den generelle parser og læser claims uden at tjekke typen, accepteres usignerede tokens. `SignedJWT.parse()` afviser derimod `none`.
- **Algoritmen vælges ud fra den nøgle, man giver verifieren.** Vælger man verifier ud fra `jwt.getHeader().getAlgorithm()`, åbner man for algorithm confusion. Med kun en HMAC-nøgle og `MACVerifier` er risikoen lille, fordi en RS256-token bliver afvist.
- **`getHeader().getJWK()`, `getJWKURL()` og `getKeyID()`** er værdier fra angriberen. Nimbus henter ikke selv nøgler fra `jku`, men man kan let komme til at bruge dem i egen kode.
- **`verify()` tjekker kun signaturen, ikke `exp`, `nbf` eller `iss`.** Det skal man selv gøre, eller bruge `DefaultJWTProcessor` med en `JWSVerificationKeySelector` (låser algoritmen) og en `DefaultJWTClaimsVerifier` (tjekker claims). Processoren afviser også usignerede tokens som standard.
- **`MACSigner` og `MACVerifier` kræver mindst 256 bit nøgle** og kaster ellers `KeyLengthException`. Det beskytter mod korte nøgler, men ikke mod en lang nøgle, der kan gættes.
- **Gamle versioner har kendte CVE'er**, fx DoS via en meget stor PBES2-iterationstæller (CVE-2023-52428, rettet i 9.37.2) og stack overflow via dybt indlejret JSON (CVE-2025-53864, rettet i 10.0.2). Det er A03-problemer, men de rammer login-koden.

## miseOS – min egen analyse af JWT-koden

miseOS bruger Nimbus 10.7 med HS256. Koden ligger i [SecurityService.java](https://github.com/Mortenjenne/miseOS/blob/main/src/main/java/app/services/impl/SecurityService.java). Efter PortSwigger-labs har jeg gennemgået min egen kode og tjekket, om angrebene fra labs ville virke mod den.

### Angrebene fra labs mod min kode

| Angreb | Virker det mod miseOS? | Hvorfor |
|---|---|---|
| Unverified signature | Nej | `SignedJWT.parse()` efterfulgt af `verify(new MACVerifier(...))`, før claims læses. |
| `alg: none` | Nej | `SignedJWT.parse()` afviser `none`, så jeg aldrig får et `PlainJWT`. |
| Weak signing key | Nej | `SECRET_KEY` er 32 tilfældige tegn genereret i min password manager. Det er langt over 128 bits entropi, så en ordliste eller brute force med hashcat når det ikke. Appen nægter også at starte, hvis nøglen er under 32 bytes. Jeg kendte risikoen ved en svag nøgle, før jeg lavede labs, og valgte derfor en tilfældig nøgle fra starten. Labben bekræftede valget. |
| JWK/JKU header injection | Nej | Nøglen kommer kun fra miljøvariablen. Jeg læser aldrig `jwk` eller `jku` fra headeren. |
| Kid path traversal | Nej | `kid` bliver ikke brugt. Der er kun én nøgle. |
| Algorithm confusion | Nej | Der er kun en HMAC-nøgle, og `MACVerifier` afviser RS256. |
| Udløbet eller forfalsket token | Nej | `exp` skal findes, `nbf` tjekkes, og `iss` skal passe. Rollen valideres mod `UserRole`-enum'et. |

Det jeg er blevet klogere på: de fejl, PortSwigger viser, opstår typisk, når koden lader **headeren** bestemme noget. Det gør min kode ikke, så header injection rammer ikke miseOS.

### Mine valg og deres konsekvenser

- **Tokenet gælder i 30 minutter** (`TOKEN_EXPIRE_TIME`). Det er grænsen for, hvor længe et stjålet token kan bruges.
- **Rollen ligger i tokenet.** `SecurityController.authenticate()` slår ikke brugeren op i databasen, men stoler på rollen i det verificerede token. Hvis jeg fjerner en brugers rolle eller sletter brugeren, virker det gamle token stadig i op til 30 minutter. Se afvejningen nedenfor.
- **Logout sker kun på klienten.** Tokenet slettes i browseren, men serveren kan ikke trække det tilbage. Et kopieret token virker stadig, til det udløber.
- **Tokenet ligger i `localStorage`.** Det kan JavaScript læse, så et XSS-hul i miseOS-web vil kunne stjæle tokenet. Alternativet er en `HttpOnly`-cookie, men så skal jeg til gengæld tænke på CSRF. Lige nu er forsvaret, at React escaper output, og at tokenet kun gælder i 30 minutter. Der er ingen `dangerouslySetInnerHTML`, `innerHTML` eller `eval` i miseOS-web.
- **WebSocket-forbindelsen sender tokenet i URL'en** (`?token=...` i `useNotificationWebSocket.js`), fordi browserens WebSocket-API ikke kan sætte en `Authorization`-header. Backend accepterer derfor både headeren og `token`-parameteren. Ulempen er, at URL'er ofte bliver logget, fx i reverse proxyens access-log, så tokens kan ende i logfiler. A07 nævner netop session-ID'er i URL'en som en fejl. Den korte levetid begrænser skaden.
- **Hvis nøglen lækker**, kan alle tokens forfalskes. Så skifter jeg `SECRET_KEY` (GitHub Secrets og dropletten). Det eneste, der sker, er at alle brugere bliver logget ud.

### Afvejning: rollen fra tokenet eller fra databasen?

Det er et bevidst valg, ikke en fejl. Begge løsninger kan forsvares, men de giver forskellige risici.

| | Rollen fra tokenet (miseOS i dag) | Opslag i databasen ved hver request |
|---|---|---|
| **Fordel** | Intet opslag pr. request. Serveren behøver ikke huske noget, og tokenet er selv beviset. Det er pointen med JWT. | Ændringer slår igennem med det samme. En slettet bruger afvises, og en nedgraderet rolle gælder fra næste request. |
| **Ulempe** | En ændret rolle eller en slettet bruger slår først igennem, når tokenet udløber. | Et opslag pr. request. Det koster næsten intet i miseOS, men så mister JWT det meste af sin fordel frem for almindelige sessioner. |

Den konkrete risiko i miseOS: Hvis jeg fjerner en head chef, kan personen stadig ændre roller og slette brugere i op til 30 minutter.

Der findes mellemveje:
1. **Kort levetid.** Den har jeg allerede med 30 minutter.
2. **Kun følsomme handlinger tjekkes i databasen:** rolleskift, sletning af brugere og ændring af e-mail og adgangskode. Det er der, en forældet rolle gør mest skade.
3. **`tokenVersion` på brugeren.** Tallet lægges i tokenet og sammenlignes med databasen. Når jeg tæller det op, bliver alle brugerens tokens ugyldige med det samme. Det giver også rigtig logout og tilbagekaldelse, som miseOS mangler i dag.

**Min beslutning:** Jeg beholder rollen fra tokenet og de 30 minutter og tilføjer et databasetjek på HEAD_CHEF-handlingerne. Det løser den konkrete risiko uden at ændre hele designet. Hvis jeg senere vil have rigtig logout, er `tokenVersion` næste skridt.

Det hænger sammen med *complete mediation* fra [adgangskontrolnoterne](adgangskontrol.md#grundprincipper): kurset advarer mod at genbruge en adgangsafgørelse, efter at rettighederne er ændret. En rolle i et JWT er netop en gemt afgørelse, der gælder til udløb.

### Det følger jeg op på

- [ ] Tilføje et databasetjek af rollen på HEAD_CHEF-handlingerne (rolleskift og sletning af brugere) og teste, at en nedgraderet head chef bliver afvist med det samme.

- [x] Søge i miseOS-web efter `dangerouslySetInnerHTML` og andre steder, hvor XSS kunne stjæle tokenet fra `localStorage`. Ingen fundet.
- [ ] Tjekke, om reverse proxyen logger query-strengen på `/notifications`, og i så fald fjerne `token` fra loggen. Alternativet er en kortlivet engangsbillet til WebSocket-forbindelsen i stedet for selve JWT'en.
- [ ] Overveje at låse algoritmen til HS256 med `DefaultJWTProcessor`. Det er ikke et hul i dag, fordi en angriber stadig skal kende nøglen, men det gør hensigten tydelig.
- [ ] Tilføje `aud`, hvis miseOS får flere tjenester. Med én backend er det ikke nødvendigt.

### Gamle tokens i git-historikken

Et par JWT'er fra mine testbrugere har ligget i `http-client.env.json` i den offentlige git-historik (se [authentication-noterne](authentication.md#det-jeg-vil-undersøge-i-miseos)). Det er præcis den situation, "Weak signing key"-labben handler om: en angriber kan tage et token og prøve at knække nøglen offline med hashcat, uden at serveren opdager det. Fordi min nøgle er tilfældig, giver det ikke adgang til at forfalske nye tokens, og de gamle tokens er udløbet for længst.

Relateret, men ikke en JWT-fejl: Login kan afsløre via svartiden, om en e-mail findes. Det hører under A07 og står i [authentication-noterne](authentication.md#miseos-mod-de-samme-tricks).

## Kilder

- [PortSwigger – JWT attacks](https://portswigger.net/web-security/jwt)
- [Nimbus JOSE+JWT – dokumentation og eksempler](https://connect2id.com/products/nimbus-jose-jwt/examples)
- [OWASP JWT for Java Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_for_Java_Cheat_Sheet.html)
- [RFC 8725 – JWT Best Current Practices](https://www.rfc-editor.org/rfc/rfc8725)
- [OWASP Top 10:2025 – A07 Authentication Failures](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)

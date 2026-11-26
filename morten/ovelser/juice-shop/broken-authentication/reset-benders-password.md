# Reset Bender's Password (⭐⭐⭐⭐⭐)

**Kategori:** Broken Authentication – A07  
**Dato:** 2026-11-26  
**Miljø:** OWASP Juice Shop i Docker, Burp Suite Community  
**Status:** Løst  
**Tidsforbrug:** ca. 15 min

## Mål

Nulstille adgangskoden til Benders konto via "Forgot Password" med det oprindelige svar på hans sikkerhedsspørgsmål.

## Fremgangsmåde

1. **Find e-mailen.** Jeg fandt Bender i en produktanmeldelse i shoppen, hvor hans e-mail stod: `bender@juice-sh.op`.
2. **Find spørgsmålet.** Jeg skrev e-mailen under "Forgot Password", og Juice Shop viste sikkerhedsspørgsmålet: *Company you first work for as an adult?*
3. **Research svaret (OSINT).** Jeg søgte på karakteren og fandt firmaet, han først arbejdede for: **Stop'n'Drop**.
4. **Byg en lille brute force-liste.** Ordet var klart, men ikke tegnsætningen (apostroffer, bindestreger, store/små bogstaver). I stedet for tilfældige ord lavede jeg en kort liste med varianter af samme ord.
5. **Fandt svaret.** Jeg kørte listen i Intruder (Sniper) på `answer`-feltet. Der er ingen rate limiting på `reset-password`, så varianterne kunne køres frit. Responsen gav succes på **`Stop'n'Drop`**, og password blev nulstillet.

## Request

```http
POST /rest/user/reset-password HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{"email":"bender@juice-sh.op","answer":"§Stop'n'Drop§","new":"<ny adgangskode>","repeat":"<ny adgangskode>"}
```

Brute force-liste (varianter af samme ord):

```
Stop'n'Drop
Stop'n Drop
Stop n Drop
Stop-n-Drop
StopnDrop
Stop'N'Drop
```

Bemærk: JSON kræver escaping af `"`, men ikke af `'`. Apostroffer skrives direkte i payloaden.

## Manglende kontrol

- **Ingen rate limiting / lockout** på `reset-password`.
- Svaret bygger på et sikkerhedsspørgsmål, hvor svaret kan findes via OSINT (firma, by, skole).

## Rettelse

- Rate limiting + lockout på nulstilling.
- Fjern sikkerhedsspørgsmål. NIST SP 800-63B fraråder dem.
- Nulstil via engangslink på e-mail med kort levetid.

## Forskel fra Jim-opgaven

Hos [Jim](reset-jims-password.md) var svaret et ukendt mellemnavn fra en stor ordliste. Her kendte jeg **ordet** fra OSINT — det eneste ukendte var **tegnsætningen**. Så i stedet for en stor liste lavede jeg en kort liste med stavevarianter af samme ord. Samme svaghed (ingen rate limiting), men en mindre og mere målrettet liste.

## Det jeg lærte

Når OSINT giver dig selve ordet, men ikke den præcise skrivemåde, er en lille liste med varianter nok — så længe serveren ikke begrænser antal forsøg.

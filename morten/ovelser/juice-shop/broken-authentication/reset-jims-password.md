# Reset Jim's Password (⭐⭐⭐)

**Kategori:** Broken Authentication – A07  
**Dato:** 2026-11-26  
**Miljø:** OWASP Juice Shop i Docker, Burp Suite Community  
**Status:** Løst  
**Tidsforbrug:** ca. 20min

## Mål

Nulstille adgangskoden til Jims konto via "Forgot Password" med det oprindelige svar på hans sikkerhedsspørgsmål.

## Forventet adgangsregel

Kun ejeren af kontoen må kunne nulstille adgangskoden. Det kræver noget, kun ejeren kender eller har, og svaret på et sikkerhedsspørgsmål må hverken kunne gættes eller brute forces.

## Fremgangsmåde

1. **Find e-mailen.** Jims konto er `jim@juice-sh.op`.
2. **Find spørgsmålet.** Når jeg skriver e-mailen under "Forgot Password", viser Juice Shop sikkerhedsspørgsmålet: *Your eldest siblings middle name?*
3. **Test for rate limiting.** Jeg tog nulstillings-requesten over i Burp og sendte den manuelt ~10 gange med forskellige svar i `answer`-feltet. Jeg blev **ikke** låst ude eller throttlet — ingen rate limiting. Dermed er brute force realistisk.
4. **Brute force i Intruder.** Jeg sendte requesten til **Intruder**, satte payload-markøren på `answer`-feltet og kørte **Sniper** med en ordliste over de **100 mest almindelige amerikanske mandlige mellemnavne**.
5. **Fandt svaret.** Responsen skiftede fra fejl til succes ved **`Samuel`**.
6. **Nulstil adgangskoden.** Jeg skrev `Samuel` som svar sammen med en ny adgangskode, og opgaven blev løst.

## Request

Spørgsmålet hentes ud fra e-mailen:

```http
GET /rest/user/security-question?email=jim@juice-sh.op HTTP/1.1
Host: localhost:3000
```

Nulstilling (payload-markøren sat på `answer` i Intruder):

```http
POST /rest/user/reset-password HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{"email":"jim@juice-sh.op","answer":"§Samuel§","new":"<ny adgangskode>","repeat":"<ny adgangskode>"}
```

Bemærk: Så længe der ikke er rate limiting, er feltet `answer` reelt kun beskyttet af, hvor stor ordlisten skal være. Et mellemnavn ligger i et lille, forudsigeligt rum — perfekt til Sniper.

## Resultat og bevis

Responsen på det rigtige svar gav en vellykket nulstilling, og Score Board viste opgaven som løst.

## Manglende kontrol

- **Ingen rate limiting / lockout** på `reset-password`. Jeg kunne sende ubegrænset mange svar uden straf.
- Svaret på sikkerhedsspørgsmålet ligger i et **lille, gættebart rum** (et mellemnavn), og karakteren er offentligt kendt (OSINT).
- Endpointet `security-question` afslører også, om en e-mail findes.

## Konsekvens

En angriber kan overtage kontoen rent automatiseret: ingen OSINT nødvendig, bare en ordliste og tålmodighed. Jo mindre og mere forudsigeligt svaret er (navn, by, farve), jo hurtigere falder det.

## Rettelse

- **Rate limiting + lockout** på nulstilling (fx eksponentielt backoff, CAPTCHA efter få forsøg).
- Fjern sikkerhedsspørgsmål. NIST SP 800-63B fraråder dem.
- Nulstil via et tilfældigt engangslink sendt til brugerens e-mail med kort levetid.
- Samme svar/respons uanset om e-mailen findes.

## Forskel fra Bjoern-opgaven

I [Bjoerns opgave](bjoerns-favorite-pet.md) var brute force ikke realistisk — et kæledyrsnavn kan være hvad som helst, så svaret skulle **findes** (OSINT). Her var svaret et **mellemnavn** fra et lille rum, og fordi der **ikke var rate limiting**, kunne det **gættes** med Intruder. Samme sårbarhedsklasse (svagt, offentligt sikkerhedsspørgsmål), men to forskellige angrebsveje afhængigt af svarets entropi og serverens forsvar.

## Overførsel til miseOS

miseOS har ingen gendannelse via sikkerhedsspørgsmål, men pointen om **rate limiting** er bred: ethvert endpoint, der verificerer en hemmelighed (login, 2FA, nulstilling), skal throttle og låse efter få forsøg. Tjek at login og eventuelle fremtidige nulstillings-flows har lockout. Se [A07-noterne](../../../emner/authentication.md#miseos-mod-de-samme-tricks).

## Det jeg lærte

Et svagt sikkerhedsspørgsmål bliver farligt, når serveren ikke begrænser antal forsøg. Først **test for rate limiting** (send requesten manuelt et par gange) — er der ingen, så afgør svarets entropi, om det kan brute forces. Sniper + en målrettet ordliste slår et lille svarrum hurtigt.

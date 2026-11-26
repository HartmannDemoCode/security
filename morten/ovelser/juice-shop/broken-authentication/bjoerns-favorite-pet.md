# Bjoern's Favorite Pet (⭐⭐⭐)

**Kategori:** Broken Authentication – A07  
**Dato:** 2026-11-26  
**Miljø:** OWASP Juice Shop i Docker, Burp Suite Community  
**Status:** Løst  
**Tidsforbrug:** ca. X min

## Mål

Nulstille adgangskoden til Bjoerns OWASP-konto via "Forgot Password" med det rigtige svar på hans sikkerhedsspørgsmål.

## Forventet adgangsregel

Kun ejeren af kontoen må kunne nulstille adgangskoden. Det kræver noget, kun ejeren har, fx adgang til sin e-mail, og ikke noget, andre kan finde på nettet.

## Fremgangsmåde

1. **Find personen.** Bjoern er manden bag Juice Shop, **Bjoern Kimminich**. Kontoen er `bjoern@owasp.org`.
2. **Find spørgsmålet.** Når jeg skriver e-mailen under "Forgot Password", viser Juice Shop sikkerhedsspørgsmålet: *Name of your favorite pet?*
3. **Brug hintet.** Tooltippet i Score Board antyder, at han har nævnt svaret, mens et kamera kørte.
4. **Søg på nettet (OSINT).** Jeg fandt en video, hvor Bjoern demonstrerer Juice Shop og selv skriver sit kæledyrs navn som svar på sikkerhedsspørgsmålet.
5. **Nulstil adgangskoden.** Jeg skrev svaret og en ny adgangskode, og opgaven blev løst.

Blindgyder: Brute force af svaret er ikke realistisk, fordi et navn kan være hvad som helst. Svaret skulle findes, ikke gættes.

## Request

Spørgsmålet hentes ud fra e-mailen:

```http
GET /rest/user/security-question?email=bjoern@owasp.org HTTP/1.1
Host: localhost:3000
```

Nulstilling:

```http
POST /rest/user/reset-password HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{"email":"bjoern@owasp.org","answer":"Zaya","new":"<ny adgangskode>","repeat":"<ny adgangskode>"}
```

Bemærk: Det første endpoint afslører også, om en e-mail findes. En ukendt e-mail giver intet spørgsmål.

## Resultat og bevis

Adgangskoden blev nulstillet, og Score Board viste opgaven som løst.

## Manglende kontrol

Nulstillingen stoler på et sikkerhedsspørgsmål, hvis svar er offentligt tilgængeligt. Sikkerhedsspørgsmål er en svag gendannelse af konti, som OWASP A07 nævner direkte. Der er heller ingen rate limiting på svarene.

## Konsekvens

En angriber kan overtage en konto alene med information fra sociale medier, videoer eller blogs. Kæledyr, fødeby, skole og mors pigenavn er typisk offentlige eller lette at finde.

## Rettelse

- Fjern sikkerhedsspørgsmål. NIST SP 800-63B fraråder dem.
- Nulstil via et tilfældigt engangslink sendt til brugerens e-mail med kort levetid.
- Samme svar uanset om e-mailen findes.
- Rate limiting på nulstilling.

## OSINT: pas på med offentlig information

OSINT (Open Source Intelligence) er information, alle kan finde: sociale medier, videoer, foredrag, GitHub, LinkedIn og blogs. Opgaven viser, at et "hemmeligt" svar ikke er hemmeligt, hvis man selv har delt det. Det jeg tager med:

- Som udvikler: byg aldrig login eller gendannelse på oplysninger, der kan stå på nettet.
- Som bruger: svar på sikkerhedsspørgsmål med en tilfældig værdi fra password manageren i stedet for det rigtige svar.
- Pas på, hvad der ligger offentligt: e-mails, navne og testdata i en offentlig git-historik er også OSINT. Det så jeg selv i miseOS.

## Overførsel til miseOS

miseOS har ingen funktion til glemt adgangskode og ingen sikkerhedsspørgsmål. Hvis jeg tilføjer en, skal den bruge et engangslink på e-mail. OSINT-delen er relevant: mine testbrugeres e-mails lå i den offentlige git-historik, og registreringen afslører, om en e-mail findes. Se [A07-noterne](../../../emner/authentication.md#miseos-mod-de-samme-tricks).

## Det jeg lærte

Et sikkerhedsspørgsmål er kun så hemmeligt som det, personen har delt offentligt. Kontoovertagelse kræver ikke altid teknik; nogle gange er en søgning på nettet nok.

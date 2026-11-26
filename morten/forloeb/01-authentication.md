# Login og sessioner – A07

Status: I gang. OWASP A07:2025 er læst, Pluralsight-kurset er set, og PortSwiggers authentication- og JWT-afsnit er løst. Juice Shop Broken Authentication er i gang. Afprøvning i miseOS mangler.

OWASP Top 10:2025: A07 Authentication Failures.

A07 var først en del af [forløbet om login og adgangskontrol](01-adgangskontrol.md). Jeg har givet den sin egen plan, fordi den er blevet større. De to hører stadig sammen: A07 handler om, hvorvidt systemet kan stole på, hvem brugeren er. A01 handler om, hvad brugeren så må.

Mine faglige noter ligger i [A07 – Authentication Failures](../emner/authentication.md).

## Det jeg vil finde ud af

Hvordan kan fejl i login, adgangskoder og sessioner give en angriber adgang til en konto? Hvilke af OWASPs anbefalinger er relevante for miseOS, og hvordan ser de ud i Java?

## Læse og se

- [x] Læse Description, How to prevent og Example attack scenarios i OWASP A07:2025.
- [x] Se Pluralsight-kurset OWASP: Authentication Failures.
- [x] Læse PortSwiggers afsnit om authentication vulnerabilities.
- [x] Læse PortSwiggers afsnit om JWT attacks og gennemgå miseOS' brug af Nimbus JOSE+JWT. Se [JWT-noterne](../emner/jwt.md).
- [ ] Læse de relevante dele af OWASP Authentication, Session Management og Password Storage Cheat Sheets.
- [ ] Kontrollere NIST SP 800-63B's krav til adgangskoders længde.

## Øvelser

- [x] Løse PortSwigger-labben 2FA simple bypass og forklare, hvilken kontrol der mangler i loginforløbet.
- [x] Løse PortSwiggers JWT-labs.
- [ ] Løse Juice Shops opgaver i kategorien Broken Authentication.
- [ ] Prøve at knække et af mine egne miseOS-tokens med hashcat for at tjekke, om `SECRET_KEY` er stærk nok.
- [x] Undersøge, hvordan et manglende trin i et loginforløb kan give adgang, før brugeren er færdig med at logge ind.
- [ ] Prøve kursets demoapp med zxcvbn og Pwned Passwords.
- [ ] Gemme relevante requests og resultater, så jeg kan vise fejlene igen.

## miseOS

- [x] Undersøge, hvordan adgangskoder gemmes, og med hvilken hashfunktion (BCrypt, cost 12).
- [ ] Teste, om login giver samme fejlbesked og samme svartid ved forkert brugernavn og forkert adgangskode.
- [ ] Teste, om der er en grænse for mislykkede loginforsøg.
- [x] Undersøge, hvilke claims der valideres i JWT'en (signatur, `exp`, `nbf`, `iss`).
- [ ] Finde ud af, hvor længe JWT'en gælder, og hvor klienten gemmer den.
- [ ] Afklare, hvad logout betyder, når der bruges JWT.
- [ ] Køre SonarQube eller Semgrep og se efter hardkodede hemmeligheder og svag kryptografi.
- [ ] Afprøve [kodeidéen med Pwned Passwords](../emner/authentication.md#kodeidé-tjek-mod-pwned-passwords-ved-oprettelse) ved oprettelse og skift af adgangskode.

Jeg skriver tydeligt, hvad jeg har afprøvet, og hvad der kun er beskrevet.

## Kilder

- [OWASP A07:2025 – Authentication Failures](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)
- [Pluralsight – OWASP: Authentication Failures](https://www.pluralsight.com/courses/owasp-authentication-failures)
- [PortSwigger – Authentication vulnerabilities](https://portswigger.net/web-security/authentication)
- [PortSwigger – JWT attacks](https://portswigger.net/web-security/jwt)

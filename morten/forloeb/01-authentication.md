# Login og sessioner – A07

Status: I gang. OWASP A07:2025 er læst, og Pluralsight-kurset OWASP: Authentication Failures er set. PortSwiggers authentication-afsnit, 2FA simple bypass og afprøvning i miseOS mangler.

OWASP Top 10:2025: A07 Authentication Failures.

A07 var først en del af [forløbet om login og adgangskontrol](01-adgangskontrol.md). Jeg har givet den sin egen plan, fordi den er blevet større. De to hører stadig sammen: A07 handler om, hvorvidt systemet kan stole på, hvem brugeren er. A01 handler om, hvad brugeren så må.

Mine faglige noter ligger i [A07 – Authentication Failures](../emner/authentication.md).

## Det jeg vil finde ud af

Hvordan kan fejl i login, adgangskoder og sessioner give en angriber adgang til en konto? Hvilke af OWASPs anbefalinger er relevante for miseOS, og hvordan ser de ud i Java?

## Læse og se

- [x] Læse Description, How to prevent og Example attack scenarios i OWASP A07:2025.
- [x] Se Pluralsight-kurset OWASP: Authentication Failures.
- [ ] Læse PortSwiggers afsnit om authentication vulnerabilities.
- [ ] Læse de relevante dele af OWASP Authentication, Session Management og Password Storage Cheat Sheets.
- [ ] Kontrollere NIST SP 800-63B's krav til adgangskoders længde.

## Øvelser

- [ ] Løse PortSwigger-labben 2FA simple bypass og forklare, hvilken kontrol der mangler i loginforløbet.
- [x] Undersøge, hvordan et manglende trin i et loginforløb kan give adgang, før brugeren er færdig med at logge ind.
- [ ] Prøve kursets demoapp med zxcvbn og Pwned Passwords.
- [ ] Gemme relevante requests og resultater, så jeg kan vise fejlene igen.

## miseOS

- [ ] Undersøge, hvordan adgangskoder gemmes, og med hvilken hashfunktion.
- [ ] Teste, om login giver samme fejlbesked ved forkert brugernavn og forkert adgangskode.
- [ ] Teste, om der er en grænse for mislykkede loginforsøg.
- [ ] Undersøge, hvilke claims der valideres i JWT'en, hvor længe den gælder, og hvor klienten gemmer den.
- [ ] Afklare, hvad logout betyder, når der bruges JWT.
- [ ] Køre SonarQube eller Semgrep og se efter hardkodede hemmeligheder og svag kryptografi.
- [ ] Afprøve [kodeidéen med Pwned Passwords](../emner/authentication.md#kodeidé-tjek-mod-pwned-passwords-ved-oprettelse) ved oprettelse og skift af adgangskode.

Jeg skriver tydeligt, hvad jeg har afprøvet, og hvad der kun er beskrevet.

## Kilder

- [OWASP A07:2025 – Authentication Failures](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)
- [Pluralsight – OWASP: Authentication Failures](https://www.pluralsight.com/courses/owasp-authentication-failures)
- [PortSwigger – Authentication vulnerabilities](https://portswigger.net/web-security/authentication)

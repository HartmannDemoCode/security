# MOOC.fi – Cyber Security Base

**Link:** https://cybersecuritybase.mooc.fi/

Jeg har undersøgt *Cyber Security Base fra University of Helsinki* for at se, hvad der kunne være relevant i vores kommende security-valgfag.

Det interessante for mig er ikke så meget at kopiere deres kursus, men at se **hvilke emner og øvelser der giver mening for en datamatiker**.

En datamatiker på 4. semester har allerede arbejdet med webudvikling, databaser, API'er, HTTP, frontend/backend osv. Derfor synes jeg ikke, at vi skal bruge ret meget tid på at lære de ting igen.

MOOC.fi starter eksempelvis med:

- hvordan webservere fungerer
- request/response
- GET og POST
- HTML/JavaScript
- databaser og SQL
- Django basics
- sessions og cookies

Det er fint i deres kursus, fordi det også skal kunne tages af folk, der ikke nødvendigvis har samme baggrund.

Men for os vil meget af det være repetition.

Det interessante starter efter min mening, når de tager den viden og begynder at kigge på:

> Hvordan kan det her misbruges?

---

## Porte, services og netværk

En af de første security-opgaver er at lave en simpel port scanner i Python.

Man giver den en IP-adresse og et interval af porte, hvorefter programmet prøver at forbinde til dem og finder ud af, hvilke porte der er åbne.

Jeg synes ikke nødvendigvis, at selve Python-programmeringen her er så vigtig.

Det interessante er forståelsen af:

- hvad en åben port betyder
- at der typisk ligger en service bag porten
- hvordan man finder services på en maskine
- hvordan en angriber kan starte med at undersøge et system
- forskellen på bare at bruge et værktøj og forstå, hvad værktøjet gør

Man kunne godt lave en lille øvelse selv først og derefter introducere et rigtigt værktøj som eksempelvis Nmap.

Det passer også godt sammen med vores læringsmål omkring netværk og Linux.

---

# Web security

Det her synes jeg er en af de mest relevante dele af MOOC.fi.

De tager udgangspunkt i OWASP og arbejder blandt andet med:

- SQL Injection
- XSS
- Broken Access Control
- Authentication
- Session vulnerabilities
- CSRF
- Security Misconfiguration
- vulnerable dependencies

Det gode er, at man ikke bare læser om sårbarhederne.

Man arbejder faktisk med dem.

Eksempelvis får man en applikation med en SQL Injection og skal finde ud af, hvordan den kan udnyttes.

Derefter giver det også mening at kigge på, hvordan problemet undgås med eksempelvis parameterized queries eller et ORM.

SQL i sig selv skal vi selvfølgelig ikke lære igen.

Det interessante er forskellen på:

```python
query = "SELECT * FROM users WHERE username = '" + username + "'"
```

og en sikker måde at lave samme operation på.

Altså:

> Hvorfor er den ene løsning farlig, og hvad kan en angriber faktisk gøre med den?

Det samme gælder XSS.

JavaScript og HTML kender vi allerede.

Det interessante er at forstå, hvad der sker, hvis brugerinput bliver lagt direkte ud på siden, og hvordan det kan bruges til eksempelvis at køre JavaScript hos en anden bruger.

---

# Broken Access Control

Jeg synes Broken Access Control er specielt relevant for datamatikere.

Det er nemt at komme til at tænke:

> Brugeren er logget ind, så alt er fint.

Men det er ikke det samme som, at brugeren har adgang til en bestemt resource.

Eksempelvis:

```text
/users/1/document/10
```

Hvad sker der, hvis brugeren ændrer den til:

```text
/users/2/document/10
```

Hvis serveren bare returnerer dokumentet, har vi et problem.

Det er ikke nødvendigvis en avanceret vulnerability.

Det er bare dårlig kode eller manglende checks.

Og det er nok også netop derfor, det er relevant for en datamatiker.

Det er fejl, som vi selv realistisk kan komme til at bygge ind i software.

---

# Sessions og authentication

Vi ved allerede, hvad cookies og sessions er.

Derfor synes jeg ikke, vi skal bruge tid på at lære sessioner fra bunden.

Men MOOC.fi har nogle interessante øvelser, hvor session IDs er dårligt implementeret og kan gættes.

Det gør forskellen mellem:

> Hvad er en session?

og:

> Hvad sker der, hvis session management er dårligt implementeret?

Det sidste er relevant.

Det samme gælder authentication.

Ikke hvordan man laver et login-system fra bunden, men eksempelvis:

- dårlige passwords
- brute force
- session hijacking
- manglende rate limiting
- dårlig password storage
- egne hjemmelavede authentication-løsninger

---

# Angreb for at forstå forsvaret

Noget jeg godt kan lide ved MOOC.fi er, at man nogle gange skal angribe applikationen.

Jeg tror det er vigtigt i et security-fag.

Det er svært rigtigt at forstå eksempelvis SQL Injection, hvis man kun ser:

```text
Husk parameterized queries.
```

Hvis man selv først får SQL Injection til at virke, bliver det meget mere tydeligt, hvorfor det er et problem.

Derfor tror jeg ikke, at valgfaget skal være enten:

**Pentesting**

eller:

**Secure Software Development**

Jeg tror pentesting-delen kan bruges til at lære secure software development.

Altså:

```text
Find problemet
      ↓
Udnyt problemet
      ↓
Forstå hvorfor det virker
      ↓
Ret problemet
      ↓
Test at det faktisk er rettet
```

Det giver efter min mening mere mening for en datamatiker end kun at lære en masse pentesting tools.

---

# Burp Suite

MOOC.fi introducerer også Burp Suite.

Burp kan stå mellem browseren og serveren, så man kan se og ændre HTTP requests.

Selve HTTP-protokollen kender vi allerede.

Det interessante er at kunne tage eksempelvis:

```http
POST /transfer
```

og ændre requesten manuelt.

Det gør det meget tydeligt, at frontend aldrig kan være vores sikkerhedsgrænse.

Hvis frontend eksempelvis har:

```html
<input type="number" min="1" max="100">
```

betyder det ikke, at serveren kun kan modtage værdier mellem 1 og 100.

Requesten kan bare ændres.

Det synes jeg er meget relevant for datamatikere, fordi man bliver tvunget til at se sin egen applikation fra den anden side.

---

# Fuzzing

MOOC.fi går også videre til fuzzing.

Grundideen er at give et program en masse inputs, som udvikleren måske ikke havde forventet, og se hvad der sker.

Eksempelvis:

- ekstremt lange strings
- tomme værdier
- meget store tal
- negative tal
- special characters
- mærkelige formater

Jeg synes ideen er relevant.

Om man behøver gå meget dybt ned i avancerede fuzzers er jeg mere i tvivl om.

For en datamatiker kunne det interessante være princippet:

> Hvad sker der med vores software, når brugeren ikke gør det vi forventer?

Det hænger meget sammen med almindelig software testing, men med en mere fjendtlig tilgang.

---

# Binary exploitation

MOOC.fi går også ind i:

- stack
- heap
- CPU registers
- instruction pointer
- buffer overflow
- memory corruption
- NOP slides

Det er interessant security.

Men jeg er ikke sikker på, hvor relevant det er for vores valgfag.

De fleste datamatikere kommer nok primært til at arbejde med eksempelvis:

- C#
- Java
- JavaScript/TypeScript
- Python
- web frameworks
- databaser
- API'er
- cloud

Derfor synes jeg ikke, vi skal bruge en stor del af et 10 ECTS-fag på binary exploitation.

Det kunne være interessant som en introduktion eller demonstration af, at vulnerabilities også eksisterer længere nede i systemet.

Men jeg ville prioritere web-, API- og application security højere.

---

# Secure Software Development

En anden interessant del af MOOC.fi er, at moderne frameworks allerede beskytter os mod mange klassiske vulnerabilities.

Eksempelvis:

- ORM kan hjælpe mod SQL Injection
- template engines kan hjælpe mod XSS
- frameworks har authentication
- CSRF protection kan være indbygget
- session management er typisk allerede lavet

Men det betyder ikke, at applikationen automatisk er sikker.

Man kan stadig bygge dårlig business logic.

Eksempel:

```text
POST /api/users/42/delete
```

Frameworket kan godt sikre, at requesten kommer fra en authenticated bruger.

Men frameworket ved ikke nødvendigvis:

> Må DENNE bruger slette bruger 42?

Det skal vi stadig selv programmere korrekt.

Det synes jeg er meget relevant for et security-valgfag.

---

# Threat Modeling

En anden del jeg synes kunne være rigtig relevant er Threat Modeling.

Her prøver man at finde security-problemer, før man bare begynder at angribe applikationen.

Man tegner eksempelvis et Data Flow Diagram:

```text
Bruger
  |
  v
Frontend
  |
  v
API
  |
  v
Database
```

Derefter kan man begynde at stille spørgsmål:

- Hvor kommer input fra?
- Hvad kan brugeren selv kontrollere?
- Hvor krydser data en trust boundary?
- Hvem må kalde API'et?
- Hvilke data er følsomme?
- Hvad sker der hvis en request bliver ændret?
- Hvad sker der hvis en bruger prøver at tilgå en anden brugers data?
- Hvilke dependencies stoler vi på?

Det synes jeg passer rigtig godt til en datamatiker, fordi det handler om softwarearkitektur og udvikling, som vi allerede arbejder med.

Man lægger bare security ovenpå.

---

# Projektet på MOOC.fi

MOOC.fi har et projekt, som jeg synes er ret interessant.

Man skal lave en webapplikation med flere bevidste vulnerabilities fra OWASP.

Derefter skal man:

1. beskrive vulnerabilityen
2. vise hvordan den kan udnyttes
3. forklare hvorfor den eksisterer
4. lave en fix
5. dokumentere før og efter

Det kunne være en interessant måde at arbejde på i vores valgfag.

Eksempelvis kunne man få eller bygge en mindre applikation med:

- Broken Access Control
- SQL Injection
- XSS
- dårlig authentication
- en vulnerable dependency

Derefter skal man finde fejlene, demonstrere dem og rette dem.

Det rammer faktisk mange af vores læringsmål på én gang.

---

# Hvad synes jeg ikke er så relevant?

Ud fra MOOC.fi ville jeg ikke bruge ret meget undervisningstid på:

- grundlæggende HTML
- grundlæggende JavaScript
- hvad GET og POST betyder
- hvordan HTTP virker helt grundlæggende
- hvordan man laver routes
- introduktion til SQL
- introduktion til databaser
- grundlæggende ORM
- hvordan man bygger en almindelig webapplikation
- meget dyb binary exploitation

Det første har datamatikere allerede arbejdet med.

Binary exploitation er en anden type problem. Det er interessant, men jeg tror tiden er bedre brugt på security-problemer, som en datamatiker sandsynligvis selv kommer til at skabe, opdage eller skulle løse.

---

# Hvad synes jeg er relevant?

Fra MOOC.fi ville jeg især tage:

- Linux i en security-kontekst
- porte og services
- simple scanning-teknikker
- OWASP vulnerabilities
- Broken Access Control
- XSS
- Injection
- Authentication/session security
- CSRF
- Security Misconfiguration
- vulnerable dependencies
- Burp Suite
- vulnerability analysis
- fuzzing på et grundlæggende niveau
- Threat Modeling
- secure coding
- business logic vulnerabilities
- hvordan man går fra vulnerability til mitigation
- dokumentation af findings

---

# Min foreløbige konklusion

Efter at have kigget på MOOC.fi synes jeg ikke, vores fag skal være et rent pentesting-fag.

Men jeg synes heller ikke, det kun skal være secure coding.

Jeg tror en kombination giver mest mening.

For mig kunne den røde tråd være:

> **Lær at finde og udnytte fejl, så du bliver bedre til at bygge software uden dem.**

Det passer også bedre på en datamatiker.

Vi skal ikke nødvendigvis uddanne penetration testers.

Men en datamatiker bør kunne kigge på en applikation og tænke:

> Hvordan ville jeg selv prøve at bryde det her?

Og derefter:

> Hvordan designer og implementerer jeg det, så det ikke kan lade sig gøre?

Det synes jeg indtil videre er den mest interessante retning for valgfaget.

---

# Videre undersøgelse – Advanced Topics / Cyber Security II

MOOC.fi har også mere avanceret materiale under **Advanced Topics / Cyber Security II**.

Jeg har **ikke undersøgt det nærmere endnu**, men jeg har fundet ud af, at der blandt andet findes materiale omkring:

- Threat Analysis / Threat Modeling
- Architectural security
- Cryptography
- Certificates og Public Key Infrastructure
- Internet security
- Network security
- Penetration testing
- Intrusion prevention
- CTF
- IoT security
- 4G/5G security

Det kan være relevant at undersøge senere, fordi det muligvis kan supplere *Securing Software*-delen.

Især emner som:

- Threat Modeling
- netværkssikkerhed
- penetration testing
- praktisk kryptografi

kunne være interessante i forhold til vores valgfag.

Andre emner som eksempelvis **4G/5G, IoT og dybere cryptanalysis** er jeg mere i tvivl om er relevante for en generel datamatiker.

Det skal derfor undersøges nærmere senere.

**Links til materialet:**

- https://cybersecuritybase.mooc.fi/descriptions/
- https://cybersecuritybase.mooc.fi/module-4.1/
- https://cybersecuritybase.mooc.fi/module-4.3/
- https://cybersecuritybase.mooc.fi/module-4.5/
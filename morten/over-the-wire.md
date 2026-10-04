# OverTheWire – Bandit

**Link:** https://overthewire.org/wargames/bandit/

Jeg har kigget på **OverTheWire Bandit** for at se, hvad der kunne være relevant i et security-valgfag.

Bandit er en slags **wargame / CTF**, hvor man forbinder til en Linux-server gennem SSH og løser en række mindre opgaver.

Hver opgave giver adgang til den næste, og man skal selv finde ud af, hvilke Linux-kommandoer eller værktøjer der kan bruges til at løse problemet.

Det jeg synes er godt ved Bandit er, at man ikke bare får forklaret en kommando og bagefter laver en øvelse med den.

Man får i stedet et problem og skal selv finde ud af:

> Hvordan undersøger jeg det her system, og hvilke værktøjer kan hjælpe mig?

Det gør, at man bliver tvunget til at eksperimentere, læse dokumentation og selv finde frem til en løsning.

---

# Linux og terminalen

En stor del af Bandit handler om at blive komfortabel med Linux og terminalen.

Der arbejdes blandt andet med:

- SSH
- filer og mapper
- skjulte filer
- filnavne
- permissions
- users og groups
- pipes
- redirects
- environment og shell

Der bruges kommandoer som blandt andet:

```bash
ls
cd
cat
file
find
grep
sort
uniq
strings
```

Det gode er, at kommandoerne bliver brugt til at løse konkrete problemer.

Man kan eksempelvis skulle finde en fil, som:

- har en bestemt størrelse
- tilhører en bestemt bruger
- tilhører en bestemt gruppe
- ligger et ukendt sted
- indeholder bestemte data

På den måde bliver Linux ikke bare noget teori, men et værktøj man bruger til at undersøge et system.

Det passer godt til security, fordi rigtig mange security-værktøjer og miljøer arbejder ud fra Linux.

---

# Arbejde med ukendt data

Bandit indeholder også opgaver, hvor man skal undersøge forskellige former for data.

Her bruges blandt andet:

- `grep`
- `strings`
- `sort`
- `uniq`
- `base64`
- `tr`
- `xxd`
- `gzip`
- `bzip2`
- `tar`

Data kan eksempelvis være:

- encoded
- komprimeret
- gemt som hex
- blandet med store mængder andet data

Det interessante er ikke nødvendigvis den enkelte kommando.

Det interessante er arbejdsmetoden:

> Hvad er det her for noget data, og hvordan kan jeg finde ud af, hvad der gemmer sig i det?

Det synes jeg er en god security-kompetence.

---

# SSH og authentication

Bandit bruger SSH meget.

Det betyder, at man får praktisk erfaring med:

- remote login
- brugere
- passwords
- SSH keys
- private keys
- permissions på keys

På nogle opgaver får man eksempelvis ikke et normalt password, men en private SSH key, som skal bruges til at logge ind.

Det gør SSH authentication mere konkret.

SSH keys er også noget, som er relevant for udviklere i forbindelse med blandt andet:

- Linux-servere
- deployment
- Git
- cloud
- CI/CD

---

# Netværk, porte og services

Senere bliver opgaverne mere fokuseret på netværk og services.

Der arbejdes blandt andet med:

- localhost
- porte
- services
- Netcat
- OpenSSL
- TLS
- port scanning

Man kan eksempelvis skulle finde ud af, hvilken service der lytter på en bestemt port eller finde en åben port inden for et interval.

Det gode er, at man ikke kun lærer:

> En server har åbne porte.

Man skal selv undersøge systemet og finde ud af, hvad der rent faktisk kører.

Det giver en mere praktisk forståelse af services og netværk.

---

# Permissions og privileges

Bandit kommer også ind på Linux permissions og privileges.

Her bliver begreber som:

- users
- groups
- file permissions
- executables
- setuid

mere konkrete.

Det interessante i en security-sammenhæng er ikke bare at kunne læse:

```text
rwx
```

men at forstå:

> Hvilken bruger kører dette program som?

og:

> Hvilke rettigheder får programmet adgang til?

Det er vigtigt, når man skal forstå både system security og privilege escalation.

---

# Cron og scripts

Der findes også opgaver med:

- cron jobs
- shell scripts
- automatiske jobs
- processer der bliver kørt med bestemte intervaller

Her skal man blandt andet undersøge scripts og finde ud af, hvad systemet automatisk udfører.

Det synes jeg er interessant, fordi man lærer at undersøge et system frem for kun at bruge færdige security-værktøjer.

Man skal læse scripts, følge hvad de gør og forstå, hvilke filer og brugere de arbejder med.

---

# Brute force

Bandit introducerer også et simpelt eksempel på brute force.

I stedet for at kende den rigtige værdi skal man systematisk prøve forskellige muligheder.

Det demonstrerer meget godt princippet bag brute force-angreb.

Det kan samtidig bruges til at tænke over defensive løsninger som:

- rate limiting
- lockout
- delays
- MFA

Det gode ved sådan en opgave er, at man først ser problemet praktisk og derefter kan begynde at tænke over, hvordan man ville beskytte imod det.

---

# Git fra en security-vinkel

Bandit indeholder også opgaver omkring Git.

Git kender en datamatiker allerede, så det interessante er ikke de normale Git-kommandoer.

Det interessante er i stedet at undersøge:

- commit history
- branches
- tags
- tidligere versioner
- information der tidligere har været committed

Det kan eksempelvis vise, hvorfor det kan være et problem at committe:

- passwords
- API keys
- tokens
- andre secrets

Det er ikke nødvendigvis nok bare at slette secret'en i næste commit, fordi den stadig kan eksistere i Git-historikken.

Det synes jeg er et rigtig relevant eksempel på security i almindelig softwareudvikling.

---

# CTF og problemløsning

Noget af det bedste ved Bandit synes jeg egentlig er selve måden, man arbejder på.

Man får ikke altid at vide præcist, hvilken kommando man skal bruge.

Man får et mål og må selv finde vejen derhen.

Arbejdsformen bliver derfor noget i retning af:

```text
Forstå problemet
      ↓
Undersøg systemet
      ↓
Find et muligt værktøj
      ↓
Læs dokumentationen
      ↓
Prøv det
      ↓
Se hvad der sker
      ↓
Tilpas løsningen
```

Det synes jeg passer rigtig godt til security.

Man kommer hele tiden til at møde ting, man ikke har set før, så det er vigtigt at kunne undersøge et problem selv.

---

# Hvad synes jeg især er godt ved Bandit?

De ting jeg især synes virker relevante er:

- Linux i en praktisk security-kontekst
- SSH
- users, groups og permissions
- Linux-værktøjer
- pipes og shell
- arbejde med ukendt data
- SSH keys
- porte og services
- Netcat
- OpenSSL/TLS
- port scanning
- setuid og privileges
- cron jobs
- simple scripts
- brute force som koncept
- Git fra en security-vinkel
- selvstændig problemløsning

Bandit giver efter min mening en god måde at lære grundlæggende Linux og security på, fordi man hele tiden bruger tingene praktisk.

Det er ikke kun:

> Lær denne kommando.

Det er mere:

> Her er et problem. Find ud af hvordan Linux kan bruges til at løse det.

---

# Min foreløbige vurdering

Jeg synes Bandit er en god ressource til især **Linux-delen af et security-valgfag**.

Det giver praktisk erfaring med terminalen samtidig med, at opgaverne langsomt bevæger sig over i mere security-relaterede områder som permissions, services, authentication og netværk.

Det jeg bedst kan lide ved Bandit er, at man bliver tvunget til selv at undersøge problemer.

Man får ikke altid løsningen serveret.

Man skal selv:

- læse
- prøve
- fejle
- undersøge
- forstå
- prøve igen

Det er efter min mening en vigtig del af at arbejde med security.
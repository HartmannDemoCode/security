# Hashcat og Hydra — login og JWT-secret

Opskrifter til egne lokale testmiljøer. Kommandoerne er til Bash på Linux/Kali/WSL med værktøjerne installeret. De er testforslag, ikke dokumentation for udførte forsøg. Jeg noterer værktøjsversion og testforudsætninger sammen med resultatet.

## Vælg den rigtige test

| Mit spørgsmål | Værktøj og fremgangsmåde | Betydning af rate limiting |
| --- | --- | --- |
| Kan et svagt JWT-secret gættes ud fra et signeret HS256-token? | Hashcat afprøver kandidater lokalt og sammenligner signaturer. | API'ets rate limiting begrænser ikke denne offline-test. |
| Tillader login-endpointet gentagne passwordforsøg? | Hydra sender faktiske loginforsøg med en kendt testbruger og en lille passwordliste. | Serverens begrænsning af loginforsøg er det, jeg undersøger. |

Brugerens loginpassword og serverens JWT-signeringssecret er to forskellige hemmeligheder. Se [PortSwigger om offline-gætning af JWT-secrets](https://portswigger.net/web-security/jwt#brute-forcing-secret-keys) og [Hydras officielle projekt](https://github.com/vanhauser-thc/thc-hydra).

## Hashcat: afprøv kandidater til et HS256-secret

### 1. Kontrollér forudsætningerne

Jeg finder et originalt signeret token fra mit testmiljø og undersøger algoritmen. Denne opskrift er til **HS256**, som bruger et delt secret. Et **RS256**-token bruger et privat/offentligt nøglepar og kan ikke undersøges med denne opskrift som et HMAC-secret. Headeren er et spor; serverens konfiguration bestemmer den tilladte algoritme.

Tokenet gemmes på én linje i `jwt.txt`, uden `Bearer `, anførselstegn eller ekstra mellemrum. Hele tokenet bruges: `header.payload.signature`. Hashcats mode og inputformat fremgår af de [officielle eksempler](https://hashcat.net/wiki/doku.php?id=example_hashes).

### 2. Start med et kendt øvelseseksempel

Følgende er Hashcats offentlige HS256-eksempel med det bevidst svage secret `hashcat`. Det bruges til at kontrollere, at mit værktøj virker, før jeg drager slutninger om min egen app.

```bash
mkdir -p jwt-lab
cd jwt-lab

cat > jwt.txt <<'EOF'
eyJhbGciOiJIUzI1NiJ9.eyIzNDM2MzQyMCI6NTc2ODc1NDd9.f1nXZ3V_Hrr6ee-AFCTLaHRnrkiKmio2t3JqwL32guY
EOF

printf '%s\n' 'forkert-lab-secret' 'hashcat' 'andet-lab-secret' > secrets.txt

hashcat -m 16500 -a 0 jwt.txt secrets.txt --potfile-path jwt-lab.pot
hashcat -m 16500 jwt.txt --show --potfile-path jwt-lab.pot
```

`-m 16500` vælger JWT/HS256, og `-a 0` afprøver en ordliste. `--show` viser tidligere fund fra den valgte potfile. Potfilen gemmer genfundne hemmeligheder og deles ikke som almindelig rapportbilag. Se [Hashcats kommandoreference](https://hashcat.net/wiki/doku.php?id=hashcat).

Hvis Hashcat ikke kan starte sin beregningsbackend, undersøger jeg installationen med `hashcat -I`. En driver- eller inputfejl er en blokeret test, ikke et tegn på et stærkt secret.

### 3. Brug samme fremgangsmåde på mit eget testtoken

Jeg gemmer mit eget originale HS256-token i en separat fil, eksempelvis `jwt-egen-app.txt`, og vælger en begrundet liste af kandidater til øvelsen:

```bash
hashcat -m 16500 -a 0 jwt-egen-app.txt secrets.txt \
  --potfile-path egen-app.pot --runtime 60

hashcat -m 16500 jwt-egen-app.txt --show --potfile-path egen-app.pot
```

Jeg noterer kandidatlistens indhold/omfang, køretid og slutstatus. **Cracked** betyder et match. **Exhausted** betyder, at de genererede kandidater er afprøvet uden match. Et stop efter tidsgrænsen kan efterlade kandidater uprøvede. Ingen af de to sidste resultater beviser i sig selv, at secretet er stærkt.

### Hvis secretet er svært

Hashcat forsøger kandidater; det dekrypterer ikke automatisk et vilkårligt secret. En ordliste hjælper ved eksempelvis standardværdier og menneskevalgte ord. Et langt, kryptografisk tilfældigt secret er i praksis urealistisk at finde ved udtømmende gætning.

I en lokal øvelse kan jeg vælge et kendt begrænset mønster, fx `lab` efterfulgt af fire cifre, og afprøve netop de 10.000 kandidater:

```bash
hashcat -m 16500 -a 3 jwt-egen-app.txt 'lab?d?d?d?d' \
  --potfile-path egen-app.pot --runtime 60
```

Dette giver kun mening, hvis øvelsens secret følger det mønster. Jeg skal ikke forvente, at en stadig større maskesøgning gør et stærkt tilfældigt secret realistisk at finde. I min egen kode undersøger jeg også, hvordan nøglen genereres, indlæses og opbevares.

### Rettelse og gentest

Ved et påvist svagt HS256-secret skifter jeg til en kryptografisk tilfældig nøgle på mindst 256 bit, opbevaret uden for kildekoden. Jeg kontrollerer, hvordan applikationen fortolker nøgleværdien, eksempelvis rå tekst eller base64. Nøglekravet følger [RFC 7518, afsnit 3.2](https://www.rfc-editor.org/rfc/rfc7518#section-3.2).

Jeg dokumenterer konfigurations-/kodeændringen og kontrollerer, at et gammelt testtoken afvises, når den gamle nøgle er fjernet fra valideringen, mens et nyt token fra et normalt login accepteres. Jeg gentager kandidatforsøget mod det nye token. Det dokumenterer virkningen af den valgte ændring; ordlisten alene beviser ikke nøglens styrke.

## Hydra: afgrænset test af login og rate limiting

### 1. Bekræft loginformat og succeskriterium

Jeg opretter en disponibel lokal testkonto og afprøver manuelt ét korrekt og ét forkert password. I Burp noterer jeg metode, sti, Content-Type, body og forskellen på succes og fejl.

Jeg vælger en konkret forsøgsgrænse, fx **fem passwordkandidater mod én konto**, og stopper ved spærring, rate limiting eller uventede fejl. Det lille forsøg undersøger adfærden ved den valgte belastning.

```bash
hydra -h
hydra -U http-post-form
```

Jeg følger hjælpen til min installerede version. Optionsrækkefølge kan variere; nedenstående har moduloptioner før det afsluttende succeskriterium, som beskrevet i [Hydras aktuelle HTTP-form-modul](https://github.com/vanhauser-thc/thc-hydra/blob/master/hydra-http-form.c).

### 2. Eksempel til et lokalt JSON-login

Dette eksempel forudsætter en lokal app på `127.0.0.1:7000`, som modtager `email` og `password` på `/api/v1/auth/login`. Jeg tilpasser det til den request, jeg faktisk har set. Et succesfuldt svar skal indeholde JSON-feltnavnet `"token"`, og fejlsvar må ikke indeholde samme markør.

Jeg sætter på forhånd den lokale testkontos password til `LabPassword2026`. Alle fem kandidater bruger enkle tegn, så formmodulets encoding ikke ændrer deres betydning i JSON.

```bash
printf '%s\n' 'WrongLab01' 'WrongLab02' 'WrongLab03' \
  'WrongLab04' 'LabPassword2026' > passwords-test.txt

hydra -l 'test@example.test' -P passwords-test.txt \
  -s 7000 -t 1 -W 2 -f \
  127.0.0.1 http-post-form \
  '/api/v1/auth/login:{"email"\:"^USER^","password"\:"^PASS^"}:H=Content-Type\: application/json:1=:S="token"'
```

`-l` vælger én konto, `-P` kandidatfilen, `-t 1` én samtidig opgave, `-W 2` ventetid mellem forbindelser og `-f` stop ved første fund. `\:` beskytter koloner inde i JSON og headers mod Hydras optionsparser. `H=` sætter Content-Type, `1=` behandler 401 som mislykket login, og `S=` er min på forhånd kontrollerede succesmarkør.

Jeg bruger en positiv succesmarkør, så et uventet fejlsvar ikke automatisk bliver bedømt som korrekt login. Jeg kontrollerer en meldt succes manuelt. CAPTCHA, CSRF-tokens, specialtegn og andre loginflows kræver tilpasning; jeg kontrollerer den faktisk afsendte request før brug. Flagene er beskrevet i [Hydras kommandolinjehjælp](https://github.com/vanhauser-thc/thc-hydra/blob/master/hydra.c).

### 3. Dokumentér rate limiting særskilt

Jeg undersøger API-svar, tidsforbrug og serverlogs for antallet af loginforsøg, forsinkelse, eventuel spærring og en dokumenteret reaktion som HTTP 429. At finde et kendt password dokumenterer loginforsøget; det fastslår ikke alene, at alle former for rate limiting mangler.

Hvis alle fem forsøg behandles normalt, beskriver jeg præcis dét. En højere grænse kan stadig eksistere. Jeg sammenholder med et fastlagt krav, fx højst tre fejlslagne forsøg pr. konto inden for 30 sekunder, og undersøger den relevante konfiguration/kode. Rate limiting kan ligge i applikationen eller foran den; jeg noterer, hvilke lag mit lokale forsøg omfatter.

### Rettelse og gentest

Jeg implementerer en begrundet begrænsning af loginforsøg, fx pr. konto suppleret med kildeadresse, og vurderer risikoen for at låse legitime brugere ude. Se [OWASP om login throttling](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html#login-throttling).

Jeg gentager forsøget med samme timing og genskabte forudsætninger. Jeg viser, hvornår begrænsningen træder i kraft, og kontrollerer, at tilladt login fungerer efter den aftalte ventetid. Efter ændringen kan Hydra ikke stå alene som måling: logs og konkrete svar skal vise, at beskyttelsen udløses.

## Det jeg gemmer som evidens

- Test-ID, dato, versioner og lokalt miljø.
- For Hashcat: algoritme, kandidatgrundlag, tidsgrænse og slutstatus; hemmeligheder maskeres i delt materiale.
- For Hydra: testkonto, kandidatantal, timing, succesmarkør og faktiske API-svar/logs.
- Den relevante kode eller opsætning før/efter og resultaterne af gentest og almindelig brug.

Jeg bruger den fælles [testnoteskabelon](../fremgangsmaade.md#7-skriv-en-kort-testnote). Forsøgene bliver først fund i den undersøgte applikation, når jeg faktisk har kørt og dokumenteret dem i det valgte miljø.

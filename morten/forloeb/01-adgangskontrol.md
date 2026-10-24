# Login, sessioner og adgangskontrol

Status: Ikke startet.

OWASP Top 10:2025: A01 Broken Access Control og A07 Authentication Failures.

Jeg arbejder i rækkefølgen her og samler op til vores møder. I [ugeplanen](ugeplan.md) vælger jeg, hvad jeg tager fat på nu. Denne uge undersøger jeg også afhængigheder og Dependabot, så jeg behøver ikke nå hele dette forløb på én gang.

## Det jeg vil finde ud af

Hvordan kan fejl i login, sessioner og adgangskontrol give adgang til en konto eller data, man ikke burde have adgang til? Kan vi bruge samme applikation til at undersøge begge kategorier og vise rettelserne i Java?

Jeg har styr på authorization i forvejen. Jeg vil bruge tiden på fejlene, konsekvenserne og den kode, der skal forhindre dem.

Jeg holder forskellen tydelig: A07 handler her om, hvorvidt systemet kan stole på brugerens identitet og loginforløb. A01 handler om, hvad den bruger må få adgang til. En bruger kan godt være korrekt logget ind og stadig kunne hente en anden brugers dokument.

Jeg skriver vurderinger af materialet i [ressourceundersøgelsen](../ressourcer/adgangskontrol.md). Forklaringer, requests, kode og testresultater samler jeg i [mine faglige noter](../emner/adgangskontrol.md).

## Læse om fejlene

- [ ] Læse Description, How to prevent og Example attack scenarios i OWASP A01:2025.
- [ ] Læse de samme afsnit i OWASP A07:2025 med fokus på loginforløb og sessioner.
- [ ] Læse PortSwiggers eksempler på adgangskontrolfejl og siden om IDOR.
- [ ] Undersøge adgang til andre brugeres data og adgang til funktioner for andre roller.
- [ ] Undersøge, hvordan et manglende trin i et loginforløb kan give adgang, før brugeren er færdig med at logge ind.
- [ ] Læse de relevante dele af OWASP Authorization Cheat Sheet om kontrol af rettigheder på serveren.
- [ ] Skrive med egne ord, hvordan fejlene kan opstå i en applikation, vi selv kunne have bygget.

Jeg vil have en forklaring af fejlene og nogle spørgsmål, jeg kan prøve af i øvelserne.

## Undersøge materialerne

- [ ] Se de relevante dele af Pluralsight-kurset om IDOR og forced browsing.
- [ ] Undersøge WebGoats opsætning og finde en relevant øvelse om adgangskontrol.
- [ ] Sammenholde forklaringerne fra OWASP, PortSwigger og Pluralsight.
- [ ] Notere, hvad der kræver forhåndsviden, og hvor en studerende kunne få brug for hjælp.

Her vurderer jeg materialerne. Selve løsningen af øvelserne hører til næste del. Jeg skriver tydeligt, hvad jeg har læst, set og selv afprøvet.

## Afprøve øvelserne

- [ ] Løse PortSwigger-labben User ID controlled by request parameter.
- [ ] Løse PortSwigger-labben 2FA simple bypass og forklare, hvad der mangler i kontrollen af loginforløbet.
- [ ] Gemme relevante requests og resultater, så jeg kan vise fejlene igen.
- [ ] Forklare, hvilken kontrol der mangler, og hvad konsekvensen er.
- [ ] Notere tidsforbrug, problemer med opsætning og den hjælp, jeg bruger.

Jeg starter med de to PortSwigger-øvelser. Derefter kan jeg vælge en ekstra variant: en rolleoplysning, brugeren kan ændre, et forløb med manglende kontrol på ét trin eller en IDOR-øvelse i WebGoat eller TryHackMe. Jeg behøver ikke prøve alle platformene.

Jeg bruger mit lokale øvelsesmiljø eller platformens labs til afprøvningen.

## Lave og teste en rettelse i Java

- [ ] Lave et lille eksempel med to brugere og dokumenter, der tilhører hver sin bruger.
- [ ] Vise fejlen, hvor et ændret dokument-ID giver adgang til en anden brugers dokument.
- [ ] Rette kontrollen på serveren, så den tager højde for, hvem der ejer dokumentet.
- [ ] Teste, at ejeren har adgang, mens en anden bruger bliver afvist.
- [ ] Teste, at en request uden en gyldig bruger bliver afvist.
- [ ] Gennemgå login og sessioner i det valgte framework og udvælge en relevant kontrol, jeg kan afprøve i samme eksempel.
- [ ] Forklare rettelsen til 2FA-labben uden at bygge et helt nyt login-system.
- [ ] Gemme resultatet før og efter rettelsen.

Jeg holder eksemplet lille og bruger en kendt Java-stack. Jeg behøver ikke en frontend eller et nyt login-system. Hvis noget kun bliver en skitse, skriver jeg, at det ikke er afprøvet.

## Samle fund og vurdere materialerne

- [ ] Skrive en kort sårbarhedsanalyse med fejl, fremgangsmåde, konsekvens, rettelse og testresultat.
- [ ] Vurdere materialernes forklaringer, niveau, opsætning og øvelser ud fra det, jeg selv har prøvet.
- [ ] Vælge det materiale, jeg ville bruge i undervisningen, og begrunde valget.
- [ ] Beskrive, hvilken forhåndsviden og hjælp øvelsen kræver.
- [ ] Knytte mine fund til læringsmålene om sårbarhedsanalyse, forebyggelse, sikker kode og dokumentation.

Analysen ligger i de faglige noter. Vurderingen af materialerne ligger i ressourceundersøgelsen.

## Møde og opsamling

- [ ] Forberede et kort eksempel, jeg kan vise og forklare til mødet.
- [ ] Tage min vurdering af materialerne og mine åbne spørgsmål med.
- [ ] Drøfte, om arbejdsformen passer til valgfaget.
- [ ] Skrive, hvad vi blev enige om, og vælge næste undersøgelse.

## Det jeg vil stå med bagefter

- En dokumenteret adgangskontrolfejl og en afprøvet rettelse, eller en tydelig beskrivelse af det, der mangler.
- Et eksempel på en fejl i loginforløbet og en forklaring af, hvordan den adskiller sig fra adgangskontrolfejlen.
- En vurdering af materialerne baseret på min egen læsning og afprøvning.
- Et forslag til en øvelse i valgfaget med en begrundelse ud fra læringsmålene.

## Opsamling fra møder

Dato:

Hvor langt er jeg:

Det vi blev enige om:

Det jeg arbejder videre med:

## OWASP-kapitler

- [A01:2025 – Broken Access Control](https://top10.owasp.org/2025/A01_2025-Broken_Access_Control/)
- [A07:2025 – Authentication Failures](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/)

Øvelser og supplerende materiale ligger i [ressourceundersøgelsen](../ressourcer/adgangskontrol.md).

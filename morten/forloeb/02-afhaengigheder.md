# Afhængigheder, opdateringer og sikker opsætning

Status: Ikke startet.

OWASP Top 10:2025: A03 Software Supply Chain Failures og A02 Security Misconfiguration.

## Problemet

Et typisk forløb kan være:

1. Mit projekt bruger et bibliotek med en sikkerhedsfejl.
2. Fejlen bliver offentliggjort, og bibliotekets udviklere udgiver en rettelse.
3. Mit projekt fortsætter med den sårbare version, fordi opdateringen bliver overset.
4. En angriber udnytter fejlen gennem min applikation.

Min egen kode kan godt være fornuftig, mens et bibliotek introducerer en sårbarhed. Det gælder også indirekte afhængigheder: biblioteker, som mine biblioteker bruger.

Et eksempel kunne være en applikation på en droplet, som bruger et JOSE/JWT-bibliotek. Applikationen virker stadig, og derfor bliver den stående med samme biblioteksversion i et år. Hvis der i mellemtiden kommer en relevant sikkerhedsrettelse, hjælper den først, når den opdaterede applikation er sat i drift. Eksemplet er tænkt; en gammel version er ikke i sig selv dokumentation for en sårbarhed.

Derfor vil jeg undersøge hele arbejdsformen:

Find afhængigheder → få advarsler → vurder opdateringen → test → sæt rettelsen i drift → kontrollér den kørende version.

Dependabot kan hjælpe med advarsler og opdateringsforslag. Jeg skal også have en rutine for at følge op, så en PR ikke bare bliver liggende, og serveren ikke fortsætter med det gamle build.

Jeg vil undersøge opsætningen i samme case. Et projekt kan have opdaterede biblioteker og stadig afsløre interne detaljer gennem fejlbeskeder eller have unødvendige services åbne. Derfor samler jeg A03 om komponenter og opdateringer med A02 om sikker konfiguration. Jeg dokumenterer, hvilken af de to fejl jeg viser.

## Det jeg vil finde ud af

Hvordan holder man biblioteker og resten af sin applikations stack opdateret? Hvad kan Dependabot hjælpe med, og hvad skal man stadig selv tage ansvar for?

Hvordan sikrer jeg, at den opdaterede applikation også bliver startet med den rigtige konfiguration?

Hvordan opdager jeg nye stabile releases uden at skulle huske at besøge alle bibliotekernes hjemmesider? Hvordan vælger jeg biblioteker, jeg har grund til at stole på, og hvordan følger jeg deres vedligeholdelse bagefter?

Jeg bruger PHP2Shell som case og et kendt Java-projekt til den praktiske del. Jeg behøver ikke bygge en PHP-applikation for at undersøge arbejdsformen.

Jeg vil også undersøge, om en praktisk øvelse kan give en bedre forståelse af problemet: først vise, hvordan en sårbar afhængighed kan misbruges, og derefter prøve samme angreb efter en rettelse. Det kunne være en øvelse, andre studerende også får noget ud af.

Vurderinger af materialet ligger under [ressourcer](../ressourcer/afhaengigheder.md). Forklaringer og resultater fra afprøvninger ligger under [emner](../emner/afhaengigheder.md). Prioriteringen for denne uge ligger i [ugeplanen](ugeplan.md).

## Læse om problemet

- [ ] Læse OWASP A03:2025 om Software Supply Chain Failures.
- [ ] Læse OWASP A02:2025 om Security Misconfiguration.
- [ ] Læse PHP Foundations forklaring af PHP2Shell og PHP-projektets sikkerhedsmeddelelse.
- [ ] Skelne mellem en fejl i et bibliotek, en fejl i runtime'en og en pakke, der bevidst er blevet ændret af en angriber.
- [ ] Undersøge direkte og indirekte afhængigheder og hvorfor begge er relevante.

## Undersøge Dependabot

- [ ] Forklare forskellen på alerts, security updates og version updates ud fra GitHubs dokumentation.
- [ ] Undersøge dependency graph og GitHub Advisory Database som grundlag for advarslerne.
- [ ] Undersøge, hvor funktionerne aktiveres, og hvad der konfigureres i `.github/dependabot.yml`.
- [ ] Undersøge understøttelsen af Maven, Composer, Docker og GitHub Actions, og hvilke filer der skal være i repoet.
- [ ] Skrive et udkast til konfiguration for det valgte Java-projekt med passende mapper og opdateringsinterval.
- [ ] Undersøge, hvem der får besked om opdateringer og sikkerhedsadvarsler, og hvordan jeg undgår, at de bliver overset.

Konfigurationen er et udkast i mine noter. Jeg noterer, hvad der først kan afprøves, når den er taget i brug på GitHub.

## Vælge biblioteker og holde fast i vedligeholdelsen

- [ ] Undersøge bibliotekets officielle kilde, vedligeholdelse, support og måde at offentliggøre sikkerhedsrettelser på.
- [ ] Se på udgivelsesnoter og kompatibilitet frem for kun at vælge det nyeste versionsnummer.
- [ ] Foreslå en fast rutine for gennemgang af almindelige opdaterings-PR'er og en særskilt opfølgning på relevante sikkerhedsadvarsler.
- [ ] Beskrive, hvem der har ansvar for at vurdere, teste og sætte opdateringer i drift, også efter at projektet er afleveret.

Jeg vil bruge vedligeholdte biblioteker fra kendte kilder. Tillid til biblioteket skal følges op med opdateringer og kontrol; en stabil release er ikke en garanti for, at der aldrig bliver fundet fejl.

## Finde en øvelse, hvor fejlen kan udnyttes

- [ ] Undersøge WebGoats Vulnerable Components som mulig Java-øvelse.
- [ ] Bruge Snyks Log4Shell-gennemgang til at forstå, hvordan et almindeligt logging-bibliotek kan få betydning for hele applikationens sikkerhed.
- [ ] Vælge én øvelse med en kendt sårbarhed og dokumenterede betingelser for, at den virker.
- [ ] Prøve øvelsen lokalt og gemme et konkret resultat, der viser konsekvensen.
- [ ] Kontrollere, om rettelsen er en biblioteksopdatering, en ændring i kode eller konfiguration eller en kombination.

Jeg vil bruge et lokalt øvelsesmiljø til denne del. Min droplet er eksemplet på vedligeholdelse og deployment; jeg lægger ikke den bevidst sårbare øvelse derud.

En opgave, hvor man kun finder et gammelt versionsnummer, viser ikke i sig selv, hvordan fejlen kan udnyttes. Jeg vil finde ud af, om den valgte øvelse faktisk kan bruges til en demonstration før og efter rettelsen. Hvis ikke, skriver jeg det i vurderingen af materialet.

## Afprøve rettelsen

- [ ] Finde de direkte og indirekte afhængigheder i øvelsesprojektet. Bruge et kendt Java-projekt som supplement, hvis øvelsen ikke giver adgang til de nødvendige filer.
- [ ] Vælge en relevant opdatering og kontrollere udgivelsesnoter, kompatibilitet og eventuelle advisories.
- [ ] Notere nuværende version, ny version og begrundelsen for opdateringen.
- [ ] Afprøve opdateringen lokalt og køre projektets relevante tests og build.
- [ ] Gentage angrebet fra øvelsen og kontrollere, om det stadig virker.
- [ ] Kontrollere, at den almindelige funktion stadig virker, så en stoppet applikation ikke bliver forvekslet med en rettet sårbarhed.
- [ ] Beskrive resultatet og eventuelle ændringer, der var nødvendige.

Jeg bruger så vidt muligt samme øvelsesprojekt til at vise fejlen og teste rettelsen. Hvis jeg kun får afprøvet en almindelig versionsopdatering, kalder jeg den ikke et sikkerhedsfix uden dokumentation.

## Undersøge opsætningen og resten af stacken

- [ ] Vælge én fejlkonfiguration i det lokale øvelsesmiljø, eksempelvis detaljerede fejlbeskeder, der afslører interne oplysninger.
- [ ] Vise konsekvensen, rette konfigurationen og gentage samme request.
- [ ] Forklare forskellen på at rette en sårbar biblioteksversion og at rette en usikker indstilling.

- [ ] Finde ud af, hvor projektets Java- eller PHP-runtime kommer fra: hosting, OS-pakke eller container-image.
- [ ] Undersøge, hvem der har ansvar for at opdatere den, og hvor sikkerhedsmeddelelserne findes.
- [ ] Beskrive forskellen på at få en opdaterings-PR, bygge en ny container og få den nye version sat i drift.
- [ ] Gennemgå droplet-eksemplet: Hvad skal bygges igen, hvordan bliver det sat i drift, og skal processen eller containeren genstartes eller erstattes?
- [ ] Undersøge, hvordan man kontrollerer, hvilken version der faktisk kører bagefter.
- [ ] Forklare, hvorfor en nyere version ikke i sig selv garanterer, at en pakke er sikker.

## Vurdere det til valgfaget

- [ ] Foreslå en øvelse med en afhængighed, en opdatering, test og dokumentation.
- [ ] Lade samme case indeholde en fejlkonfiguration, så man både undersøger versioner og den måde, applikationen kører på.
- [ ] Vurdere, om man bliver bedre til at forstå problemet ved selv at se angrebet virke og derefter stoppe det med en rettelse.
- [ ] Beskrive, hvilke dele der kan laves lokalt, og hvilke der kræver adgang til et GitHub-repo.
- [ ] Knytte øvelsen til læringsmålene om sårbarhedsanalyse, forebyggelse og sikker udvikling.
- [ ] Notere, hvad Dependabot ikke har vist eller afprøvet i min undersøgelse.

## Kilder til problemstillingen

- [OWASP A03:2025](https://top10.owasp.org/2025/A03_2025-Software_Supply_Chain_Failures/)
- [OWASP A02:2025](https://top10.owasp.org/2025/A02_2025-Security_Misconfiguration/)
- [Dependabot version updates](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-version-updates)
- [Dependabot security updates](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-security-updates)
- [WebGoat: Vulnerable Components](https://github.com/WebGoat/WebGoat/tree/main/src/main/java/org/owasp/webgoat/lessons/vulnerablecomponents)
- [Snyk Learn: Log4Shell](https://learn.snyk.io/lesson/log4shell/)

# Oplæg til problemformulering

**Emne:** Sårbarhedsanalyse og sikkerhedsforbedringer i miseOS med udgangspunkt i OWASP Top 10:2025  


**Foreløbigt rapportfokus til drøftelse** A01:2025 Broken Access Control og A07:2025 Authentication Failures. Jeg vil fortsat undersøge alle ti kategorier som del af mit læringsforløb. Valget til rapporten er et forslag, som jeg vil kvalificere gennem undersøgelserne.

## Problemstilling

Jeg udviklede miseOS som mit eksamensprojekt på 3. semester ud fra den viden og de færdigheder, jeg havde på daværende tidspunkt. Applikationen indeholder blandt andet login og forskellige brugerroller, som skal beskytte adgangen til systemets data og funktioner.

Med den sikkerhedsfaglige viden, jeg tilegner mig i valgfaget Cybersecurity, vil jeg undersøge, hvor godt disse sikkerhedsforanstaltninger fungerer, og om der er svagheder, jeg ikke var opmærksom på under udviklingen. Undersøgelsen skal give grundlag for at vurdere relevante risici og gennemføre og efterprøve udvalgte forbedringer.

## Problemformulering

**I hvilket omfang beskytter miseOS' autentifikation og adgangskontrol udvalgte data og funktioner mod uautoriseret adgang, og hvordan kan relevante sikkerhedsforbedringer begrundes, implementeres og efterprøves med udgangspunkt i A01 og A07 fra OWASP Top 10:2025?**

## Underspørgsmål

1. Hvilke data og funktioner skal beskyttes i de valgte arbejdsgange, hvem skal have adgang, og hvilke misbrugsscenarier er relevante?
2. Hvordan kan udvalgt teori om A01 og A07 og relevante testvejledninger omsættes til begrundede tests og kodegennemgang af miseOS?
3. I hvilket omfang opfylder de undersøgte kontroller de fastlagte sikkerhedskrav, og hvilke årsager og risici viser eventuelle afvigelser?
4. Hvilke forbedringer bør prioriteres ud fra undersøgelsen, og hvordan kan udvalgte forbedringer implementeres og forklares i koden med hensyn til sikkerhed, brugervenlighed og vedligeholdelse?
5. Hvad viser gentest og test af tilladt brug om forbedringernes virkning, og hvilke begrænsninger og resterende risici har undersøgelsen?

## Forberedelse før rapportarbejdet

Jeg vil først arbejde mig gennem Top 10 med teori og udvalgte øvelser. For hvert emne vil jeg skrive ned, hvad fejlen går ud på, hvilke betingelser der skal være til stede, hvordan jeg kan lede efter den, og hvad der skal til for at undgå eller rette den. På den måde bygger jeg løbende min opskrift op.

Juice Shop og WebGoat bruger jeg til træning, før jeg går i gang med rapportens undersøgelse af miseOS. Her øver jeg mig i at vælge tests, forstå resultater og forklare årsag og afhjælpning. Øvelserne indgår ikke som selvstændige undersøgelser eller resultater i rapporten.

## Sådan vil jeg undersøge miseOS

Top 10 bruger jeg som overblik over risikokategorier. Til den konkrete undersøgelse vælger jeg relevante testtilfælde fra OWASP Web Security Testing Guide og eventuelt enkelte krav fra OWASP ASVS. Jeg begrunder, hvilke dele jeg bruger, og noterer versionerne. Jeg samler erfaringerne fra træningen i [min fremgangsmåde til sikkerhedsundersøgelser](emner/fremgangsmaade.md), som hjælper mig med at vælge tests, dokumentere resultater og efterprøve rettelser i miseOS. Fremgangsmåden tilpasses de valgte scenarier; selve øvelsesresultaterne indgår ikke i rapporten.

I miseOS vil jeg først beskrive, hvilke data og funktioner der skal beskyttes, og hvem der skal have adgang til dem. Det bruger jeg til at vælge konkrete tests og gennemgå den relevante kode. Jeg vil blandt andet bruge Burp Suite til at ændre og gentage requests og teste med forskellige brugere og roller. Jeg vil sammenholde det, der sker i frontend og API, med den kontrol, der ligger i backenden. Jeg har allerede forberedt en testmatrix til adgangskontrollen, men testene er endnu ikke kørt.

Jeg vil opstille en enkel trusselsmodel med værdier, mulige angribere, indgangspunkter og tillidsgrænser. De forventede adgangsregler skal begrundes i systemets tilsigtede brug og afklares, hvor dokumentation og kode er forskellige. Koden er noget, jeg undersøger; den kan ikke alene afgøre, hvad der bør være tilladt. Jeg vil lade en medstuderende udfordre trusselsmodellen og notere, hvad feedbacken ændrer i mine antagelser og tests.

For hvert væsentligt resultat vil jeg dokumentere forventet og faktisk adfærd, forudsætninger og evidens. Ved en svaghed vurderer jeg, hvem der kan udnytte den, hvor realistisk scenariet er, og hvilke konsekvenser det har for miseOS. Det bruger jeg til at prioritere forbedringer.

Jeg vil gemme de relevante requests og resultater og forklare årsagen til eventuelle svagheder i koden. For de svagheder, jeg vælger at rette, vil jeg vise den relevante kode før og efter ændringen og forklare, hvorfor rettelsen afhjælper svagheden. Derefter vil jeg gentage det samme angrebsforsøg og dokumentere, om den tidligere påviste uautoriserede adgang eller handling nu bliver forhindret. Jeg vil også kontrollere, at den tilladte brug af funktionen stadig virker. Hvis en rettelse kun er foreslået, skal det fremgå, at den ikke er implementeret og testet. Hvis en kontrol allerede fungerer, vil jeg også dokumentere det.

Et succeskriterium kan være, at en LINE_COOK fortsat kan læse sin egen råvareanmodning, mens en anden LINE_COOK og en bruger uden login afvises fra den beskyttede anmodning. Jeg kontrollerer både svarets indhold og eventuelle ændringer i data. For autentifikation kan et kriterium være, at et gyldigt token accepteres, mens et udløbet token eller et token med ugyldig signatur afvises. De præcise forventninger fastlægges før testene.

Rapporten skal indeholde udvalgte implementerede og efterprøvede forbedringer. Hvis kontrollerne allerede virker i de valgte tests, dokumenterer jeg det og vurderer muligheder for yderligere beskyttelse og bedre automatiseret verifikation. Hvis undersøgelsen ikke giver grundlag for en relevant ændring af selve sikkerhedskontrollerne, vil jeg drøfte med Thomas, hvad der opfylder vejledningens forventning om en implementeret forbedring.

Det, jeg vil stå med til sidst, er en fremgangsmåde, jeg kan bruge igen, og en undersøgelse af miseOS med konkrete eksempler på, hvad jeg har testet, hvad jeg har ændret, og hvad resultaterne viser.

## Afgrænsning

Jeg vil lære om alle ti kategorier. Den brede undersøgelse giver mig et grundlag for at vælge og begrunde rapportens fokus. Til rapporten foreslår jeg en case om sikkerhedsforbedringer med en afgrænset undersøgelse af A01 og A07. Andre kategorier kan indgå som perspektivering eller begrunde et ændret emnevalg efter drøftelsen med Thomas.

Mit foreløbige udvalg er:

- **A01 — adgangskontrol:** Ejerskab ved læsning og ændring af råvareanmodninger samt rollekrav ved godkendelse. Offentlig adgang til publicerede menuer og beskyttelse af kladder er en mulig udvidelse.
- **A07 — autentifikation:** Login med korrekte og forkerte oplysninger samt validering af JWT, herunder udløb og ugyldig signatur, ved adgang til udvalgte beskyttede API-ruter.

Jeg vælger et begrænset antal scenarier fra den eksisterende matrix og supplerer med autentifikationstests. Logout og håndtering af allerede udstedte tokens ved ændringer i brugerens konto kræver først en afklaring af den ønskede adfærd. De er mulige udvidelser. WebSocket, SSE, eksterne tjenester, afhængigheder og en fuld gennemgang af serverens opsætning ligger uden for det foreløbige rapportfokus.

Rapportens undersøgelse og resultater handler om miseOS. Forsøgene foregår i et lokalt testmiljø med egne testdata. Hvis jeg ikke finder en fejl i en test, vil jeg beskrive, hvad jeg har undersøgt, og hvad jeg stadig ikke ved.

Jeg registrerer backend- og frontendversion, relevant opsætning, testkonti og fremgangsmåde, så undersøgelsen kan gentages. Jeg beskriver tilladelsen og afgrænsningen for testene samt relevante etiske og juridiske rammer med kilder. Tokens, passwords og personoplysninger fjernes fra delt evidens. Det lokale Linux-/containermiljø beskrives i det omfang, det er relevant for at forstå og gentage undersøgelsen.

## Det vil jeg drøfte med min lærer

Jeg vil gerne gennemgå hele Top 10 som læringsforløb og bruge udvalgte erfaringer som grundlag for en fokuseret rapport. Mit forslag er A01 og A07, men emnevalget er foreløbigt.

- Er der krav om præcis to OWASP-kategorier, eller gælder vejledningens forslag om ét eller to sikkerhedstemaer?
- Passer de foreslåede scenarier og A01/A07 til rapportens omfang på 8–12 normalsider, og hvordan skal forslaget om tre til fem sikkerhedsproblemer forstås?
- Hvordan dokumenterer jeg Linux, etik/jura og samarbejde til eksamen, og hvad forventes behandlet i rapporten?
- Hvilken implementeret forbedring forventes, hvis de undersøgte kontroller allerede virker?

## Grundlag for udkastet

- [Rapportens indhold](../report_content.md) og [mulige rapportformater](../report_format.md).
- [Kildegrundlag om OWASP og testmetode](ressourcer/problemformulering-kildegrundlag.md).

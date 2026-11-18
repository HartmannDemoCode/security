# Ekstra læsning og podcasts

Status: Forslag til materiale, ikke mine færdige undersøgelser.

Udvalget er fundet den 24. oktober 2026 ud fra artikler, indholdsfortegnelser, officielle episodesider og transskripter. Bøgerne er ikke læst fra ende til anden, og podcastafsnittene er ikke gennemlyttet som del af undersøgelsen.

Materialet er på engelsk.

Det her er supplement til [ugeplanen](../forloeb/ugeplan.md). Jeg behøver ikke gennemgå hele listen. Jeg kan vælge noget, der hjælper med de spørgsmål, øvelserne giver mig.

## Tre steder at starte

- [OWASP om IDOR](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html), når jeg arbejder med adgang til andre brugeres data. Den forbinder problemet med konkrete kontroller og tests.
- [Google: Know, Prevent, Fix](https://security.googleblog.com/2021/02/know-prevent-fix-framework-for-shifting.html), når jeg arbejder med afhængigheder. Den forklarer, hvorfor det kræver mere end bare at vælge et bibliotek, man stoler på.
- [Security Now 849: Log4j & Log4Shell](https://twit.tv/shows/security-now/episodes/849), hvis jeg vil høre en teknisk gennemgang af en konkret hændelse. Jeg kan gå til delen om Log4j og springe de øvrige nyheder over.

## Til login, sessioner og adgangskontrol – A01 og A07

### OWASP: Insecure Direct Object Reference Prevention

Gratis artikel: [IDOR Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html).

Jeg ville begynde med Examples, Verifying access controls og Java and Spring Boot example. Teksten viser, hvorfor et svært gætteligt ID ikke erstatter kontrol af brugerens adgang. Den beskriver også en test med to brugere og objekter, de hver især ejer.

Det passer direkte til mit dokumenteksempel og kan hjælpe mig med at forklare rettelsen efter labben.

### OWASP: Session Management

Gratis reference: [Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).

Det er en længere tekst, så jeg ville vælge afsnittene Cookies, Renew the Session ID After Any Privilege Level Change og Automatic Session Expiration. De handler om beskyttelse af sessionen og dens levetid, hvilket supplerer forståelsen af loginøvelsen.

Jeg kan bagefter undersøge, hvilke af kontrollerne mit Java-framework håndterer, og hvad der kræver konfiguration.

### Bog: API Security in Action af Neil Madden

[Forlagets bogside](https://www.manning.com/books/api-security-in-action) og [forlagets indholdsfortegnelse](https://www.simonandschuster.net/books/API-Security-in-Action/Neil-Madden/9781617296024).

Bogen kræver køb eller adgang gennem en relevant bogtjeneste. Den er fra 2020 og er på 576 sider, så det er ikke en lille bog. Den er interessant som opslagsbog, fordi den henvender sig til udviklere med REST-erfaring og bruger Java-eksempler.

Jeg ville vælge kapitel 4, Session cookie authentication, og derefter de relevante dele af kapitel 8, Identity-based access control. Kapitel 6 om JWT'er kan vente, til jeg har et konkret spørgsmål om tokens.

Eventuelle biblioteksversioner og konkrete opsætningsråd skal kontrolleres mod aktuel dokumentation.

## Til afhængigheder og sikker opsætning – A03 og A02

### Google: Know, Prevent, Fix

Gratis artikel fra 2021: [Know, Prevent, Fix: A framework for shifting the discussion around vulnerabilities in open source](https://security.googleblog.com/2021/02/know-prevent-fix-framework-for-shifting.html).

Jeg ville læse Know your Vulnerabilities, Prevent New Vulnerabilities og Fix or Remove Vulnerabilities. Artiklen forklarer blandt andet, hvorfor en rettelse kan være fanget længere nede i kæden af indirekte afhængigheder, og hvorfor beskeder om opdateringer er nødvendige.

Det passer godt til spørgsmålet om et JWT-bibliotek, der er blevet glemt på en droplet. Jeg bruger artiklen til at forstå problemet og arbejdsformen; Dependabots aktuelle funktioner undersøges i GitHubs dokumentation.

### Bogkapitel: Deploying Code

Gratis online kapitel: [Kapitel 14 i Building Secure and Reliable Systems](https://google.github.io/building-secure-and-reliable-systems/raw/ch14.html), skrevet af Jeremiah Spradlin og Mark Lodato med Sergey Simakov og Roxana Loza. Bogen kan findes gennem [Googles bogoversigt](https://sre.google/books/).

Jeg ville begynde med Concepts and Terminology og derefter vælge Rely on Automation, Treat Configuration as Code og Post-Deployment Verification. Det hjælper med forbindelsen fra en ændring i repoet til det build og den konfiguration, som faktisk bliver sat i drift.

Det er et kapitel fra en større bog. Jeg kan vente med de avancerede dele om blandt andet dokumentation af, hvor et build kommer fra.

### OWASP: Docker Security

Gratis reference: [Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html).

Jeg ville vælge reglerne om at holde host og Docker opdateret, vælge containerens bruger og passe på port-mapping sammen med hostens firewall. Det giver konkrete spørgsmål at stille til en container på en droplet.

Det er især relevant til A02-delen: Opdaterede biblioteker er kun én del af opsætningen. Jeg bruger teksten som udgangspunkt for at undersøge mit eget lokale miljø.

## Podcasts som ekstra forklaring og cases

### Security Now 849: Log4j & Log4Shell

Gratis lyd på [den officielle episodeside](https://twit.tv/shows/security-now/episodes/849), med [transskript hos Steve Gibson](https://www.grc.com/sn/sn-849.htm). Afsnittet er fra 14. december 2021 og har Steve Gibson og Leo Laporte som værter.

Jeg ville vælge delen om Log4j & Log4Shell. I transskriptet kan jeg søge efter “Let's talk about Log4Shell”, hvor hovedgennemgangen begynder. Det er mit første podcastvalg til at forstå, hvordan et logging-bibliotek kunne blive til et sikkerhedsproblem i mange applikationer. Resten af afsnittet indeholder andre nyheder.

Det er en historisk gennemgang. Versionsnumre og midlertidige rettelser i afsnittet er ikke aktuelle opdateringsråd; dem kontrollerer jeg hos Apache.

### Darknet Diaries 73: WannaCry

Gratis episode: [WannaCry](https://darknetdiaries.com/episode/73/), med [fuldt transskript](https://darknetdiaries.com/transcript/73/). Udgivet 1. september 2020 om hændelsen i 2017.

Det er et ekstra valg, hvis jeg hellere vil have en fortælling om en virkelig hændelse. Episoden handler blandt andet om ransomwareangrebet og konsekvenserne for det britiske sundhedsvæsen. Den kan give anledning til at diskutere vedligeholdelse og opdateringer af resten af stacken.

Jeg ville bruge den som case til driftsansvar. Værten anbefaler nogle tidligere afsnit som baggrund, men skriver i transskriptet også, at denne episode kan stå alene.

## Hvad jeg vil notere efter læsning eller lytning

Jeg vil skrive, hvad materialet gjorde tydeligere, og om jeg kunne forbinde forklaringen med en af mine afprøvninger. Jeg vil også notere, hvad jeg stadig mangler at forstå, og om jeg ville anbefale materialet til de andre på holdet.

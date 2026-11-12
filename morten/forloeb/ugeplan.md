# Ugens undersøgelse

Status: I gang. OWASP A01 og A07 er læst, og PortSwiggers adgangskontrolafsnit med 13 labs er gennemført. Campfires A01 - Broken Access Control, basic-forløb og grundlæggende sikkerhedsgennemgang er også gennemført. Overførslen til eget Java-projekt og loginøvelsen mangler endnu.

Den samlede ramme er 20 timer. Jeg arbejder med to samlede forløb: login og adgangskontrol samt afhængigheder og sikker opsætning. Der er ingen fast fordeling mellem opgaverne.

Forløbene tager fat på A01 og A07 samt A03 og A02 fra OWASP Top 10:2025. Det er en første undersøgelse af udvalgte fejl og øvelser. Jeg har ikke gennemgået en hel kategori, bare fordi jeg har løst en enkelt opgave.

## Systemet jeg undersøger til sidst

miseOS er valgt til den senere rapport og er det sidste, jeg vil afprøve. Først lærer jeg teorien og arbejder med øvelser i blandt andet PortSwigger og WebGoat. Derefter anvender jeg det, jeg har lært, på udvalgte dele af miseOS. FOG kan også bruges som øvelsesprojekt undervejs.

[Adgangskontrolmatrixen](../emner/adgangskontrol/miseos-adgangskontrol.xlsx) er forberedt til den afsluttende undersøgelse og har et afgrænset startsæt på 12 tests af profiler, råvareanmodninger, roller og menuadgang. De øvrige tests er kandidater, og testene er endnu ikke kørt. Jeg tilpasser omfanget, når jeg når til miseOS; ugeplanen er ikke en plan for at gennemføre alle matrixens ruter.

Min første milepæl i miseOS bliver et dokumenteret forsøg med egen råvareanmodning, en anden brugers anmodning og adgang uden login. Jeg skal kunne forklare resultatet ud fra miseOS-koden og gemme evidens med test-ID, dato og version.

## Det jeg vil have ud af ugen

Jeg vil vise en adgangskontrolfejl og en fejl i et loginforløb. Jeg vil også undersøge en sårbar afhængighed og en usikker indstilling i et lokalt miljø. For hvert eksempel vil jeg forklare årsag, konsekvens og rettelse.

PHP2Shell er min case til opdateringsdelen. Jeg tager udgangspunkt i PHP-projektets egne beskrivelser og undersøger, hvad sagen siger om den software, vores applikationer bygger på.

## Login og adgangskontrol – A01 og A07

- [x] Læse OWASP A01 og A07.
- [x] Bruge PortSwiggers adgangskontrolafsnit og de 13 tilhørende labs til uddybning af A01.
- [x] Gennemføre Campfires A01 - Broken Access Control (100 XP), basic-forløb og grundlæggende sikkerhedsgennemgang.
- [ ] Bruge PortSwigger til uddybning af A07.
- [x] Løse én IDOR-lab: User ID controlled by request parameter, with unpredictable user IDs.
- [ ] Undersøge WebGoats opsætning og afprøve en relevant adgangskontroløvelse som næste skridt.
- [ ] Løse 2FA simple bypass.
- [ ] Gemme relevante requests og resultater fra øvelserne.
- [ ] Skrive en kort forklaring af årsag, konsekvens og rettelse.
- [ ] Forklare rettelsen til loginøvelsen og forskellen mellem de to kategorier.
- [ ] Til sidst gennemføre de første adgangstests i miseOS og undersøge relevant Java-kode.
- [ ] I miseOS teste ejerens adgang, en anden bruger og adgang uden login på råvareanmodninger.

Jeg bruger [adgangskontrolforløbet](01-adgangskontrol.md) til detaljerne. Jeg prioriterer nu mere teori og øvelser, herunder WebGoat, før den afsluttende undersøgelse i miseOS.

## Afhængigheder og sikker opsætning – A03 og A02

- [ ] Læse OWASP A03 og A02 og PHP Foundations forklaring af PHP2Shell.
- [ ] Undersøge forskellen på Dependabot alerts, security updates og version updates.
- [ ] Bruge et tidligere Java-projekt til at undersøge direkte og indirekte afhængigheder.
- [ ] Skrive et udkast til Dependabot-konfiguration og notere de GitHub-indstillinger, der også er nødvendige.
- [ ] Vælge én lokal øvelse med en sårbar afhængighed og vise konsekvensen af fejlen.
- [ ] Undersøge rettelsen, gentage angrebet efter ændringen og kontrollere, at den almindelige funktion stadig virker.
- [ ] Vise én usikker indstilling lokalt, rette den og kontrollere forskellen.
- [ ] Beskrive, hvordan runtime, container-image og server holdes opdateret ved siden af bibliotekerne.
- [ ] Foreslå en rutine, så nye releases og sikkerhedsrettelser bliver fulgt op, og kontrollere vejen fra opdaterings-PR til den version, der kører på en droplet.

Detaljerne ligger i [forløbet om afhængigheder](02-afhaengigheder.md).

## Det jeg tager med til mødet

- En demonstration af adgangskontrolfejlen og det, jeg har nået at teste af rettelsen.
- Et eksempel på fejl i et loginforløb og forskellen på identitet og adgang til en ressource.
- En forklaring af Dependabots funktioner og begrænsninger.
- Et eksempel på en sårbar afhængighed før og efter rettelsen, eller en tydelig beskrivelse af, hvor langt jeg nåede med afprøvningen.
- Et eksempel på en fejlkonfiguration og resultatet efter ændringen.
- En kort forklaring af PHP2Shell og hvorfor en biblioteksopdatering ikke nødvendigvis opdaterer PHP-runtime'en.
- Min vurdering af, hvad der kunne blive gode øvelser i valgfaget.

Hvis jeg ikke når det hele, skriver jeg konkret, hvad der mangler. Jeg vil hellere have dokumenterede resultater end flere halvfærdige eksempler.

## Efter mødet

Dato:

Det jeg nåede:

Det vi blev enige om:

Næste skridt:

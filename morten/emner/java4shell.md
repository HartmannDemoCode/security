# Java4Shell (CVE-2026-114938)

Status: Første undersøgelse af min droplet er lavet 18. november 2026. CPU'en og JVM-flaget i miseOS er kontrolleret. Java-versionen i miseOS mangler at blive kontrolleret.

Sagen hører til samme spor som [PHP2Shell](afhaengigheder.md#php2shell): en fejl i selve runtime'en og ikke i et bibliotek, applikationen selv har valgt. Den passer under [A03:2025 – Software Supply Chain Failures](https://top10.owasp.org/2025/A03_2025-Software_Supply_Chain_Failures/), fordi det handler om at holde den software opdateret, som applikationen kører på.

## Hvor hører den til i OWASP Top 10?

Java4Shell hører primært under **A03:2025 – Software Supply Chain Failures**. Fejlen sidder i en komponent, applikationen er bygget på (Java-runtime'en), og ikke i applikationens egen kode eller design. A03 omfatter i 2025 det, der før hed *Vulnerable and Outdated Components*. Selve fejlen er en skrivning uden for buffer (CWE-787). Det er en hukommelsesfejl i C++/assembly-koden i JVM'en, og den slags fejl er ikke en kategori i Top 10, fordi listen handler om webapplikationer.

**Er det A06 Insecure Design?** Nej, ikke som årsag. A06 handler om fejl i applikationens eget design, fx et loginforløb der mangler en kontrol, eller en forretningsregel der kan misbruges. Java4Shell viser det modsatte: Man kan have et sikkert design og korrekt kode og stadig blive hacket helt ned på serveren, fordi fejlen ligger et lag under applikationen. Ingen kodegennemgang af miseOS ville have fundet den.

Sikkert design kan ikke forhindre fejlen, men det kan **begrænse konsekvensen**. Det er her, A06-tankegangen hører hjemme (defense in depth):

- Containeren kører ikke som root, så RCE ikke giver adgang til hele værten. Det er det, jeg fandt i [FOG-containeren](afhaengigheder.md#undersøgelse-af-min-fog-container).
- Mindst mulige rettigheder til databasen og netværket fra containeren.
- Hemmeligheder (JWT-nøgle, databaseadgangskode) med kort levetid, så en læsning af hukommelsen ikke giver adgang for altid.
- Logning og alarmer (A09), så et angreb eller et nedbrud bliver opdaget. Et mislykket forsøg kan få JVM'en til at gå ned.

**Andre kategorier, sagen rører:**

- **A02 Security Misconfiguration:** Den midlertidige løsning er en JVM-indstilling (`-XX:-UseBASE64Intrinsics`). Hvor sættes den, og bliver den fjernet igen efter opdateringen?
- **A07 Authentication Failures:** Ikke som årsag. Men Basic auth-headere og JWT'er er Base64 og dekodes før login, så loginkoden er den vej, en angriber kan nå fejlen uden en konto. Se [A07-noterne](authentication.md#forbindelse-til-java4shell).

Min pointe: OWASP-kategorierne er ikke vandtætte. Årsagen er A03, men forsvaret er en blanding af A03 (opdatere), A02 (midlertidig indstilling), A06 (begrænse skaden) og A09 (opdage det).

## Hvad er Java4Shell?

Java4Shell blev offentliggjort af forskerne hos Calif den 17. november 2026. Navnet ligner Log4Shell, men det er en anden sag. Log4Shell var en fejl i logbiblioteket Log4j. Java4Shell er en fejl i selve OpenJDK.

Fejlen sidder i Base64-dekoderen i HotSpot-JVM'en. På nogle Intel- og AMD-processorer bruger Java en hardwareoptimeret genvej (en *intrinsic*) til at dekode Base64 hurtigere. Med en særligt konstrueret Base64-streng kan genvejen skrive uden for den hukommelse, den har fået tildelt. Det kan ødelægge andre data i hukommelsen.

Ifølge Calif kan det bruges til to ting:

- at læse hukommelse, som kan indeholde nøgler og tokens
- at køre kode på serveren (RCE)

Base64 bruges mange steder, ofte før brugeren er logget ind. Eksempler er Basic authentication-headeren, JWT'er og filupload. Derfor kan en angriber i nogle applikationer nå fejlen uden login. Calif har vist kodeeksekvering i standardopsætninger af blandt andet Keycloak, Jira, Confluence, Jenkins og JFrog Artifactory. Der skal laves et særskilt exploit til hver applikation.

| | |
|---|---|
| CVE | CVE-2026-114938 |
| Offentliggjort | 17. november 2026 |
| Komponent | OpenJDK, HotSpot Base64-dekoder på x86-64 |
| Type | Hukommelseskorruption (skrivning uden for buffer) |
| Konsekvens | Læsning af hukommelse og mulig RCE |
| CVSS | Oracle 8.1 (CVSS 3.1), Calif 9.2 (CVSS 4.0) |
| Kendt udnyttelse | Ikke kendt pr. 17. november ifølge Tenable |

### Hvornår er et system ramt?

Alle tre betingelser skal være opfyldt:

1. **Java-versionen er berørt.** Java 17, 21, 25 og 27. Calif skriver 17.0.6 og nyere. Almindelig Java 8 og 11 er ikke nævnt i OpenJDK's advisory. Det gælder også distributioner bygget på OpenJDK, fx Amazon Corretto, Eclipse Temurin og Microsofts build.
2. **CPU'en understøtter AVX-512 VBMI.** CPU'en skal have alle fire flag: `avx512f`, `avx512bw`, `avx512vbmi` og `bmi2`. Det gælder fx Intel Ice Lake og nyere samt AMD Zen 4 og nyere. ARM64 er ikke ramt.
3. **Applikationen dekoder Base64 fra brugeren.** Data fra en angriber skal kunne nå de berørte Base64-API'er.

Det var den vigtigste pointe for mig: en sårbar Java-version betyder ikke automatisk, at systemet kan angribes. Hardwaren og applikationens brug af Base64 afgør, om fejlen kan nås.

### Rettelse og midlertidig beskyttelse

Oracle udgav rettelser den 17. november: 17.0.21.1, 21.0.13.1, 25.0.5.1 og 27.0.1.1. Amazon Corretto udgav 17.0.21.1.2 samme dag. Jeg har ikke fundet en tekst fra Amazon, der nævner CVE'en ved den version, så det skal jeg kontrollere i Correttos changelog.

Indtil der er opdateret, anbefaler Calif én af to JVM-indstillinger:

- `-XX:-UseBASE64Intrinsics` slår den sårbare Base64-genvej fra.
- `-XX:UseAVX=2` slår AVX-512 fra. Det virker kun på x86-64; på ARM starter Java ikke med indstillingen.

Begge er midlertidige løsninger. De står ikke i Oracles eller OpenJDK's egne advisories, og de kan gøre Base64-tunge programmer langsommere.

## Min undersøgelse af dropletten

Dropletten kører Ubuntu 22.04 på x86-64 med CPU-modellen DO-Regular. Jeg undersøger miseOS, som kører i en Docker-container på dropletten.

### 1. Understøtter CPU'en AVX-512 VBMI?

```bash
grep -m1 '^flags' /proc/cpuinfo | tr ' ' '\n' | grep -E '^(avx512f|avx512bw|avx512vbmi|bmi2)$'
```

Resultat:

```text
bmi2
```

CPU'en har kun `bmi2`. De tre AVX-512-flag mangler. En container bruger værtens CPU, så resultatet gælder også inde i miseOS-containeren.

### 2. Bruger JVM'en i miseOS den sårbare genvej?

```bash
docker exec miseOS sh -c "java -XX:+PrintFlagsFinal -version 2>&1 | grep UseBASE64Intrinsics"
```

Resultat:

```text
bool UseBASE64Intrinsics = false {product} {default}
```

Genvejen er slået fra. `{default}` betyder, at det er JVM'ens eget valg. Det er ikke en indstilling, jeg selv har sat. Jeg tolker det sådan, at JVM'en har slået den fra, fordi CPU'en ikke understøtter den. Så det er nok ikke en ekstra beskyttelse uafhængigt af CPU'en; det er samme forklaring set fra JVM'en. Det har jeg ikke bekræftet.

### Resultat indtil nu

| Kontrol | Resultat |
|---|---|
| CPU har `avx512f`, `avx512bw`, `avx512vbmi` | Nej, kun `bmi2` |
| miseOS: `UseBASE64Intrinsics` | `false` (standard) |
| Java-version i miseOS | Mangler at blive kontrolleret med `java -version` |

Min vurdering: Fejlen kan efter alt at dømme ikke udnyttes på dropletten lige nu, fordi CPU'en mangler AVX-512 VBMI. Java skal stadig opdateres. En droplet kan blive flyttet til nyere hardware ved en resize eller migrering hos DigitalOcean, og så kan genvejen blive slået til uden ændringer i min kode. Derfor holder jeg mig ikke til hardwaren som beskyttelse.

## Næste skridt

- [ ] Kontrollere Java-versionen i miseOS: `docker exec miseOS java -version`
- [ ] Finde basis-imaget i miseOS' Dockerfile og opdatere til en rettet Java-version
- [ ] Kontrollere i Correttos changelog, at 17.0.21.1.2 indeholder rettelsen til CVE-2026-114938
- [ ] Bygge, deploye og køre `java -version` igen for at dokumentere versionen før og efter
- [ ] Undersøge, om Watchtower eller Dependabot opdager en ny Java-version i basis-imaget, eller om det skal følges op manuelt
- [ ] Undersøge, hvor miseOS dekoder Base64 fra brugeren, fx i JWT-håndteringen

## Kilder

- [Tenable – Java4Shell FAQ](https://www.tenable.com/blog/java4shell-cve-2026-114938-frequently-asked-questions-about-openjdk-rce-vulnerability) – berørte versioner, CPU-krav, rettelser og berørte applikationer
- [Endor Labs – Pre-Auth Heap Overflow in OpenJDK](https://www.endorlabs.com/learn/java4shell-unauthenticated-rce-reported-in-openjdk-17-cve-2026-114938)
- [Element Security – pre-auth RCE in OpenJDK Base64](https://element.security/java4shell-pre-auth-rce-in-openjdk-base64-cve-2026-114938/) – Califs midlertidige løsninger
- [SecurityWeek – Java4Shell](https://www.securityweek.com/java4shell-vulnerability-puts-many-enterprise-applications-at-risk-of-takeover/)
- [Amazon Corretto 17 releases](https://github.com/corretto/corretto-17/releases) – 17.0.21.1.2 udgivet 17. november

Kilderne er alle fra dagen efter offentliggørelsen. Jeg har ikke læst Califs egen advisory eller Oracles og OpenJDK's advisories direkte endnu.

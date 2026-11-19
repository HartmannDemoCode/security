# Kildegrundlag til gennemgang af problemformuleringen

Kontrolleret 12. november 2026. Dette er et researchnotat til afgrænsning af undersøgelsen, ikke resultater fra sikkerhedstest af miseOS.

## Rapportens rammer

Den **lokale kopi** af [report_content.md](../../report_content.md) foreslår 8–12 normalsider, ét eller to sikkerhedstemaer og konkrete undersøgelser med implementerede og verificerede forbedringer. Eksisterende kontroller må også undersøges; projektet behøver ikke afhænge af, at der findes en sårbarhed. Dokumentets overskrift om at vælge ét tema stemmer dog ikke helt med indledningens ét eller to.

Den **lokale kopi** af [report_format.md](../../report_format.md) anbefaler et *security improvement case study* og foreslår tre til fem sikkerhedsproblemer undersøgt i dybden. Kvaliteten af begrundelser, dokumentation og evaluering vægtes over antal sårbarheder og værktøjer. Antal problemer er ikke det samme som antal OWASP-kategorier. Dokumenterne fastslår dermed ikke et entydigt krav om præcis to kategorier. Det endelige omfang og hvilken vejledning der er styrende, bør afklares med Thomas.

De aktuelle GitHub-versioner af [rapportindhold](https://github.com/HartmannDemoCode/security/blob/main/report_content.md) og [rapportformat](https://github.com/HartmannDemoCode/security/blob/main/report_format.md) er også kontrolleret under gennemgangen og stemmer overens med de lokale kopier på disse punkter.

## Kategorier og metoder

[OWASP Top 10:2025](https://top10.owasp.org/2025/) beskriver centrale risikokategorier for webapplikationer og betegner sig som et awareness-dokument. Min vurdering er derfor, at Top 10 er velegnet til at vælge og forklare fokusområder, mens konkrete testtrin bør hentes fra eksempelvis [OWASP Web Security Testing Guide v4.2](https://wstg.owasp.org/v4.2/4-Web_Application_Security_Testing/).

| Kategori | Fagligt indhold | Mulig afgrænset undersøgelse i miseOS |
| --- | --- | --- |
| [A01:2025 Broken Access Control](https://top10.owasp.org/2025/A01_2025-Broken_Access_Control/) | Autorisation: om handlinger og dataadgang overholder brugerens rettigheder. Kontrollen skal håndhæves på serveren. | Test udvalgte API-handlinger med forskellige roller og forskellige brugere med samme rolle. Sammenhold adgang til egne og andres ressourcer med de tilsigtede forretningsregler. |
| [A07:2025 Authentication Failures](https://top10.owasp.org/2025/A07_2025-Authentication_Failures/) | Autentifikation og sessionshåndtering: om identitet og tokens kontrolleres korrekt, herunder udløb og logout. | Undersøg login og udvalgte dele af JWT-livscyklussen: gyldighedskontrol, udløb og den adgang, der fortsat er mulig efter logout. |
| [A02:2025 Security Misconfiguration](https://top10.owasp.org/2025/A02_2025-Security_Misconfiguration/) | Usikker opsætning, eksempelvis unødvendige tjenester, informationsrige fejl og manglende hardening. | Alternativt fokus: udvalgte konfigurationskontroller i et lokalt miljø, der repræsenterer den relevante deployment. Resultaterne kan kun udtale sig om det undersøgte miljø. |
| [A03:2025 Software Supply Chain Failures](https://top10.owasp.org/2025/A03_2025-Software_Supply_Chain_Failures/) | Fejl i softwareforsyningskæden, herunder direkte og transitive afhængigheder samt build-, distributions- og opdateringsprocesser. | Alternativt fokus: få udvalgte afhængigheder og relevante advisories, en begrundet opdatering og kontrol af kompatibilitet. En scanneralarm er et udgangspunkt for vurdering, ikke i sig selv dokumentation for et udnytteligt hul i miseOS. |

Kolonnen med mulige undersøgelser er forslag, ikke påviste fejl. Adgang til andre brugeres data er kun et sikkerhedsproblem, hvis det strider mod de tilsigtede adgangsregler. Eksempelvis kan en køkkenleder legitimt have adgang til en anden medarbejders råvareanmodning.

## Foreløbig anbefaling

**A01 + A07** giver et sammenhængende fokus på identitet og rettigheder. Valget bygger også på det eksisterende arbejde: [ressourcenoten om adgangskontrol](adgangskontrol.md) beskriver læsning af begge kategorier og en forberedt testmatrix, men ingen udførte miseOS-tests endnu. Undersøgelsen bør udvælge enkelte forløb frem for at love fuld dækning af begge kategorier.

Kategorierne overlapper omkring tokens. Rapporten bør forklare årsagen til hvert fund og undgå at tælle samme fejl to gange. Logout skal vurderes ud fra den valgte JWT-model, forventet adfærd og risiko; A01 omtaler blandt andet kortlivede stateless JWT'er og muligheder for tilbagekaldelse ved længere levetid. At frontend sletter et token, dokumenterer ikke alene, hvad serveren accepterer efter logout.

Konkrete testvejledninger:

- [WSTG-ATHZ-02: Testing for Bypassing Authorization Schema](https://wstg.owasp.org/v4.2/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema/): tests på tværs af identiteter og roller.
- [WSTG-SESS-06: Testing for Logout Functionality](https://wstg.owasp.org/v4.2/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality/): sammenhold logout i brugergrænsefladen med faktisk afslutning af adgang.

En bred gennemgang af alle ti kategorier kan fortsat indgå i læringsforløbet. Min anbefaling er, at rapportens hovedspørgsmål og dokumentation koncentreres om det valgte fokus. Juice Shop og WebGoat bruges som forberedende træning. Rapportens undersøgelser og resultater handler om miseOS.

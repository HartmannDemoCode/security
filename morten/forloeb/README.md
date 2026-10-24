# Overblik over forløbene

Jeg samler beslægtede OWASP-emner i samme case, så jeg kan bruge opsætning og kode igen. Det her er min opdeling til undersøgelserne, ikke OWASPs officielle inddeling af undervisning.

Den aktuelle prioritering ligger i [ugeplanen](ugeplan.md). Resten af listen er et forslag til senere forløb. Ingen af kategorierne er færdigundersøgt endnu.

## 1. Login, sessioner og adgangskontrol

A01 Broken Access Control og A07 Authentication Failures.

Jeg vil undersøge, hvordan man både kontrollerer brugerens identitet og adgangen til den konkrete handling eller ressource. Jeg bruger login og dokumenter i samme applikation som udgangspunkt.

[Plan for forløbet](01-adgangskontrol.md).

## 2. Afhængigheder, opdateringer og sikker opsætning

A03 Software Supply Chain Failures og A02 Security Misconfiguration.

Jeg vil undersøge, hvordan et projekt holdes opdateret og sættes sikkert i drift. Jeg bruger en sårbar afhængighed, en fejlkonfiguration og vejen til deployment i samme case. Dependabot og PHP2Shell indgår her.

[Plan for forløbet](02-afhaengigheder.md).

## 3. Input og fejlhåndtering

A05 Injection og A10 Mishandling of Exceptional Conditions.

Et muligt forløb om, hvad der sker med uventet eller ondsindet input. Jeg kan undersøge både injection og fejl i håndteringen af afbrudte eller mislykkede operationer. Det kræver forskellige eksempler; en injection-opgave dækker ikke automatisk begge kategorier.

Skal planlægges.

## 4. Kryptografi og integritet

A04 Cryptographic Failures og A08 Software or Data Integrity Failures.

Et muligt forløb om beskyttelse af data og kontrol af, hvor kode og data kommer fra. Jeg kan undersøge kryptering og integritetskontrol med hver sit eksempel og bruge det til at diskutere, hvilken beskyttelse der er nødvendig.

Skal planlægges.

## 5. Sikkert design, logging og alarmer

A06 Insecure Design og A09 Security Logging and Alerting Failures.

Et muligt forløb, hvor jeg beskriver et misbrug af en applikation, vurderer forebyggelsen i designet og undersøger, hvilke hændelser der skal logges og udløse en reaktion. Logging er en del af opdagelsen; det erstatter ikke rettelsen af et usikkert design.

Skal planlægges.

## Opsamling undervejs

For hver kategori vil jeg kunne pege på min forklaring, en relevant afprøvning, en rettelse eller forebyggelse og en vurdering i forhold til læringsmålene. Hvis noget kun er læst eller beskrevet, skriver jeg det.

Kategorierne og navnene tager udgangspunkt i [OWASP Top 10:2025](https://top10.owasp.org/2025/).

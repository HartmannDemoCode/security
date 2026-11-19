# OWASP ASVS – Application Security Verification Standard

ASVS er en standard med detaljerede sikkerhedskrav til webapplikationer og webservices. Den kan bruges fra krav og design til implementering og verifikation. Kravene giver både udvikleren noget konkret at bygge efter og testeren noget at efterprøve.

## Udgave og officielle ressourcer

Kontrolleret 27. oktober 2026: Den aktuelle stabile udgave er **ASVS 5.0.0**, udgivet 30. maj 2025.

- [ASVS – officiel projektside og downloads](https://asvs.owasp.org/)
- [ASVS 5.0.0 – den udgivne standard](https://github.com/OWASP/ASVS/tree/v5.0.0/5.0/en)

Jeg bruger den faste 5.0.0-udgave som reference. Når jeg henviser til et bestemt krav, tager jeg versionsnummeret med, fordi krav og numre kan ændre sig mellem udgaver. OWASP anbefaler formatet `v<version>-<kravnummer>`.

## Sammenhæng med Proactive Controls

[Proactive Controls](proactive-controls.md) giver en introduktion til centrale sikkerhedstiltag. ASVS går mere i detaljer med, hvilke krav applikationen skal opfylde, og hvad der skal verificeres. OWASP anbefaler selv at supplere Proactive Controls med mere omfattende krav fra ASVS. [Kilde](https://top10proactive.owasp.org/archive/2024/)

Et eksempel er adgangskontrol: C1 i Proactive Controls introducerer principper som at afvise adgang som standard og kontrollere adgang til den konkrete ressource. ASVS kan derefter bruges til at vælge konkrete autorisationskrav til applikationen.

## Hvordan vil jeg bruge ASVS i valgfaget?

Jeg vil vælge få relevante krav til hver øvelse og lade de studerende følge dem fra krav til kode og test. Til [login og adgangskontrol](../adgangskontrol.md) vil jeg starte med krav om autentificering, sessioner og autorisation.

Et muligt øvelsesforløb med adgang til ordrer:

1. Find relevante autorisationskrav i ASVS, og notér version og kravnummer.
2. Omsæt dem til acceptkriterier for casen, eksempelvis at en kunde kun kan læse egne ordrer.
3. Implementér kontrollen på serveren.
4. Test adgang som ejer, anden bruger og anonym. Undersøg også, om kontrollen kan omgås gennem et andet endpoint.
5. Dokumentér resultatet med henvisning til kravet og den udførte test.

Eksemplet er mit forslag til en undervisningsøvelse. Jeg skal vælge de konkrete ASVS-krav, når casen er fastlagt. Arbejde med enkelte krav dokumenterer kun den valgte del af applikationen; det er ikke en fuld ASVS-vurdering.

[Tilbage til OWASP-oversigten](README.md).

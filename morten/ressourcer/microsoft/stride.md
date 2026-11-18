# Microsoft STRIDE – trusselsmodellering

STRIDE er seks kategorier, der hjælper med at identificere mulige trusler mod et system. Man kan bruge dem til at gennemgå komponenter og dataflows, især hvor data krydser en tillidsgrænse (*trust boundary*). Microsoft beskriver metoden i [SDL-praksis 3: Sikkert design og trusselsmodellering](https://www.microsoft.com/en-us/securityengineering/sdl/practices/secure-by-design).

## De seks kategorier

Her er et kort dansk overblik baseret på [Microsofts beskrivelse af STRIDE-trusler](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats).

| Kategori | Hvad undersøger jeg? |
| --- | --- |
| Spoofing | Kan nogen udgive sig for en anden bruger eller tjeneste? |
| Tampering | Kan nogen ændre data uden tilladelse? |
| Repudiation | Kan en handling benægtes, fordi der mangler dokumentation for, hvem der udførte den? |
| Information Disclosure | Kan oplysninger blive tilgængelige for uvedkommende? |
| Denial of Service | Kan systemet gøres utilgængeligt for legitime brugere? |
| Elevation of Privilege | Kan nogen opnå rettigheder, de ikke skal have? |

## Hvordan vil jeg bruge STRIDE i valgfaget?

Mit forslag er at bruge en lille webapplikation med login og adgang til ordrer:

1. Tegn browser, API og database med dataflows og tillidsgrænser.
2. Brug kategorierne til at finde mulige trusler, eksempelvis misbrug af en stjålet session eller adgang til en anden kundes ordre.
3. Beskriv forudsætninger og konsekvenser, og prioritér truslerne.
4. Vælg sikkerhedskrav og tiltag, og test, om de håndterer truslerne.

Det kan kobles til [login og adgangskontrol](../adgangskontrol.md) og udvalgte krav fra [OWASP ASVS](../owasp/asvs.md).

## Værktøj

[Microsoft Threat Modeling Tool](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool) understøtter diagrammer og STRIDE-analyse. Jeg vil starte med et enkelt diagram og en tabel over trusler, tiltag og test; værktøjet kan undersøges bagefter.

[Tilbage til Microsoft-oversigten](README.md).

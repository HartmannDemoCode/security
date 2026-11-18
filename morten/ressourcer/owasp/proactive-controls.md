# OWASP Top 10 Proactive Controls

OWASP Top 10 Proactive Controls er en introduktion til, hvordan man bygger sikkerhed ind i applikationer fra starten. Den samler ti områder med konkrete råd til design og implementering.

Jeg vil bruge den som supplement til arbejdet med at finde og forstå sårbarheder. [OWASP Top 10:2025](https://top10.owasp.org/2025/) beskriver centrale sikkerhedsrisici i webapplikationer, mens Proactive Controls giver udviklere råd til at forebygge dem. De to lister har forskellig nummerering og er ikke en direkte én-til-én-mapping.

## Hvilken udgave?

Kontrolleret 27. oktober 2026: Den seneste færdige udgave af Proactive Controls er **2024**. OWASP viser næste udgave som *Future Top 10 (WIP)*. Derfor bruger jeg 2024-udgaven som reference til undervisning i 2025/2026.

- [OWASP Top 10 Proactive Controls – 2024-udgaven](https://top10proactive.owasp.org/archive/2024/)
- [Introduktion til kontrollerne](https://top10proactive.owasp.org/archive/2024/the-top-10/)
- [Projektets forside og kommende udgave](https://top10proactive.owasp.org/)

## De ti kontroller

Her er et kort dansk overblik. Links fører til de enkelte kontroller i 2024-udgaven med forklaringer, implementeringsråd og videre læsning.

| Kontrol | Hvad betyder det i praksis? |
| --- | --- |
| [C1: Implementer adgangskontrol](https://top10proactive.owasp.org/archive/2024/the-top-10/c1-accesscontrol/) | Kontrollér på serveren, hvem der må udføre en handling på en bestemt ressource. Afvis som standard, og giv kun nødvendige rettigheder. |
| [C2: Brug kryptografi til at beskytte data](https://top10proactive.owasp.org/archive/2024/the-top-10/c2-crypto/) | Beskyt følsomme data under transport og lagring. Brug etablerede biblioteker, og håndtér nøgler sikkert. |
| [C3: Validér input, og håndtér fejl](https://top10proactive.owasp.org/archive/2024/the-top-10/c3-validate-input-and-handle-exceptions/) | Validér input på serveren. Brug også parameteriserede databaseforespørgsler og korrekt output-encoding. Håndtér fejl uden at afsløre interne detaljer eller omgå sikkerhedskontroller. |
| [C4: Tænk sikkerhed ind fra starten](https://top10proactive.owasp.org/archive/2024/the-top-10/c4-secure-architecture/) | Definér sikkerhedskrav, undersøg trusler, og brug kravene i design, implementering og test. |
| [C5: Brug sikre standardindstillinger](https://top10proactive.owasp.org/archive/2024/the-top-10/c5-secure-by-default/) | Slå unødvendige funktioner fra, fjern standardadgangskoder, og undgå debugfunktioner i produktion. |
| [C6: Hold komponenterne sikre](https://top10proactive.owasp.org/archive/2024/the-top-10/c6-use-secure-dependencies/) | Hold styr på biblioteker, frameworks og øvrige afhængigheder. Følg sikkerhedsmeddelelser, opdatér, og fjern komponenter, der ikke bruges. |
| [C7: Beskyt digitale identiteter](https://top10proactive.owasp.org/archive/2024/the-top-10/c7-secure-digital-identities/) | Brug sikre løsninger til autentificering, sessionsstyring og kontogendannelse. Beskyt adgangskoder med egnet password-hashing, og brug flerfaktorautentificering efter behov. |
| [C8: Brug browserens sikkerhedsfunktioner](https://top10proactive.owasp.org/archive/2024/the-top-10/c8-leverage-browser-security-features/) | Brug blandt andet Content Security Policy, relevante sikkerhedsheaders og sikre cookieindstillinger som ekstra beskyttelse. |
| [C9: Implementer sikkerhedslogging og overvågning](https://top10proactive.owasp.org/archive/2024/the-top-10/c9-security-logging-and-monitoring/) | Log sikkerhedsrelevante hændelser, og sørg for overvågning og alarmer, som nogen reagerer på. Undgå adgangskoder og tokens i logs. |
| [C10: Forebyg Server-Side Request Forgery (SSRF)](https://top10proactive.owasp.org/archive/2024/the-top-10/c10-stop-server-side-request-forgery/) | Begræns, hvilke destinationer serveren må kontakte, når den henter eksterne ressourcer. Beskyt interne tjenester og cloud-metadata mod adgang via brugerinput. |

## Hvordan vil jeg bruge ressourcen?

Til valgfaget virker den relevant som bindeled mellem en sårbarhed og den kode eller opsætning, der forebygger den. Efter en øvelse kan de studerende vælge en relevant kontrol, rette problemet og teste, om rettelsen virker.

Jeg vil især koble C1 og C7 til [login og adgangskontrol](../adgangskontrol.md), og C5 og C6 til [afhængigheder og sikker opsætning](../afhaengigheder.md).

Kontrollerne giver et overblik og et udgangspunkt. Til konkrete implementeringsspørgsmål kan jeg bruge [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/), og til mere detaljerede sikkerhedskrav og verifikation kan jeg bruge [OWASP ASVS](asvs.md).

Sammenhængen mellem ressourcerne er beskrevet i [OWASP-oversigten](README.md).

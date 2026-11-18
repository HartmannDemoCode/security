# Microsoft Security Development Lifecycle (SDL)

SDL er Microsofts tilgang til at integrere sikkerhed i softwareudvikling og DevOps. Den omfatter design, kode, build, deployment og drift og kan anvendes på tværs af platforme. [Microsofts introduktion til SDL](https://www.microsoft.com/en-us/securityengineering/sdl)

## Officielle ressourcer

Links kontrolleret 28. oktober 2026:

- [Microsoft Security Development Lifecycle – overblik](https://www.microsoft.com/en-us/securityengineering/sdl)
- [SDL Practices – de ti sikkerhedspraksisser](https://www.microsoft.com/en-us/securityengineering/sdl/practices)
- [Praksis 3: Sikkert design og trusselsmodellering](https://www.microsoft.com/en-us/securityengineering/sdl/practices/secure-by-design)

Praksisserne omfatter blandt andet sikkerhedskrav, gennemprøvede teknologier, trusselsmodellering, kryptografi, forsyningskæde, udviklingsmiljø, sikkerhedstest, drift, overvågning og uddannelse. Microsoft opdaterer dem løbende. [Kilde: SDL Practices](https://www.microsoft.com/en-us/securityengineering/sdl/practices)

## Hvordan vil jeg bruge SDL i valgfaget?

Jeg vil bruge SDL som ramme for et mindre udviklingsforløb. Mit forslag er:

1. Beskriv systemets værdier og sikkerhedskrav.
2. Lav en trusselsmodel med [STRIDE](stride.md).
3. Omsæt prioriterede trusler til konkrete krav, eventuelt med [OWASP ASVS](../owasp/asvs.md).
4. Implementér tiltag, og undersøg afhængigheder og opsætning.
5. Test kravene, og dokumentér fund og rettelser.
6. Beskriv, hvordan sikkerhedshændelser opdages og håndteres i drift.

Det virker relevant til at forbinde sårbarhedsanalyse med forebyggelse og udviklerens ansvar gennem hele forløbet. Jeg vil udvælge få aktiviteter, som de studerende kan gennemføre og dokumentere i deres projekt.

[Tilbage til Microsoft-oversigten](README.md).

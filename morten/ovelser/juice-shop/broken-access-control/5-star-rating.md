# 5-Star Feedback (⭐⭐)

**Kategori:** Broken Access Control (A01)  
**Dato:** 2026-11-21  
**Miljø:** OWASP Juice Shop i Docker, Chrome DevTools, Burp Suite Community  
**Status:** Under undersøgelse  
**Tidsforbrug:** Ca. 15 min.

## Mål

Undersøge, om en almindelig bruger kan slette feedback, selvom funktionen er beregnet til administratorer.

## Forventet adgangsregel

Kun administratorer skal kunne slette anmeldelser. Almindelige brugere må ikke have adgang til denne funktion.

## Fremgangsmåde

1. Jeg ændrede JWT-tokenet for at få adgang til administratorsiden.
2. På administratorsiden kunne jeg se alle anmeldelser.
3. Jeg fandt ud af, at anmeldelserne blev slettet via et API-kald.
4. Jeg brugte Burp Suite til at undersøge requesten og teste, om jeg kunne slette en anmeldelse.

## Blindgyder

Jeg forsøgte først at slette en anmeldelse med det ændrede JWT-token, men fik en HTTP 401-fejl med beskeden `invalid signature`.

Det betød, at serveren afviste tokenet, fordi signaturen ikke var gyldig.

## Request

Jeg fandt følgende request i Burp Suite:

```http
DELETE /api/Feedbacks/1 HTTP/1.1
Host: localhost:3000
```

Serveren svarede først med:

```http
HTTP/1.1 401 Unauthorized
```

Jeg ændrede derefter rollen tilbage til `customer` og sendte requesten igen.

## Resultat og bevis

Med rollen `customer` kunne requesten nu komme igennem.

Det tyder på, at API-endpointet ikke kontrollerer administratorrollen korrekt.

Hvis anmeldelsen faktisk bliver slettet, viser det, at en almindelig bruger kan udføre en handling, som kun burde være tilladt for administratorer.

## Manglende kontrol

Serveren ser ikke ud til at kontrollere, om brugeren har administratorrettigheder, når requesten sendes til dette endpoint.

Det er ikke nok kun at beskytte administrationssiden i frontend. API'et skal også kontrollere brugerens rettigheder.

## Konsekvens

Hvis almindelige brugere kan slette anmeldelser, kan de misbruge funktionen til at fjerne feedback fra andre brugere.

## Rettelse

Serveren skal kontrollere brugerens rolle ved hvert API-kald.

Kun brugere med administratorrettigheder må kunne slette anmeldelser. JWT-tokenets signatur og brugerens rettigheder skal valideres på serversiden.

## Overførsel til miseOS

Undersøg, om almindelige brugere kan udføre handlinger, der kun burde være tilgængelige for administratorer.

Relevant test: Vertikal adgangskontrol og Broken Access Control.

## Det jeg lærte

Jeg lærte at finde API-requests i Burp Suite og undersøge, hvordan serveren kontrollerer brugerens rettigheder.

Jeg lærte også, at man skal teste adgangskontrollen direkte mod API'et og ikke kun undersøge, hvad man kan se i frontend.
# Admin Section (⭐⭐)

**Kategori:** Broken Access Control (A01)  
**Dato:** 2026-11-21  
**Miljø:** OWASP Juice Shop i Docker, Chrome DevTools, Burp Suite Community  
**Status:** Løst  
**Hjælp brugt:** Ingen  
**Tidsforbrug:** Ca. 15 min.

## Mål

Få adgang til butikkens administrationsside.

## Forventet adgangsregel

Kun administratorer skal kunne tilgå admin-sektionen. Almindelige brugere må ikke have adgang.

## Fremgangsmåde

1. Jeg gik direkte til `/administration` i browseren, men fik en 403-fejl: "You are not allowed to access this page!"
2. Jeg åbnede DevTools med `F12` og gik ind under Sources.
3. Jeg brugte `Ctrl + Shift + F` og søgte efter `admin`.
4. Jeg fandt en JavaScript-funktion (`canActivate()`), som kontrollerer, om brugerens rolle er `admin`.
5. Jeg undersøgte tokenet under Application → Local Storage → token.
6. Jeg brugte Burp Suite til at undersøge requests og responses.

### JavaScript-kode jeg fandt

```javascript
canActivate() {
    let t = this.loginGuard.tokenDecode();
    return t?.data && t.data.role === $n.admin
        ? !0
        : (this.loginGuard.forbidRoute(), !1);
}
```

Funktionen kontrollerer, om brugeren har rollen `admin`, før administrationssiden åbnes.

### Blindgyder

Jeg prøvede først at åbne `/administration` direkte, men fik en 403-fejl. Jeg havde glemt at undersøge tokenet, som frontend bruger til at kontrollere brugerens rolle.

## Request

Jeg undersøgte JWT-tokenet og fandt brugerens rolle i payloaden:

```json
{
  "data": {
    "id": 26,
    "email": "morten@example.dk",
    "role": "admin"
  }
}
```

Jeg undersøgte, hvordan rollen blev brugt i frontendens adgangskontrol.

## Resultat og bevis

Jeg fandt admin-funktionen og dens rollecheck i JavaScript.

Jeg fik adgang til administrationssektionen og kunne dermed løse opgaven i Juice Shop.

## Manglende kontrol

Applikationen bruger en rollecheck i frontend. Serveren skal også kontrollere, at brugeren har administratorrettigheder.

## Konsekvens

Hvis serveren ikke kontrollerer brugerens rolle korrekt, kan almindelige brugere potentielt få adgang til administrative funktioner.

## Rettelse

Serveren skal kontrollere brugerens rolle ved hvert admin-kald og validere JWT-tokenets signatur.

Man må ikke stole på oplysninger fra browseren alene.

## Overførsel til miseOS

Undersøg, om almindelige brugere kan tilgå funktioner, der kun burde være tilgængelige for administratorer.

Relevant test: Vertikal adgangskontrol.

## Det jeg lærte

Jeg lærte at finde rollechecks i JavaScript med `Ctrl + Shift + F`.

Jeg lærte også, at frontendens adgangskontrol ikke er nok. Serveren skal selv kontrollere brugerens rettigheder.
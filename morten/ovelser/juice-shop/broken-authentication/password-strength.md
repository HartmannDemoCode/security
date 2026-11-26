# Password Strength (⭐⭐)

**Kategori:** Broken Authentication – A07:2021  
**Dato:** 2026-11-21  
**Miljø:** OWASP Juice Shop i Docker, Chrome DevTools, Burp Suite Community  
**Status:** Løst  
**Tidsforbrug:** Ca. 20 min.

## Mål

Logge ind med administratorens oprindelige loginoplysninger uden at ændre adgangskoden eller bruge SQL Injection.

## Forventet adgangsregel

Administratorens konto skal være beskyttet af en stærk adgangskode, som ikke kan gættes ved almindelige brute force-forsøg.

## Fremgangsmåde

1. Jeg åbnede Chrome DevTools med `F12` og undersøgte JavaScript-koden.
2. Jeg fandt en funktion, som indeholdt administratorens e-mailadresse.
3. Jeg fandt følgende e-mailadresse i koden: `admin@juice-sh.op`.
4. Jeg åbnede Burp Suite og gik til Proxy → HTTP history.
5. Jeg fandt login-requesten og sendte den til Intruder.
6. Jeg opsatte Intruder til at afprøve forskellige adgangskoder.
7. Jeg gennemgik resultaterne og fandt adgangskoden `admin123`.
8. Jeg brugte loginoplysningerne til at logge ind som administrator.

## JavaScript-kode jeg fandt

```javascript
text: `Enter the admin's email address into the **email field**.`,
fixture: `#email`,
resolved: u(`#email`, `admin@juice-sh.op`, {
  replacement: [`juice-sh.op`, `application.domain`]
})
```

Koden viste, at administratorens e-mailadresse var `admin@juice-sh.op`.

## Blindgyder

Jeg skulle først finde administratorens e-mailadresse, før jeg kunne begynde at teste adgangskoder.

Jeg undersøgte derfor frontendens JavaScript-kode i Chrome DevTools.

## Request

Jeg fandt login-requesten i Burp Suite og sendte den til Intruder.

Jeg brugte administratorens e-mailadresse og testede forskellige adgangskoder mod login-endpointet.

**Fundne loginoplysninger:**

```text
Email: admin@juice-sh.op
Password: admin123
```

## Resultat og bevis

Jeg fandt administratorens adgangskode ved hjælp af Burp Intruder.

Jeg kunne derefter logge ind med administratorens oprindelige loginoplysninger.

## Manglende kontrol

Administratorens adgangskode var for svag og kunne findes ved brute force.

Applikationen burde have bedre beskyttelse mod gentagne loginforsøg.

## Konsekvens

En angriber kan gætte en svag adgangskode og få adgang til en konto med høje rettigheder.

Hvis kontoen tilhører en administrator, kan det give adgang til følsomme oplysninger og administrative funktioner.

## Rettelse

- Kræv stærkere adgangskoder.
- Begræns antallet af loginforsøg.
- Indfør midlertidig låsning eller forsinkelse efter gentagne fejlede loginforsøg.
- Overvåg mistænkelige loginforsøg.
- Overvej multifaktorautentifikation (MFA).

## Overførsel til miseOS

Undersøg, om loginfunktionen beskytter mod gentagne adgangskodeforsøg.

Relevant test: Password Policy, Brute Force Protection og Authentication.

## Det jeg lærte

Jeg lærte at finde oplysninger i JavaScript-kode med Chrome DevTools.

Jeg lærte også at bruge Burp Intruder til at teste adgangskoder og se, hvordan en svag adgangskode kan føre til kompromittering af en administratorkonto.
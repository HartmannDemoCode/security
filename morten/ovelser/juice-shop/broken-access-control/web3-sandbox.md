# Web3 Sandbox (⭐⭐)

**Kategori:** Security Misconfiguration (A05:2021) – eksponeret udviklingsfunktion  
**Dato:** 2026-11-21  
**Miljø:** OWASP Juice Shop i Docker, Chrome DevTools, Burp Suite Community  
**Status:** Løst  
**Tidsforbrug:** Ca. 10-15 min.

## Mål

Find en ved en fejl eksponeret code sandbox, som gør det muligt at skrive, kompilere og deploye smart contracts direkte i browseren.

## Forventet adgangsregel

En funktion til udvikling og deployment af smart contracts bør ikke være tilgængelig for almindelige brugere i en produktionsapplikation, medmindre det er en tilsigtet funktion.

Udviklingsværktøjer bør være beskyttet eller fjernet fra den offentlige brugerflade.

## Fremgangsmåde

1. Jeg undersøgte Juice Shops frontend ved hjælp af Chrome DevTools.
2. Jeg åbnede DevTools med `F12` og gik ind under fanen **Sources**.
3. Jeg brugte `Ctrl + Shift + F` til at søge i JavaScript-filerne efter relevante ord og funktioner.
4. Jeg ledte efter referencer til en Web3 Sandbox eller en funktion til at skrive og deploye smart contracts.
5. Jeg fandt en side med en Contract Editor, hvor man kunne skrive Solidity-kode.
6. Jeg kunne se, at siden indeholdt funktioner til at vælge compiler-version, kompilere contracts, deploye dem og interagere med dem.

### Blindgyder

Jeg ledte først efter funktionen direkte i den almindelige brugerflade. Det var ikke nødvendigvis tydeligt, hvor den lå.

Ved at undersøge JavaScript-filerne i Sources kunne jeg finde spor til den skjulte funktion.

## Request

Der er ikke dokumenteret en bestemt HTTP-request i denne gennemgang. Opgaven blev løst ved at finde og åbne den eksponerede sandbox i frontend.

Derfor indsætter jeg ikke en opdigtet request eller et opdigtet svar.

## Resultat og bevis

Jeg fandt Web3 Code Sandbox med følgende funktioner:

- Contract Editor til at skrive Solidity-kode.
- Mulighed for at vælge compiler-version.
- Mulighed for at kompilere smart contracts.
- Mulighed for at deploye contracts.
- Mulighed for at kalde funktioner i de deployede contracts.

Jeg kunne se et eksempel på en Solidity-kontrakt:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.14;

contract HelloWorld {
    function get() public pure returns (string memory) {
        return 'Hello Contracts';
    }
}
```

Dette beviser, at sandboxen var tilgængelig fra applikationens frontend.

## Manglende kontrol

Applikationen eksponerede en udviklingsfunktion, som ikke burde være offentligt tilgængelig i en produktionsversion.

Det er primært en fejl i konfiguration og eksponering af funktionalitet frem for en klassisk Broken Access Control-fejl.

## Konsekvens

En bruger kan få adgang til udviklingsfunktioner, som normalt ikke er beregnet til almindelige brugere.

Hvis sandboxen er koblet til en rigtig blockchain eller et miljø med værdifulde ressourcer, kan der være yderligere risici ved deployment og interaktion med smart contracts.

## Rettelse

- Fjern udviklingsværktøjer fra produktionsversionen.
- Begræns adgangen til interne udviklingsmiljøer.
- Sørg for, at frontend ikke indeholder skjulte udviklingsfunktioner, som stadig er tilgængelige.
- Brug adgangskontrol på serveren, hvis funktionen skal være tilgængelig for bestemte brugere.

## Overførsel til miseOS

Undersøg, om miseOS har interne udviklingsværktøjer, testfunktioner eller administrationssider, som ved en fejl er eksponeret for almindelige brugere.

En tilsvarende test fra testmatrixen vil være kontrol af eksponerede endpoints, skjulte funktioner og adgang til interne værktøjer.

## Det jeg lærte

Jeg lærte, at frontendens JavaScript kan indeholde spor til funktioner, som ikke er synlige i den almindelige brugerflade.

Med `Ctrl + Shift + F` i Chrome DevTools kan jeg søge på tværs af JavaScript-filerne og finde interessante funktioner.

Jeg lærte også, at en skjult funktion ikke nødvendigvis er en beskyttet funktion.
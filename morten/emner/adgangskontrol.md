# Login, sessioner og adgangskontrol

Status: Ikke startet.

Her samler jeg det, jeg lærer om A01 Broken Access Control og A07 Authentication Failures, og det, jeg selv prøver af. Planen ligger i [forløbet](../forloeb/01-adgangskontrol.md), og vurderingen af materialerne ligger under [ressourcer](../ressourcer/adgangskontrol.md).

## Identitet og adgang

Mine forklaringer af loginforløb, sessioner og adgang til konkrete ressourcer. For hvert eksempel vil jeg skrive, hvilken kontrol der fejler, og om jeg undersøger A01 eller A07.

## Min forklaring af fejlen

Skrives efter læsning og afprøvning. Jeg vil forklare, hvilken adgang brugeren burde have, og hvad der går galt i applikationen.

## Afprøvninger

For hver øvelse skriver jeg miljø, fremgangsmåde, relevante requests og resultat. Jeg noterer også, hvis jeg bruger hints eller en løsning.

Jeg holder resultaterne fra IDOR og loginøvelsen adskilt, selv om de indgår i samme forløb.

## Sårbarhedsanalyse

### Fund og konsekvens

Hvad kan brugeren gøre, og hvilke data eller funktioner bliver berørt?

### Sådan viser jeg fejlen

Forudsætninger, request og resultat.

### Årsagen i koden

Hvilken kontrol mangler eller virker forkert?

### Rettelse i Java

Den relevante kode før og efter rettelsen og min forklaring af ændringen.

### Test af rettelsen

Hvad har jeg kørt, hvad forventede jeg, og hvad blev resultatet? Jeg skelner mellem det, jeg har afprøvet, og det, jeg kun har beskrevet.

## Det jeg stadig er i tvivl om

Spørgsmål fra læsningen og øvelserne.

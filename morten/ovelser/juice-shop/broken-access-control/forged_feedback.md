# Forged Feedback (⭐⭐⭐)

**Kategori:** Broken Access Control (A01)  
**Dato:** 2026-11-21  
**Miljø:** OWASP Juice Shop i Docker, Chrome DevTools  
**Status:** Løst   
**Tidsforbrug:** Ca. 15 min.

## Mål

Indsende feedback i en anden brugers navn.

## Forventet adgangsregel

Feedback skal knyttes til den bruger, der er logget ind. En almindelig bruger må ikke kunne indsende feedback på vegne af andre brugere.

## Fremgangsmåde

1. Jeg gik ind på Contact Us-siden i Juice Shop.
2. Jeg åbnede Chrome DevTools med `F12` og gik ind under Elements.
3. Jeg undersøgte HTML-koden for feedback-formularen.
4. Jeg fandt et skjult inputfelt med navnet `userId`.
5. Jeg fjernede attributten `hidden`, så feltet blev synligt på siden.
6. Jeg kunne nu se og ændre bruger-ID'et i feltet.
7. Jeg ændrede bruger-ID'et til en anden brugers ID og indsendte feedbacken.

## HTML-kode jeg fandt

```html
<input hidden id="userId" type="text">
```

Feltet er skjult i formularen, men det indeholder brugerens ID.

## Blindgyder

Jeg havde først ikke lagt mærke til, at formularen indeholdt et skjult felt.

Jeg undersøgte derfor HTML-koden i DevTools for at finde ud af, hvordan formularen bestemte, hvilken bruger feedbacken skulle tilhøre.

## Resultat og bevis

Jeg kunne ændre værdien i `userId`-feltet og indsende feedback i en anden brugers navn.

Juice Shop registrerede dermed udfordringen som løst.

## Manglende kontrol

Applikationen stoler på bruger-ID'et fra formularen i browseren.

Serveren kontrollerer ikke korrekt, om den indloggede bruger har tilladelse til at indsende feedback på vegne af den bruger, som ID'et tilhører.

## Konsekvens

En bruger kan indsende falsk feedback i en anden brugers navn. Det kan bruges til at skade en anden brugers omdømme.

## Rettelse

Serveren skal selv bestemme, hvilken bruger feedbacken tilhører, ud fra den godkendte session eller JWT-token.

Serveren må ikke stole på et bruger-ID, som kan ændres direkte i browseren.

## Overførsel til miseOS

Undersøg skjulte felter i formularer, og test om værdier som `userId` kan ændres af brugeren.

Relevant test: Manipulation af inputfelter og Broken Access Control.

## Det jeg lærte

Jeg lærte, at skjulte HTML-felter stadig kan ændres i browserens DevTools.

Jeg lærte også, at skjulte felter ikke er en sikkerhedsforanstaltning. Serveren skal altid kontrollere, om brugeren har tilladelse til den handling, der udføres.
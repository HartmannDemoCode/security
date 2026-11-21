# Put an additional product into another user's shopping basket (⭐⭐⭐)

**Kategori:** Broken Access Control (A01)  
**Dato:** 2026-11-21  
**Miljø:** OWASP Juice Shop i Docker, Burp Suite Community  
**Status:** Løst  
**Hjælp brugt:** Companion guide  
**Tidsforbrug:** Ca. 20 min.

## Mål

Tilføje et produkt til en anden brugers indkøbskurv uden at være logget ind som ejeren af kurven.

## Forventet adgangsregel

En bruger må kun kunne tilføje og ændre produkter i sin egen indkøbskurv. Serveren skal kontrollere, at kurven tilhører den indloggede bruger.

## Fremgangsmåde

1. Jeg tilføjede et produkt til min egen kurv og fandt requesten i Burp Suite under Proxy → HTTP history.
2. Jeg fandt en `PUT`-request til `/api/BasketItems/3`.
3. Jeg undersøgte JSON-bodyen og fandt felterne `ProductId`, `BasketId` og `quantity`.
4. Jeg forsøgte at ændre `BasketId` fra min egen kurv til en anden brugers kurv.
5. Serveren afviste ændringen med en HTTP 400-fejl.
6. Jeg undersøgte derefter muligheden for at oprette en ny basket item med en `POST`-request.
7. Jeg forsøgte at bruge to `BasketId`-felter i samme JSON-body. Dette er kendt som HTTP Parameter Pollution (HPP).

## Blindgyder

Mit første forsøg var at ændre `BasketId` direkte i en `PUT`-request.

Serveren svarede:

```json
{
  "message": "null: `BasketId` cannot be updated due `noUpdate` constraint"
}
```

Det betyder, at serveren har en begrænsning, som forhindrer direkte ændring af `BasketId`.

Jeg prøvede derefter med en `POST`-request, men fik en HTTP 500-fejl med `SequelizeUniqueConstraintError`.

Fejlen viste, at det valgte produkt allerede fandtes i kurven.

## Request

**Normal request:**

```http
PUT /api/BasketItems/3 HTTP/1.1
Host: localhost:3000
```

Relevant JSON-body:

```json
{
  "quantity": 2,
  "ProductId": 3,
  "BasketId": 6,
  "id": 3
}
```

**Forsøg med ændret BasketId:**

```json
{
  "quantity": 2,
  "ProductId": 3,
  "BasketId": 4,
  "id": 3
}
```

Serveren afviste ændringen, fordi `BasketId` ikke måtte opdateres.

**Forsøg med POST og HTTP Parameter Pollution:**

```json
{
  "ProductId": 14,
  "BasketId": "6",
  "quantity": 1,
  "BasketId": "4"
}
```

Her forsøgte jeg at bruge to forskellige `BasketId`-værdier i samme request.

## Resultat og bevis

Jeg fandt ud af, at `BasketId` ikke kunne ændres direkte med en PUT-request.

Jeg undersøgte derfor HTTP Parameter Pollution som en alternativ metode til at forsøge at tilføje et produkt til en anden brugers kurv.

Det sidste forsøg blev afvist af databasen på grund af eksisterende unikke værdier. En vellykket tilføjelse er endnu ikke bekræftet.

## Manglende kontrol

Applikationen skal kontrollere, at brugeren kun kan tilføje produkter til sin egen kurv.

Serveren skal validere ejerskabet af kurven ved oprettelse af nye basket items og må ikke stole på `BasketId` fra klienten.

## Konsekvens

Hvis kontrollen mangler, kan en bruger muligvis tilføje produkter til en anden brugers kurv.

Det kan skabe forvirring og manipulere en anden brugers indkøbskurv.

## Rettelse

Serveren skal hente brugerens kurv ud fra den godkendte session eller JWT-token.

Den må ikke acceptere et vilkårligt `BasketId` fra requesten uden at kontrollere ejerskabet.


## Overførsel til miseOS

Undersøg funktioner, hvor brugere kan tilføje eller ændre elementer i personlige lister, kurve eller andre brugerbundne data.

Relevant test: Horisontal adgangskontrol og manipulation af objekt-ID'er.

## Det jeg lærte

Jeg lærte at undersøge API-requests i Burp Suite og se, hvilke felter serveren accepterer.

Jeg lærte også om `noUpdate`-begrænsninger og HTTP Parameter Pollution (HPP), hvor gentagne parametre kan give uventet opførsel på serveren.
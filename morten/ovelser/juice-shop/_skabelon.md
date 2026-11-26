# <Opgavens navn> (⭐⭐)

**Kategori:** Broken Access Control (A01)  
**Dato:** ÅÅÅÅ-MM-DD  
**Miljø:** Juice Shop vX.Y.Z i Docker, Burp Suite Community  
**Status:** Løst / Delvist / Ikke løst  
**Hjælp brugt:** Ingen / Hint i Score Board / Companion guide / Løsning  
**Tidsforbrug:** ca. X min

## Mål

Hvad opgaven går ud på, med mine egne ord. Én eller to linjer.

## Forventet adgangsregel

Hvem burde have lov til hvad? Fx: "Kun ejeren af kurven må se dens indhold."

## Fremgangsmåde

1. Hvilken funktion jeg brugte i appen, og hvad jeg ledte efter.
2. Hvilken request jeg fandt i Burp (Proxy → HTTP history).
3. Hvad jeg ændrede. Jeg ændrer én ting ad gangen.
4. Hvad der skete.

Blindgyder: Det jeg prøvede, som ikke virkede, og hvorfor.

## Request

Normal request:

```http
GET /rest/basket/1 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <token>
```

Ændret request:

```http
GET /rest/basket/2 HTTP/1.1
Host: localhost:3000
Authorization: Bearer <token>
```

Svar (kun det relevante):

```json
{ "status": "success", "data": { "id": 2, "UserId": 2 } }
```

Jeg fjerner tokens, cookies og passwords, før jeg committer.

## Resultat og bevis

Hvad jeg fik adgang til, og hvordan jeg ved, at opgaven er løst. Fx et skærmbillede eller beskeden i Score Board.

## Manglende kontrol

Hvilken kontrol serveren burde have lavet. Er fejlen horisontal (en anden bruger) eller vertikal (en højere rolle)?

## Konsekvens

Hvad en angriber kan gøre med fejlen i en rigtig applikation.

## Rettelse

Hvordan fejlen kunne rettes. Gerne en kort kodeskitse.

## Overførsel til miseOS

Findes der en tilsvarende funktion i miseOS? Hvilken test fra testmatrixen svarer den til?

## Det jeg lærte

Én til tre linjer.

# Login Jim (⭐⭐⭐)

**Kategori:** Injection – A05  
**Dato:** 2026-11-26  
**Miljø:** OWASP Juice Shop i Docker, Burp Suite Community  
**Status:** Løst  
**Tidsforbrug:** ca. 5 min

## Mål

Logge ind som Jim uden at kende hans adgangskode.

## Fremgangsmåde

1. Jeg fandt Jims e-mail, `jim@juice-sh.op`, under en anmeldelse på et produkt på forsiden.
2. I login skrev jeg e-mailen efterfulgt af `'--`, så resten af SQL-sætningen, inklusive tjekket af adgangskoden, blev kommenteret ud.
3. Adgangskoden kunne være hvad som helst, og jeg blev logget ind som Jim.

```text
Email: jim@juice-sh.op'--
Password: hvadsomhelst
```

## Manglende kontrol

Serveren bygger SQL-sætningen ved at sætte input direkte ind i teksten, så `'` afslutter strengen og `--` fjerner resten:

```sql
SELECT * FROM Users WHERE email = 'jim@juice-sh.op'--' AND password = '...'
```

## Rettelse

Parameteriserede queries, så input aldrig bliver en del af SQL-koden.

## Overførsel til miseOS

Ikke relevant. miseOS bruger Hibernate (JPA), og alle queries bruger navngivne parametre med `setParameter`. Der er ingen native SQL. Det eneste sted, hvor tekst sættes sammen, er `ORDER BY` i `DishSuggestionDAO`, og her vælges kolonnen fra en fast liste i en `switch`, ikke fra brugerens input. Inputvalidering er et ekstra lag, men parametrene er det, der beskytter.

## Det jeg lærte

E-mails, der vises offentligt (her i en anmeldelse), giver angriberen halvdelen af loginoplysningerne. Med SQL injection er den anden halvdel unødvendig.

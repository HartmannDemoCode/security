# Login Bender (⭐⭐⭐)

**Kategori:** Injection – A05  
**Dato:** 2026-11-26  
**Miljø:** OWASP Juice Shop i Docker, Burp Suite Community  
**Status:** Løst  
**Tidsforbrug:** ca. 2 min

## Mål

Logge ind som Bender uden at kende hans adgangskode.

## Fremgangsmåde

Samme løsning som [Login Jim](login-jim.md):

1. Jeg fandt Benders e-mail, `bender@juice-sh.op`, i en anmeldelse på forsiden.
2. I login skrev jeg e-mailen efterfulgt af `'--`, så tjekket af adgangskoden blev kommenteret ud.
3. Adgangskoden kunne være hvad som helst.

```text
Email: bender@juice-sh.op'--
Password: hvadsomhelst
```

## Manglende kontrol og rettelse

Samme fejl som i Login Jim: serveren sætter input direkte ind i SQL-sætningen. Rettelsen er parameteriserede queries.

## Overførsel til miseOS

Ikke relevant, af samme grund som i Login Jim. miseOS bruger Hibernate med parametre.

## Det jeg lærte

Når et angreb virker på én konto, virker det på dem alle, så snart jeg kender e-mailen. Anmeldelser på forsiden gav mig e-mails til begge.

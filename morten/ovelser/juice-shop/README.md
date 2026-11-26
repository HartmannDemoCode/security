# Juice Shop: noter til opgaverne

Jeg skriver én note pr. opgave, så jeg kan vise fejlen igen og bruge den i rapporten. Juice Shop er forberedende træning. Undersøgelserne til rapporten handler om miseOS.

## Sådan bruger jeg mappen

1. Jeg kopierer [_skabelon.md](_skabelon.md) til en ny fil, fx `admin-section.md`.
2. Jeg skriver noten, mens jeg løser opgaven, ikke bagefter. Requests kopierer jeg direkte fra Burp.
3. Jeg opdaterer tabellen nedenfor.
4. Når en session er slut, tager jeg en backup fra Score Board. Juice Shop nulstiller sine data, når containeren genstarter.

Jeg gemmer ikke backupfilen i repoet. Tokens og cookies fjerner jeg fra noterne.

## Broken Access Control

Listen følger Score Board i min version af Juice Shop.

| Opgave | Stjerner | Status | Note |
| --- | --- | --- | --- |
| Web3 Sandbox | ⭐ | | |
| Admin Section | ⭐⭐ | | |
| AI Debugging | ⭐⭐ | | |
| Five-Star Feedback | ⭐⭐ | | |
| View Basket | ⭐⭐ | | |
| CSRF | ⭐⭐⭐ | 🟡 | [csrf.md](broken-access-control/csrf.md) -- blokeret af browserens SameSite-adfærd, se noten |
| Forged Feedback | ⭐⭐⭐ | | |
| Forged Review | ⭐⭐⭐ | | |
| Manipulate Basket | ⭐⭐⭐ | | |
| Product Tampering | ⭐⭐⭐ | | |
| Easter Egg | ⭐⭐⭐⭐ | | |

Status: ✅ løst, 🟡 delvist, ❌ ikke løst. Note: link til notefilen.

## Mønstre på tværs af opgaverne

Her samler jeg det, der går igen, fx "serveren stoler på et ID fra klienten". Det bliver grundlaget for min forklaring i rapporten.

- 

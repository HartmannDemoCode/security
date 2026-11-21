# CSRF (⭐⭐⭐)

**Kategori:** Broken Access Control (A01:2021) -- Cross-Site Request Forgery  
**Dato:** 2026-11-21  
**Miljø:** OWASP Juice Shop i Docker, en separat lokal webserver til at simulere "en anden origin"  
**Status:** Ikke løst -- blokeret af en browser-beskyttelse, se "Det jeg lærte"  
**Hjælp brugt:** Companion guide-hint om at bruge en ekstern HTML-side (fx htmledit.squarefree.com)  
**Tidsforbrug:** Ca. 45 min. (meste af tiden gik med at fejlsøge, hvorfor angrebet ikke virkede)

## Mål

Ændre en logget ind brugers navn ved at få en request til at blive sendt fra en **anden origin** end Juice Shop selv -- altså uden at bruge Juice Shops egen "Edit Profile"-formular.

## Forventet adgangsregel

Kun brugeren selv bør kunne ændre sit profilnavn, og kun ved en handling de bevidst har udført på Juice Shop selv. En anden hjemmeside bør ikke i baggrunden kunne få min browser til at sende en request til Juice Shop, som udfører en ændring på mine vegne.

## Sådan er opgaven bygget

Juice Shops profilside (`POST /profile`) genkender den indloggede bruger ud fra en `token`-cookie, ikke ud fra en header, jeg selv skal sætte. Browseren sender cookies automatisk med, uanset hvilken side en formular blev afsendt fra. Der er ikke noget CSRF-token i formularen, og serveren tjekker ikke, om requesten rent faktisk kom fra Juice Shop selv.

Det betyder, at en formular på en helt anden hjemmeside kan sende en POST til `/profile`, og min browser sender automatisk min login-cookie med -- uden at jeg har bedt om det.

## Fremgangsmåde

1. Jeg logger ind i Juice Shop i min almindelige browser.
2. Jeg laver en HTML-side med en skjult formular, der peger på `http://localhost:3000/profile`, og som sender sig selv af sted med det samme via JavaScript.
3. Jeg åbner den side fra en **anden origin** end Juice Shop (en anden port eller en helt anden hjemmeside), mens jeg stadig er logget ind i Juice Shop i samme browser.
4. Formularen afsender en POST til Juice Shop. Min browser sender login-cookien automatisk med, så serveren tror, det er mig, der har bedt om ændringen.

### Blindgyder

1. Jeg prøvede først at køre HTML'en gennem htmledit.squarefree.com's indbyggede "live preview". Det gav en 500-fejl (`Blocked illegal activity`), fordi preview'et render'er siden i en sandboxed iframe, og min login-cookie derfor ikke blev sendt med.
2. Jeg lagde herefter filen på en lokal webserver (`python -m http.server`) på en anden port, så det var en rigtig cross-origin topnavigation og ikke en sandboxed iframe. Det løste ikke problemet -- samme fejl, `Blocked illegal activity`.
3. Jeg fik undervejs også en ren `ERR_CONNECTION_REFUSED` og en `404`, som skyldtes mine egne fejl (serveren ikke startet/forkert port, og et stavefejlsramt filnavn). De er rettet undervejs og er ikke en del af selve sikkerhedsproblemet.

Efter at have rettet alt det praktiske fik jeg **stadig** samme 500-fejl. Det peger på en mere grundlæggende årsag end iframe-sandboxing eller forkerte URL'er: se forklaringen nedenfor.

## Request

HTML-siden, der udløser angrebet:

```html
<html>
  <body>
    <form action="http://localhost:3000/profile" method="POST">
      <input type="hidden" name="username" value="CSRF account take over" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

Den resulterende request (set fra Burp):

```http
POST /profile HTTP/1.1
Host: localhost:3000
Cookie: token=<min login-cookie, sendt automatisk af browseren>
Content-Type: application/x-www-form-urlencoded

username=CSRF+account+take+over
```

## Manglende kontrol

Serveren har ingen CSRF-token og ingen kontrol af `Origin`/`Referer`. Den stoler alene på, at en gyldig session-cookie er nok til at udføre en ændring -- uanset hvilken side der bad om den.

## Konsekvens

En angriber kan lokke en logget ind bruger til at besøge en ondsindet side, der i baggrunden ændrer deres profil. I en rigtig applikation kunne det samme mønster bruges til mere alvorlige ændringer, fx skift af e-mail eller password, hvis de endepunkter er beskyttet på samme måde.

## Rettelse

- Et CSRF-token i formularen, som serveren validerer ved hver ændring.
- `SameSite=Strict` eller `Lax` sat eksplicit på session-cookien af serveren, i stedet for at stole på browserens standardværdi.
- Et tjek af `Origin`-headeren for følsomme handlinger.

## Overførsel til miseOS

Undersøg, om miseOS har handlinger, der kun er beskyttet af en session-cookie uden CSRF-token -- fx ændring af profiloplysninger eller andre indstillinger, der ikke kræver en ny godkendelse.

## Det jeg lærte

Jeg lærte, at `SameSite`-cookien er relevant for, om et CSRF-angreb virker. Juice Shop sætter sin `token`-cookie uden et eksplicit `SameSite`-flag, og moderne Chrome behandler den så som `SameSite=Lax`, hvilket normalt blokerer cookien på et cross-site POST.

Jeg lærte også, at **hvordan** jeg åbner den ondsindede side har betydning: en sandboxed iframe (som i htmledit.squarefree.com's live preview) sender ikke cookien med, selvom en rigtig topnavigation (fx via en lille lokal webserver på en anden port) normalt gør.

Chrome har dog en kendt midlertidig undtagelse, kaldet "Lax+POST": en cookie uden `SameSite`-flag bliver stadig sendt på et cross-site POST, men **kun i de første ca. 2 minutter**, efter cookien blev sat (dvs. lige efter login). Min bedste forklaring på, at angrebet blev ved med at fejle, er, at der gik for lang tid mellem mit login og mine forsøg -- al fejlsøgningen undervejs (forkert port, stavefejl i filnavn, en gammel server der blokerede porten) betød, at jeg altid var uden for det vindue, når requesten endelig nåede frem.

Konklusionen er stadig den samme, selvom jeg ikke fik vist det i praksis: Juice Shop har ingen CSRF-beskyttelse i applikationen selv (intet token, intet `Origin`-tjek). Det eneste, der reelt beskytter mod angrebet her, er en tidsbegrænset standardadfærd i browseren -- ikke noget, Juice Shop selv har bygget ind. Det er præcis den slags skrøbelig beskyttelse, man ikke bør stole på i en rigtig applikation.

## Næste skridt

- Prøve igen med login og angreb udført lige efter hinanden (inden for ca. et minut), uden fejlsøgning imellem, for at se om det bekræfter 2-minutters-teorien.
- Alternativt undersøge, om min version af Chrome overhovedet stadig har "Lax+POST"-undtagelsen, da Chromium selv kalder den midlertidig og har varslet, at den fjernes.
- Jeg går videre til næste opgave nu og vender eventuelt tilbage til denne.

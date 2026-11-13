# Ressourcer

*Dette dokument har til formål at udforske forskellige læringsressourcer - Ressourcer med relevans vil få deres eget dokument*

## Mulige / interessante læringressourcer:

- *TryHackMe*, THM (se tidligere indlæg for holdninger)
- *Hack the Box*, HTB
- *HackerDNA*, HDNA - fundet i forbindelse med sammenligning af THM vs HTB - er ikke "tried-and-tested"
- *Campfire Security*, Et dansk alternativ udviklet af AAU
- OWASP native træning - Webgoat er java baseret og rammer alle de 10 mest kritiske vulnerabilities
- CompTia certificat træning - en industristandard hvor at man kunne kigge efter indholdet i læringmaterialet uden at betale for et certifikat (Toby ved meget om det her og sidder inde med en del af materialet)

---

## Try Hack Me - THM

---

Jeg har forsøgt THM og har pt følgende tanker:

### Negativ:
- Hver path slutter med en retrospektiv gennemgang i form af en test. Testen var ikke opdateret i forhold til resten af materialet og gennemgik stof der ikke var blevet undervist. Det er en smule frustrerende. 
- Jeg føler hurtigt at jeg rammer en “paywall”, og hurtigere end jeg havde regnet med. Hvor jeg troede at enkelte moduler var gratis kan det godt differentiere inde enkelte rum i modulerne og det er ikke helt transparent. 
- Paywall’en optræder i nogle af de helt tidlige moduler der er til for at skabe en fundamental forståelse for faget. Skal vi anvende THM skal dette enten dækkes i timen eller håndteres på anden vis. 
    - Senere moduler bygger på viden fra tidligere der er gemt bag Paywall.
- Jeg bryder mig generelt ikke om læringsformen (jeg har ikke forsøgt mange rum og det kan ændres). Det kan sammenlignes med Duolingo og jeg tror ikke selv jeg kan anvende den viden de forsøger at give videre når man bliver holdt så meget i hånden - der sker ikke nogen kobling som ved research -> forståelse -> gennembrud. Indtil nu har det været meget “Gør præcis sådan her og sendt svaret”.
- Der er en tilhørende mobil app hvis primære formål er at understøtte undervisningen i form af flash cards. Appen giver umiddelbart ingen værdi. 

### Positiv:
- Alle kan følge med, det her kan umuligt være for svært for nogen.
- Det allerførste modul giver et hurtigt overblik over “Security” med rød, blå og mulige karriereveje. 

Jeg vil prøve at undersøge hvilke moduler der kan give mening herfra og ikke fokusere på hullerne da jeg allerede nu tænker at TryHackMe ikke kan bære undervisningen alene.

**UPDATE:** SECTION 1 af *Pre Security* Learning path indeholder tre moduler under categorien *Introduction to Cyber Security*.
Disse tre moduler er alle gratis og kan gennemføres med en bruger med free-tier.
De tre moduler er:

1. Offensive Security Intro
2. Defensive Security Intro
3. Careers in Cyber

Disse er enormt grundlæggende men vil fungere godt som det første læringsmateriale en elev møder på valgfaget - dette for at danne overblik.

**UPDATE:** Dette supplerer Campfires introduktions learning path godt og bør stadigvæk tages som det første.

--- 

Denne her sætter THM og HTB fint op: [Link](https://hackerdna.com/blog/tryhackme-vs-hackthebox)

**Fra artiklen:**

#### Computer Science Students

CS students typically understand programming and basic systems concepts but lack security-specific knowledge. You don't need explanations of what a server is, but you do need to learn attack methodologies and security tools.

TryHackMe's structured paths might feel slow. Hack The Box's machines might feel overwhelming without security fundamentals. Neither platform perfectly serves this audience.

**Recommendation:** Look for platforms that assume technical competence while teaching security concepts efficiently.

#### The Real-World Gap

Both platforms focus heavily on technical exploitation while underemphasizing the full penetration testing lifecycle. Real security work involves scoping, documentation, client communication, and remediation guidance. Neither platform adequately prepares you for these professional realities.

---

## Hack The Box - HTB

Meget mere teknisk end THM. 
Kræver enten en udførlig instruks i Virtuelle Maskiner på kurset; En bruger med "free-tier" har et fast timeslot (1-2 timer?) MEN kan kun åbne VM én gang i døgnet.
Lukker man derfor sin VM for at spare på minutterne kan denne ikke åbnes før dagen efter.

Brug af HTB er ikke på samme måde intuitivt og der skal en del mere styring til fra undervisers side.

For at HTB skal fungere optimalt skal bruger opsætte Linux i VM på egen maskine. Dette er teknisk, men ikke urealistisk - Der findes mange udførlige guides, bl.a. denne: [NetworkChuck](https://youtu.be/wX75Z-4MEoM?si=nu2k3QEkE2mEemrm)
NetworkChuck gør brug af VirtualBox: https://www.virtualbox.org/

Dette kan være en spændende og udfordrende øvelse for elever og vil give en god forståelse for Linux som værktøj, dog sætter det nogle åbenlyse hardware udfordringer da nogle elever kan have maskiner der ikke honorerer kravene.

---

## Campfire Security

Interessant dansk alternativ med gratis adgang for studerende (2 timers VM).

Kan fungere som introduktion og samlepunkt for viden til at opfylde vidensmål.
# Materiale om afhængigheder og sikker opsætning

Status: Skal undersøges.

Her vurderer jeg materialet. Arbejdsplanen ligger i [forløbet](../forloeb/02-afhaengigheder.md), og mine fund ligger i [de faglige noter](../emner/afhaengigheder.md).

## OWASP

- [A03:2025 – Software Supply Chain Failures](https://top10.owasp.org/2025/A03_2025-Software_Supply_Chain_Failures/)
- [A02:2025 – Security Misconfiguration](https://top10.owasp.org/2025/A02_2025-Security_Misconfiguration/)
- [A06:2021 – Vulnerable and Outdated Components](https://top10.owasp.org/2021/A06_2021-Vulnerable_and_Outdated_Components/)

Jeg bruger 2025-udgaven som udgangspunkt. Linket til 2021 er med, så jeg kan genkende kategorien i ældre undervisningsmateriale.

Jeg læser A03 og A02 sammen for at undersøge både komponenterne og den måde, de sættes op på. Jeg vil finde én fejlkonfiguration, der kan vises og rettes i samme lokale case som afhængigheden.

Min vurdering efter læsningen:

## GitHubs dokumentation om Dependabot

- [Dependabot alerts](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-alerts)
- [Dependabot security updates](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-security-updates)
- [Dependabot version updates](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-version-updates)
- [Konfigurationsmuligheder](https://docs.github.com/en/code-security/reference/supply-chain-security/dependabot-options-reference)
- [Understøttede økosystemer](https://docs.github.com/en/code-security/reference/supply-chain-security/supported-ecosystems-and-repositories)

Jeg vil vurdere, om dokumentationen er nok til at lave en lille øvelse, og hvor en studerende vil have brug for ekstra forklaring eller hjælp til GitHub-indstillingerne.

Min vurdering efter brug:

## PHP2Shell som case

- [Wikipedia – det link, jeg tog udgangspunkt i](https://en.wikipedia.org/wiki/PHP2Shell)
- [PHP Foundations forklaring fra 21. oktober 2026](https://thephp.foundation/blog/2026/10/21/todays-php-security-release-and-how-were-strengthening-php-security/)
- [PHP-projektets sikkerhedsmeddelelse](https://www.php.net/archive/2026.php#2026-10-21-5)

Jeg vil bruge projektets egne beskrivelser til at undersøge, hvad der fejlede, hvad der blev rettet, og hvordan brugere af PHP får rettelserne i drift. Jeg vil især vurdere, om sagen gør forskellen mellem biblioteker og runtime tydelig.

Min vurdering efter læsningen:

## Praktiske eksempler på sårbare biblioteker

### WebGoat – Vulnerable Components

- [WebGoat](https://owasp.org/projects/webgoat)
- [Koden til Vulnerable Components](https://github.com/WebGoat/WebGoat/tree/main/src/main/java/org/owasp/webgoat/lessons/vulnerablecomponents)

WebGoat har en Java-øvelse med XStream under Vulnerable Components. Jeg vil afprøve den og undersøge, hvilken version og opsætning den kræver. Jeg vil også undersøge, om jeg kan vise en rettelse i samme miljø. Jeg har ikke kørt øvelsen endnu.

Min vurdering efter afprøvningen:

### Snyk Learn – Log4Shell

- [Log4Shell-gennemgangen](https://learn.snyk.io/lesson/log4shell/)
- [Apaches sikkerhedsmeddelelser om Log4j](https://logging.apache.org/security.html)

Gennemgangen bruger en Java-applikation til at forklare angrebet, årsagen i logging-biblioteket og forebyggelsen. Jeg vil bruge den som introduktion og undersøge, hvilke dele jeg selv kan afprøve i et lokalt øvelsesmiljø. Det er ikke på forhånd afklaret, at siden giver mig et færdigt, kørende lab.

Teksten indeholder historiske versionsanbefalinger. Jeg bruger Apaches egne sikkerhedsmeddelelser til at kontrollere den konkrete sårbarhed og rettelsen.

Min vurdering efter gennemgangen:

### Juice Shop som alternativ

[Vulnerable Components i Juice Shop](https://help.owasp-juice.shop/part2/vulnerable-components.html)

Juice Shop har også opgaver om sårbare komponenter. Opgaven Vulnerable Library handler om at identificere og rapportere bibliotekets navn og version. Hvis jeg vil demonstrere selve udnyttelsen, skal jeg vælge en opgave, der også viser konsekvensen.

Min vurdering, hvis jeg afprøver det:

## Hvad ville jeg bruge i undervisningen?

Skrives efter læsning og afprøvning. Jeg vil vælge en kort introduktion, en konkret case og materiale til at afprøve en opdatering.

## Ekstra læsning og lyd

I [ekstra materiale](ekstra-materiale.md) ligger Googles Know, Prevent, Fix, bogkapitlet Deploying Code, OWASPs Docker-reference og podcastafsnit om Log4Shell og WannaCry. Jeg kan vælge det, der hjælper med mine spørgsmål fra øvelserne.

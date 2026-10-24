# Afhængigheder, opdateringer og sikker opsætning

Status: Ikke startet.

Her samler jeg mine faglige noter og afprøvninger. [Planen](../forloeb/02-afhaengigheder.md) og [vurderingen af materialet](../ressourcer/afhaengigheder.md) ligger hver for sig.

## Hvor hører det til i OWASP?

Min undersøgelse af afhængigheder og opdateringer tager udgangspunkt i [A03:2025 – Software Supply Chain Failures](https://top10.owasp.org/2025/A03_2025-Software_Supply_Chain_Failures/). Kategorien omfatter blandt andet sårbare tredjepartskomponenter og problemer i måden, software bygges, distribueres og opdateres på.

Jeg undersøger også [A02:2025 – Security Misconfiguration](https://top10.owasp.org/2025/A02_2025-Security_Misconfiguration/). Jeg bruger samme lokale case til at skelne mellem en sårbar komponent og en usikker indstilling.

## PHP2Shell

[PHP Foundation beskriver](https://thephp.foundation/blog/2026/10/21/todays-php-security-release-and-how-were-strengthening-php-security/) to fejl i PHP-runtime'ens hukommelses- og fejlhåndtering, CVE-2026-109618 og CVE-2026-109619. Under de rette betingelser kan en angriber uden login få kørt kode på serveren.

Sagen viser, at jeg også skal undersøge runtime-opdateringer. En opdatering af applikationens biblioteker opdaterer ikke automatisk en PHP-installation, som hostingudbyderen eller serverens pakkehåndtering leverer.

Min gennemgang af sagen og opdateringsvejen:

## Dependabots funktioner

- [Alerts](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-alerts) gør opmærksom på kendte sårbarheder i registrerede afhængigheder.
- [Security updates](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-security-updates) forsøger at oprette PR'er med rettelser til sårbare afhængigheder.
- [Version updates](https://docs.github.com/en/code-security/concepts/supply-chain-security/dependabot-version-updates) opretter opdaterings-PR'er, også når der ikke er en kendt sårbarhed.

En PR skal stadig vurderes, testes og sættes i drift. En scanning uden advarsler er ikke en garanti mod ukendte sårbarheder eller nye ondsindede ændringer.

## Mit projekt og dets afhængigheder

Projekt, stack, direkte og indirekte afhængigheder og den runtime, der bruges.

## Udkast til konfiguration

Skriv konfiguration og nødvendige GitHub-indstillinger her. Notér, hvad der er et udkast, og hvad der faktisk er afprøvet.

## Min afprøvning af en opdatering

Afhængighed, version før og efter, begrundelse, relevante advisories, tests og resultat.

## Min afprøvning af en fejlkonfiguration

Miljø, indstilling, request og konsekvens før ændringen. Derefter den nye indstilling og resultatet af samme request. Notér, om fejlen blev rettet gennem konfiguration, opdatering eller begge dele.

## Hvordan får jeg rettelsen i drift?

Beskriv vejen fra opdatering til build, deployment og kontrol af den kørende version. Notér også, hvad hostingudbyder eller serveradministrator skal håndtere.

## Det jeg stadig er i tvivl om

Spørgsmål fra læsning og afprøvning.

# Linux
Linux bruges som platform for testmiljøet, fordi de fleste sikkerhedsværktøjer er udviklet til Linux, og fordi 3. semester projekter selv driftes i Linux-baserede Docker-containere. Et Linux-baseret testmiljø ligner derfor målsystemet og gør det nemmere at beskrive opsætningen, så den kan genskabes.

## Virtual Machine
**Fordele:**
- Fuld isolation fra host-maskinen, så værktøjer, malware-samples og fejlkonfigurationer ikke påvirker det primære system.
- Snapshots gør det muligt at rulle tilbage til en kendt, ren tilstand, hvilket gør miljøet reproducerbart mellem øvelser.
- Netværket kan styres præcist (NAT, Host-only, Internal Network), så der kan bygges et lukket lab med fx Kali som angriber og en sårbar VM som mål.
- VM'en kan eksporteres som `.ova` og deles, så andre kan genskabe samme opsætning.

**Ulemper:**
- Kræver mange ressourcer (CPU, RAM, disk), da hver VM kører et fuldt OS oven på host-systemet (ved type 2 Hypervisor).
- Lavere ydeevne end native installation, især for grafik og I/O-tunge opgaver.
- Manuel opsætning i VirtualBox er ikke i sig selv reproducerbar – uden snapshots, eksport eller et IaC-værktøj som Vagrant kan miljøet "drifte" over tid.
- USB- og trådløs netværksadgang (fx WiFi-adaptere til pentest) kræver ekstra konfiguration og virker ikke altid stabilt.

**Valg:** VirtualBox med Kali Linux – gratis, kører på tværs af platforme, og Kali leverer færdige VirtualBox-images med de fleste sikkerhedsværktøjer forudinstalleret. Snapshots bruges til at nulstille miljøet.

### Specifikationer

#### Minimumskrav
- **CPU:** 64-bit (amd64) med hardwarevirtualisering (Intel VT-x / AMD-V) slået til i BIOS/UEFI
- **RAM:** min. 2 GB til Kali + min. 4 GB i overskud til host og Docker-containere
- **Storage:** 80 GB virtuel disk (dynamisk allokeret, fylder mindre fra start og vokser ved brug)

#### Opsætning
- **CPU:** 64-bit med 4 cores allokeret
- **RAM:** 8 GB 
- **Storage:** Default

### Type 1 Hypervisor
Installeres direkte på hardware

En type 1 hypervisor er selv styresystemet på maskinen. Den installeres direkte på hardwaren uden et host-OS imellem og administreres typisk fra en anden computer via en webbrugerflade. Det giver bedre ydeevne og isolation mellem VM'erne, men maskinen kan ikke samtidig bruges som almindelig arbejdscomputer. Det ligger uden for projektet, fordi det kræver en dedikeret maskine, og fordi testmiljøet skal kunne genskabes på en almindelig studiecomputer. Bemærk, at Windows 11 selv bruger Hyper-V (type 1), når WSL2 eller Docker Desktop er slået til, og så kører VirtualBox oven på Windows' hypervisor, hvilket kan sænke ydeevnen.

#### VMware

VMwares type 1 hypervisor, ESXi, er udbredt i virksomheders datacentre og kan sammen med vCenter samle flere servere i et cluster med fx live-migrering af VM'er og central administration. Den er ikke valgt, fordi den er bygget til virksomheder: efter Broadcoms opkøb sælges den primært som abonnement, den gratis udgave er stærkt begrænset, og den understøtter kun hardware på VMwares kompatibilitetsliste, så almindelige forbruger-netkort ofte ikke virker. Den er dermed mindre tilgængelig for studerende og hjemmelabs end Proxmox.

[**vm**ware](https://www.vmware.com/resources)

#### Proxmox

Proxmox VE er en gratis open source type 1 hypervisor baseret på Debian, som kan køre både fulde VM'er (KVM) og lette Linux-containere (LXC), administreret via en webbrugerflade med indbyggede snapshots og backup. Den kører på almindelig forbrugerhardware, og betaling er kun nødvendig for support og enterprise-opdateringer, hvilket gør den populær til hjemmelabs. Den er ikke valgt, fordi den ligesom andre type 1 hypervisorer kræver en dedikeret maskine. Til gengæld er den et naturligt skridt videre fra VirtualBox: en gammel PC kan omdannes til en lab-server, hvor Kali og målsystemer kører permanent og kan tilgås fra flere maskiner, og eksisterende VM'er kan importeres, så arbejdet fra type 2-miljøet kan genbruges.

[Proxmox](https://www.proxmox.com/en/)

### Type 2 Hypervisor
Installeres på eksisterende OS (HOST) og kan have 0..* VM (GUEST)

En type 2 hypervisor kører som et almindeligt program på host-maskinen, så testmiljøet kan sættes op på den maskine, som projektet i forvejen udvikles på. Prisen er lidt lavere ydeevne og at host-OS'et er en del af angrebsfladen, hvilket er acceptabelt, da der kun testes mod eget lokalt system.

#### VirtualBox

VirtualBox er en gratis open source type 2 hypervisor fra Oracle, der kører på Windows, macOS og Linux, så opsætningen kan genskabes af andre uanset deres styresystem. Versionen af VirtualBox noteres i bilag sammen med netværksopsætningen (fx Host-only-netværk mellem Kali og den maskine, der kører Sheet Herder), så angrebstrafikken holdes lokal og ikke rammer produktionsmiljøet.

[VirtualBox](https://www.virtualbox.org/)

## Vagrant
Brugt til Infrastructure as Code *IaC* hvor der skabes et reproducerbart miljø

Med Vagrant beskrives VM'en i en `Vagrantfile` (box, ressourcer, netværk og installation af værktøjer), som kan versionsstyres i git sammen med resten af projektet. En anden studerende kan dermed genskabe præcis samme miljø med `vagrant up` og fjerne det igen med `vagrant destroy`, hvilket er mere reproducerbart end en manuelt opsat VM. Ulempen er en ekstra læringskurve, og det er derfor et oplagt næste skridt frem for et krav i dette projekt.

[Vagrant](https://developer.hashicorp.com/vagrant)

## WSL2
**Fordele:**
- Let og hurtig at starte, med tæt integration til Windows-filsystemet og værktøjer som VS Code.
- Bruger færre ressourcer end en fuld VM.

**Ulemper:**
- Ringere isolation fra host – Windows og WSL deler bl.a. filsystem og netværk, hvilket er uhensigtsmæssigt ved sikkerhedstest.
- Begrænset kontrol over netværket (ingen simpel opsætning af isolerede lab-netværk) og manglende rå hardwareadgang, fx til WiFi-adaptere.
- Ingen indbyggede snapshots på samme måde som i VirtualBox, hvilket gør det sværere at nulstille til en kendt tilstand.

## Distros
Valget af distribution afhænger af rollen i testmiljøet: en stabil distribution egner sig til at køre målsystemet, mens en sikkerhedsdistribution egner sig til angriberen.

### Debian

Debian er en stabil og udbredt distribution, som både Ubuntu, Kali og Parrot bygger på. Den er velegnet som base for servere og Docker-images, men har ingen sikkerhedsværktøjer installeret fra start.

[Debian](https://www.debian.org/)

### Ubuntu

Ubuntu er baseret på Debian og er en god kandidat som host for målsystemet, hvis 3. semester opgaven skal køre i en separat VM. Den er udbredt og godt dokumenteret, også til opsætning af Docker.

[Ubuntu](https://ubuntu.com/)

### Kali

Kali er en Debian-baseret distribution til penetrationstest med værktøjer som Burp Suite, nmap og Wireshark forudinstalleret, og den bruges som angriber-maskine i testmiljøet. Da Kali er en rolling release, ændrer værktøjernes versioner sig løbende. Dette er relevant for rapporten hvor Kali-versionen og de anvendte værktøjsversioner bør angives i bilag for at analysen kan genskabes.

[Kali](https://www.kali.org/)

### Parrot OS

Parrot OS er ligesom Kali en Debian-baseret sikkerhedsdistribution med mange af de samme værktøjer, men bruger færre ressourcer og har også en udgave til almindeligt dagligt brug. Kali er valgt frem for Parrot, fordi Kali er mere udbredt i undervisningsmateriale og dokumentation, og fordi der findes officielle VirtualBox-images.

[Parrot OS](https://parrotsec.org/)

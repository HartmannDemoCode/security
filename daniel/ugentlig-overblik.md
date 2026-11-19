# Ugentlig Overblik Daniel

*Nyeste uge øverst*

## Uge 47
### Problemformulering  
Problemformuleringen er opdateret og bør genlæses i sin helhed. Thomas' to anbefalinger er indarbejdet:
- Underspørgsmål 3 er delt i to: API (3) og klient (4)
- Underspørgsmål 5 er omformuleret, så sproget er i tråd med resten af dokumentet

**Øvrige ændringer:**
- Formuleringer antager ikke længere, at der findes sårbarheder
- Underspørgsmål 2: testmiljøet er koblet til de etiske og juridiske rammer
- Afgrænsning: kun andre OWASP-kategorier udelukkes, ikke specifikke sårbarheder. CORS, hashing og JWT er tilføjet, og eksterne tjenester er udelukket

**Til mødet:**
- **"vi" eller "jeg"?** Problemformuleringen bruger forfatter-"vi", men eksamen er individuel. Er det acceptabelt, eller skal rapporten skrives med "jeg"?

[Problemformulering](problemformulering.md)

### Testmiljø
Jeg har sat et lokalt testmiljø op, som skal bruges fremadrettet. Processen og overvejelserne er beskrevet i den nye fil, der er linket herunder.

**Valg:**
- **Hypervisor:** VirtualBox (type 2). Det er gratis og open source, og testmiljøet kan genskabes på en almindelig studiecomputer. Snapshots bruges til at nulstille miljøet.
- **Distro:** Kali Linux som angriber-maskine, da de fleste sikkerhedsværktøjer er forudinstalleret, og der findes officielle VirtualBox-images.
- **Opsætning:** 4 CPU-kerner, 8 GB RAM og standardlager (80 GB dynamisk allokeret).
- **Fravalgt:** WSL2 isolerer for dårligt fra host. Type 1 hypervisorer (VMware ESXi, Proxmox) kræver en dedikeret maskine. Proxmox og Vagrant er noteret som mulige næste skridt.

**OWASP Juice Shop** er sat op i miljøet som en bevidst sårbar webapplikation, så værktøjer og teknikker kan øves, før 3. semester opgaven testes.

[Virtual Machine **VM**](linux.md)
[Juice Shop](owasp.md)

---

## Uge XX (Skabelon)
### Emne X  
Her beskriver jeg hvad jeg har undersøgt / arbejdet på siden sidste uge  
[filnavn](link-til-internt-dokument)

### Emne Y  
Her beskriver jeg hvad jeg har undersøgt / arbejdet på siden sidste uge  
[filnavn](link-til-internt-dokument)  
[ressource](url)

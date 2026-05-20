# Day 3 - Information Gathering and Vulnerability Scanning

## Forberedelse: (ca. 6-10 timer)

- https://tryhackme.com/room/passiverecon
- https://tryhackme.com/room/activerecon
- https://tryhackme.com/room/pentestingfundamentals
- https://tryhackme.com/room/introtooffensivesecurity
- https://tryhackme.com/room/nmap01
- https://tryhackme.com/room/nmap02 (valgfri)
- https://www.freecodecamp.org/news/an-introduction-to-web-server-scanning-with-nikto/

## Intro til emner plenum i klassen:

- Kort introduktion til "hvad er reverse shell" plenum i klassen.
- https://www.exploit-db.dk
- https://www.gtfobins.github.io
- `nc -nlvp <port>`

## Noter til dagen:

- I skal bruge noget som hedder `Exploit DB` i dag. (www.exploit-db.dk)

- I skal måske installere `pipcolor`. (`pip install termcolor`)

- Normalt er der 2 flag på maskiner `user.txt` og `root.txt`. Det er dog ikke altid at de hedder dette, og ved afvigelse står det måske i rummets beskrivelse.

- I kommer til at benytte hjemmeside https://gtfobins.github.io/ i dag.

- Denne kommando (`find / type -f -user root -perm -u=s 2> /dev/null`) bruges til at finde filer hvor SUID bitten er sat. Når den er det betyder det, at filen kan eksekveres med de samme rettigheder som ejeren.

- Dette TryHackMe rum skal vi sammen hacke: **RootMe**

- Feedback til dagen: (gammel) Video

- Vi hacker vores første server og får admin rettigheder. Binder tingene sammen fra sidste undervisningsgang til at kunne fuldføre et komplet hack.
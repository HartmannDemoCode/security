# OWASP TOP 10:2025

## TOP 10

## Juice Shop
OWASP Juice Shop er en bevidst sårbar webshop, udviklet som et af OWASPs flagskibsprojekter til træning i websikkerhed. Den er bygget med Node.js/Express og Angular, altså samme type stack som moderne single-page applications med REST API, og indeholder sårbarheder fra hele OWASP Top 10, bl.a. Broken Access Control og Authentication Failures. Sårbarhederne er organiseret som udfordringer med sværhedsgrad fra 1 til 6 stjerner, og et indbygget scoreboard viser, hvilke der er løst. Det gør den velegnet til at øve værktøjer og teknikker lovligt, før et rigtigt system testes.

### Opsætning på VM
Juice Shop kører som Docker-container inde i Kali-VM'en, så den er isoleret fra både host-maskinen og netværket. Porten er kun bundet til `127.0.0.1` (localhost), så applikationen kun kan nås fra VM'en selv og ikke fra andre maskiner på netværket. Det er vigtigt, fordi en bevidst sårbar applikation ellers kan angribes af andre, og fordi Docker som standard binder porte til alle netværksinterfaces (`0.0.0.0`) og går uden om firewallen (fx ufw).

```bash
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo docker pull bkimminich/juice-shop
sudo docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```

Applikationen tilgås derefter i Kalis browser på `http://localhost:3000`. Containeren kan stoppes og startes igen med `sudo docker stop juice-shop` og `sudo docker start juice-shop`, og en ny ren instans fås ved at slette containeren og køre `docker run` igen.

[OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)

## Web Goat
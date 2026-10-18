# Storingsmelder

ik heb iets angepast, Bogdan
Een klein meldingensysteem voor storingen. Je meldt een storing via een
formulier, je ziet de openstaande meldingen in een lijst, en je sluit ze af als
ze opgelost zijn.

Deze applicatie is het startpunt voor **Cloud computing (B-UCLL-MGN13A)**. Je
werkt er het hele semester mee. De applicatie zelf verandert bijna niet, wat
verandert is hoe en waar ze draait.

---

## Wat er in deze repo zit

```
app.py              de applicatie
requirements.txt    de pakketten die ze nodig heeft
templates/          de HTML van de webpagina
```

## Wat er bewust niet in zit

Geen Dockerfile en geen compose-bestand. Die schrijf je zelf, dat is precies
wat je in dit vak leert.

---

# Storingsmelder - Cloud Computing

Een klein meldingsysteem voor storingen. Je meldt een storing via een formulier, je ziet de openstaande meldingen in een lijst, en je sluit ze af als ze opgelost zijn.

Deze applicatie is het startpunt voor **Cloud Computing**. Dit project is gecontaineriseerd met Docker en PostgreSQL als database.

---

## Handleiding: Applicatie vanaf nul starten

Volg onderstaande stappen om de applicatie en de database lokaal op te starten vanaf een schone installatie.

## Repository klonen
Kloon de repository naar je lokale machine en navigeer naar de projectmap:

```bash
git clone https://github.com/JBE-1988/cloud-computing.git
cd cloud-computing

Maak een lokaal .env bestand aan op basis van het meegeleverde .env.example bestand:

cp .env.example .env

Open het .env bestand in je gewenste editor en vul de gewenste waarden in voor de database:

POSTGRES_USER=storingsmelder
POSTGRES_PASSWORD=secretpassword
POSTGRES_DB=storingsmelder

Start het hele systeem (de Python applicatie en de PostgreSQL database) op in de achtergrond via Docker Compose:

docker compose up -d

Controleer of beide containers draaien en of de database de status (healthy) heeft bereikt:

docker compose ps

Open je webbrowser en navigeer naar:

http://localhost:8000

Stoppen en containers verwijderen (met behoud van data in de volume):

docker compose down

Stoppen en alle volumes/data volledig wissen:

docker compose down -v

## Wat de applicatie nodig heeft

| | |
|---|---|
| Python | 3.11 of hoger |
| Pakketten | staan in `requirements.txt` |
| Poort | 8000, tenzij je `PORT` anders instelt |

## Instellingen

Alles wordt geregeld met environment variables. Er staat niets vast in de code,
en er is geen configuratiebestand dat je moet aanpassen.

| Variabele | Standaard | Wat het doet |
|---|---|---|
| `DATABASE_URL` | leeg | Leeg betekent opslaan in een SQLite-bestand. Vul je hier een PostgreSQL-adres in, dan gebruikt de applicatie die database |
| `SQLITE_PAD` | `/data/storingen.db` | Waar het SQLite-bestand komt te staan |
| `APP_TITEL` | `Storingsmelder` | De titel bovenaan de pagina |
| `PORT` | `8000` | De poort waarop de applicatie luistert |

### Twee standen

De applicatie draait met of zonder aparte database.

**Zonder `DATABASE_URL`** schrijft ze naar een SQLite-bestand. Dat werkt meteen
en je hebt niets anders nodig. Let op waar dat bestand terechtkomt, want daar
kom je later op terug.

**Met `DATABASE_URL`** praat ze met PostgreSQL. Bijvoorbeeld:

```
DATABASE_URL=postgresql://storing:geheim@db:5432/storingen
```

Je hoeft de tabel niet zelf aan te maken. De applicatie doet dat bij het
opstarten als ze nog niet bestaat.

---

## Controleren of ze draait

Naast de gewone pagina is er een adres dat kort antwoordt of alles in orde is:

```
GET /health
```

Draait alles, dan krijg je `{"status": "ok", ...}` met code 200. Kan de
applicatie haar database niet bereiken, dan krijg je code 503. Dat adres komt
later in het semester nog van pas.

---

## Wat je er dit semester mee doet

| Blok | Wat je doet |
|---|---|
| Week 1 tot 4 | Je zet deze applicatie in een container en laat ze samen met een database draaien |
| Week 5 tot 9 | Je zet diezelfde applicatie op een Kubernetes-cluster en maakt ze bereikbaar in je browser |
| Week 10 tot 11 | Je laat een pipeline het image bouwen en klaarzetten |

Je begint dus nooit opnieuw. Elke opdracht bouwt verder op de vorige.

---

## Deze repo is van jou

Je hebt hem gemaakt vanaf het sjabloon, dus dit is jouw kopie. Werk erin, commit
elke les, en push. Indienen doe je met de link naar een commit. Hoe dat precies
gaat, staat in de opdrachtbundel op Toledo.

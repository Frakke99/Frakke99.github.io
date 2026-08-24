# 30 Seconden

Een offline speelbare webapp/PWA van het woordraadspel, met ingebouwde timer, 1- of 2-teamsmodus en een lokaal leaderbord.

## Bestanden

| Bestand | Rol |
|---|---|
| `index.html` | De volledige app (spel, schermen, 100 kaartjes, leaderbord) |
| `manifest.json` | PWA-manifest (naam, iconen, kleuren) |
| `sw.js` | Service worker voor offline gebruik |
| `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png` | App-iconen |

Alle bestanden moeten samen in dezelfde map staan.

## Lokaal uitproberen

Dubbelklikken op `index.html` werkt om te spelen, maar de service worker (offline) en het "installeren" werken enkel via een webserver over http(s). Snel lokaal testen met een servertje:

```bash
# in de map met de bestanden
python3 -m http.server 8000
# surf dan naar http://localhost:8000
```

## Kaartjes toevoegen

Open `index.html` en zoek de lijst `const DECK = [`. Elk kaartje is één regel met precies 5 begrippen:

```js
["woord1","woord2","woord3","woord4","woord5"],
```

Plak er zoveel regels bij als je wil, telkens tussen de rechte haakjes en eindigend op een komma. Bewaren en de pagina herladen. Er zitten er nu 100 in.

## Gratis online zetten (met deelbare link)

Alle opties hieronder zijn gratis en geven je een https-URL, wat nodig is om de PWA te kunnen installeren.

- **GitHub Pages**: maak een repository, upload deze bestanden in de root, en zet Pages aan via *Settings → Pages → Deploy from branch → main /(root)*. Je krijgt een `https://<gebruiker>.github.io/<repo>/`-link.
- **Netlify**: sleep de map naar app.netlify.com/drop. Klaar in seconden.
- **Cloudflare Pages**: koppel de repo of upload de map.
- **Azure Static Web Apps**: past bij de Microsoft-stack. Maak een Static Web App (gratis SKU), koppel de GitHub-repo, laat de app-locatie op de root staan en de api-locatie leeg.

## Als app installeren (PWA)

Zodra de app op een https-URL staat:

- **Android (Chrome)**: menu ⋮ → *App installeren* of *Toevoegen aan startscherm*.
- **iPhone/iPad (Safari)**: deelknop → *Zet op beginscherm*.
- **Desktop (Chrome/Edge)**: installeericoon in de adresbalk.

Daarna draait het spel fullscreen als een app en werkt het ook zonder internet.

## Leaderbord

De topscores worden per team bewaard in de browser (`localStorage`) van het toestel waarop je speelt. Bij 2 teams komt elk team apart op het bord, zodat een sterk team kan doorstoten naar de top. Het bord is dus **lokaal per toestel/browser**, niet gedeeld tussen apparaten.

Wil je later een gedeeld (globaal) leaderbord over meerdere toestellen, dan is er een kleine backend nodig (bv. Supabase, Firebase, of Azure Static Web Apps met Functions + Table Storage). Dat kan gratis op kleine schaal, maar vergt wat extra opzet.

## Bediening in het spel

- Tik op de **cirkel** (of spatie) om de timer te starten/pauzeren.
- Tik een **woord** aan als het geraden is; nog eens tikken maakt het ongedaan. De score loopt automatisch mee.
- **Volgende beurt** zet de punten vast en geeft de beurt door. Op de laatste beurt verschijnt de uitslag.
- Sneltoetsen: `spatie` = start/pauze, `N` = volgende beurt, `R` = timer resetten.

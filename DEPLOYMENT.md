# Bot Deployment Anleitung

## Schritt 1: Systemanforderungen prüfen

### Node.js Version überprüfen
```bash
node --version
```
**Erforderlich:** Node.js Version 18.0.0 oder höher

Falls Node.js nicht installiert ist oder die Version zu alt ist:
- **Installation:** Besuche https://nodejs.org/ und lade die LTS-Version herunter
- **Oder via Homebrew (macOS):** `brew install node`

---

## Schritt 2: Projekt vorbereiten

### 1. In das Projektverzeichnis wechseln
```bash
cd /Volumes/Privat/VSC/Tier-Test-Bot
```

### 2. Dependencies installieren
```bash
npm install
```

Dies installiert automatisch:
- `discord.js` (Discord Bot Library)
- `dotenv` (Umgebungsvariablen)

---

## Schritt 3: Umgebungsvariablen konfigurieren

### 1. .env Datei erstellen
Erstelle eine Datei namens `.env` im Hauptverzeichnis des Projekts:

```bash
touch .env
```

### 2. .env Datei bearbeiten
Öffne die `.env` Datei und füge folgende Variablen ein:

```env
# Discord Bot Token (ERFORDERLICH)
DISCORD_TOKEN=dein_bot_token_hier

# Discord Server ID (Optional)
GUILD_ID=deine_guild_id_hier

# Queue Konfiguration (Optional - haben Standardwerte)
QUEUE_ROLE_ID=1432067571267666011
QUEUE_CHANNEL_ID=1431715341930860725

# Embed Konfiguration (Optional - haben Standardwerte)
EMBED_COLOR=0xFF0000
EMBED_TITLE=Crystals Queue
EMBED_FOOTER=Queue

# Skin API URL (Optional - hat Standardwert)
SKIN_API_URL=https://mc-heads.net
```

**Wichtig:** Ersetze `dein_bot_token_hier` mit deinem echten Discord Bot Token!

### Discord Bot Token erhalten:
1. Gehe zu https://discord.com/developers/applications
2. Erstelle eine neue Application oder wähle eine bestehende
3. Gehe zu "Bot" → "Add Bot"
4. Klicke auf "Reset Token" und kopiere den Token
5. Füge ihn in die `.env` Datei ein

---

## Schritt 4: Bot starten

### Option 1: Normales Starten
```bash
npm start
```

### Option 2: Direkt mit Node.js
```bash
node index.js
```

### Option 3: Im Hintergrund starten (für dauerhaften Betrieb)

**Mit `nohup` (Unix/macOS):**
```bash
nohup node index.js > bot.log 2>&1 &
```

**Mit `screen` (empfohlen für Server):**
```bash
# Screen installieren (falls nicht vorhanden)
# macOS: brew install screen

# Screen Session erstellen
screen -S discord-bot

# Bot starten
npm start

# Screen verlassen (Bot läuft weiter): Strg+A, dann D drücken
# Zurückkehren: screen -r discord-bot
```

**Mit `pm2` (Production Manager, empfohlen):**
```bash
# PM2 global installieren
npm install -g pm2

# Bot starten
pm2 start index.js --name tier-test-bot

# Bot Status prüfen
pm2 status

# Logs anzeigen
pm2 logs tier-test-bot

# Bot stoppen
pm2 stop tier-test-bot

# Bot beim Systemstart automatisch starten
pm2 startup
pm2 save
```

---

## Schritt 5: Bot-Verbindung prüfen

Nach dem Start solltest du folgende Ausgaben sehen:
- Bot verbindet sich mit Discord
- "Bot ist bereit!" oder ähnliche Meldung
- Keine Fehler

**Häufige Fehler:**
- `Invalid token`: Token ist falsch oder nicht gesetzt
- `Missing Access`: Bot hat keine Berechtigung auf dem Server
- `Cannot find module`: Dependencies nicht installiert → `npm install` ausführen

---

## Schritt 6: Bot im Discord testen

1. Füge den Bot zu deinem Discord Server hinzu:
   - Gehe zu https://discord.com/developers/applications
   - Wähle deine Application → "OAuth2" → "URL Generator"
   - Wähle "bot" Scope
   - Wähle benötigte Berechtigungen (z.B. "Send Messages", "Embed Links")
   - Kopiere die URL und öffne sie im Browser

2. Teste die Commands:
   - `/queue` - Queue starten
   - `/stopqueue` - Queue stoppen
   - `/remove [user]` - User entfernen
   - `/result` - Test Ergebnis senden

---

## Zusätzliche Tipps

### Bot dauerhaft laufen lassen
Für einen produktiven Server empfiehlt sich `pm2`:
```bash
npm install -g pm2
pm2 start index.js --name tier-test-bot
pm2 save
pm2 startup
```

### Logs überwachen
```bash
# Mit pm2
pm2 logs tier-test-bot

# Oder direkt die Logdatei
tail -f bot.log
```

### Bot neu starten
```bash
# Mit npm
# Strg+C zum Stoppen, dann erneut npm start

# Mit pm2
pm2 restart tier-test-bot
```

### Updates installieren
```bash
git pull
npm install
pm2 restart tier-test-bot
```

---

## Troubleshooting

**Bot startet nicht:**
- Prüfe Node.js Version: `node --version` (muss ≥ 18.0.0 sein)
- Prüfe ob `.env` Datei existiert und `DISCORD_TOKEN` gesetzt ist
- Führe `npm install` erneut aus

**Bot verbindet sich nicht:**
- Token überprüfen (keine Leerzeichen, vollständig)
- Bot hat Server-Zugriff? (OAuth2 URL verwenden)
- Firewall/Netzwerk prüfen

**Commands funktionieren nicht:**
- Bot braucht "Application Commands" Berechtigung
- Commands müssen registriert werden (passiert beim ersten Start)

---

## Zusammenfassung - Schnellstart

```bash
# 1. Dependencies installieren
npm install

# 2. .env Datei erstellen mit DISCORD_TOKEN
echo "DISCORD_TOKEN=dein_token_hier" > .env

# 3. Bot starten
npm start
```

**Fertig!** Der Bot sollte jetzt laufen. 🚀


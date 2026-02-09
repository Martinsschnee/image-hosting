# 🏆 sc_playtime - FiveM Spielzeit Leaderboard

Ein komplexes FiveM-Resource-Script, das die Spielzeit aller Spieler trackt und automatisch ein **Top 15 Leaderboard** in einem Discord-Channel als persistente, regelmäßig aktualisierte Nachricht anzeigt.

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![FiveM](https://img.shields.io/badge/FiveM-Ready-green.svg)
![ESX](https://img.shields.io/badge/ESX-Legacy-orange.svg)

---

## 📋 Features

- ✅ **Automatisches Spielzeit-Tracking** - Erfasst die Spielzeit aller Spieler
- ✅ **Discord Integration** - Bot läuft direkt auf dem FiveM Server (kein externes Hosting)
- ✅ **Persistente Message** - Eine Message wird regelmäßig editiert (nicht neu gesendet)
- ✅ **Top 15 Leaderboard** - Zeigt die aktivsten 15 Spieler
- ✅ **Ingame-Namen** - Verwendet ESX Charakternamen (firstname + lastname)
- ✅ **Zeitformat dd:hh:mm** - Tage:Stunden:Minuten Format
- ✅ **Spezielle Emojis** - 🥇🥈🥉 für die Top 3 Plätze
- ✅ **Countdown-Timer** - Zeigt Zeit bis zum nächsten Update
- ✅ **Konfigurierbar** - Update-Intervall, Texte, Design anpassbar
- ✅ **Performance-Optimiert** - Periodisches Speichern, Rate Limiting

---

## 🎨 Vorschau

Das Leaderboard wird im Discord wie folgt angezeigt:

```
🏆 Top 15 Spielzeit Leaderboard
Hier werden die Aktivsten 15 Spieler auf unserem Five:M Server angezeigt.

📊 Die Top 15 Spieler:
🥇 1. Justin Gamer | 45:12:30
🥈 2. Max Mustermann | 38:05:15
🥉 3. Anna Schmidt | 32:18:45
4. Tom Weber | 28:30:00
5. Lisa Müller | 25:45:20
...

Die Spielzeiten wurden zuletzt vor: 2 Minuten aktualisiert
Die nächste Aktualisierung folgt in: 13 Minuten

Sona City | 20:50 am 09.02.2026
```

---

## 📦 Voraussetzungen

- **FiveM Server** mit ESX Framework
- **oxmysql** Resource installiert
- **Discord Bot** erstellt (mit Message Content Intent)
- **Node.js Support** in FiveM aktiviert (standardmäßig aktiv)

---

## 🚀 Installation

### 1. Resource herunterladen

Kopiere den `sc_playtime` Ordner in deinen `resources/` Ordner:

```
resources/
└── sc_playtime/
    ├── fxmanifest.lua
    ├── server.js
    ├── config.js
    ├── install.sql
    └── README.md
```

### 2. Datenbank importieren

Importiere die SQL-Datei in deine Datenbank:

```bash
mysql -u root -p deine_datenbank < sc_playtime/install.sql
```

Oder über phpMyAdmin:
1. Öffne phpMyAdmin
2. Wähle deine Datenbank
3. Gehe zu "Importieren"
4. Wähle `install.sql` aus
5. Klicke auf "OK"

### 3. Discord Bot erstellen

1. Gehe zu [Discord Developer Portal](https://discord.com/developers/applications)
2. Klicke auf "New Application"
3. Gib einen Namen ein (z.B. "Sona City Playtime")
4. Gehe zu "Bot" → "Add Bot"
5. Aktiviere unter "Privileged Gateway Intents":
   - ✅ Message Content Intent
6. Kopiere den Bot Token (unter "TOKEN" → "Reset Token")
7. Gehe zu "OAuth2" → "URL Generator"
8. Wähle:
   - **Scopes:** `bot`
   - **Bot Permissions:** 
     - ✅ Send Messages
     - ✅ Read Message History
     - ✅ View Channel
9. Kopiere die generierte URL und füge den Bot zu deinem Discord Server hinzu

### 4. Channel ID ermitteln

1. Aktiviere den Discord Developer Mode:
   - Discord → Einstellungen → Erweitert → Entwicklermodus aktivieren
2. Rechtsklick auf den gewünschten Channel → "ID kopieren"

### 5. Konfiguration

#### A. `config.js` bearbeiten

Öffne `sc_playtime/config.js` und trage ein:

```javascript
// Discord Server (Guild) ID
guildId: '1007614962006491200', // Deine Guild ID

// Channel ID für das Leaderboard
channelId: 'HIER_DEINE_CHANNEL_ID_EINTRAGEN',
```

Optional kannst du auch anpassen:
- `updateInterval` - Update-Intervall (Standard: 15 Minuten)
- `saveInterval` - Speicher-Intervall (Standard: 1 Minute)
- `texts` - Alle Texte im Leaderboard
- `accentColor` - Farbe des Leaderboards
- `imageUrl` - Logo/Bild URL

#### B. `server.cfg` bearbeiten

Füge folgende Zeilen zu deiner `server.cfg` hinzu:

```bash
# sc_playtime Resource
ensure sc_playtime

# Discord Bot Token (WICHTIG: Geheim halten!)
set sc_playtime_token "DEIN_BOT_TOKEN_HIER"
```

**⚠️ WICHTIG:** Halte den Bot Token geheim! Teile ihn niemals öffentlich.

### 6. Resource starten

Starte deinen FiveM Server oder führe aus:

```bash
restart sc_playtime
```

---

## ⚙️ Konfiguration

### Update-Intervall ändern

In [`config.js`](config.js):

```javascript
// 15 Minuten (Standard)
updateInterval: 900000,

// 5 Minuten
updateInterval: 300000,

// 30 Minuten
updateInterval: 1800000,
```

### Texte anpassen

In [`config.js`](config.js):

```javascript
texts: {
    title: '## ``🏆`` Top 15 Spielzeit Leaderboard',
    subtitle: '**Dein eigener Text hier**',
    leaderboardTitle: '### ``📊`` Die Top 15 Spieler:',
    // ...
}
```

### Farbe ändern

In [`config.js`](config.js):

```javascript
// Akzentfarbe (Dezimal-Wert von Hex-Farbe)
// Konvertiere Hex zu Dezimal: https://www.rapidtables.com/convert/number/hex-to-decimal.html

accentColor: 14829852, // #E2491C (Orange)
accentColor: 5814783,  // #58B0FF (Blau)
accentColor: 3066993,  // #2ECC71 (Grün)
```

### Debug-Modus aktivieren

In [`config.js`](config.js):

```javascript
debug: true,
```

Zeigt detaillierte Logs in der Server-Konsole.

---

## 🔍 Troubleshooting

### Problem: Bot sendet keine Message

**Lösung:**
1. Prüfe ob der Bot Token korrekt in `server.cfg` gesetzt ist
2. Prüfe ob die Channel ID in `config.js` korrekt ist
3. Prüfe ob der Bot die nötigen Permissions hat:
   - Send Messages
   - Read Message History
   - View Channel
4. Prüfe die Server-Logs: `restart sc_playtime`

### Problem: Spielzeit wird nicht getrackt

**Lösung:**
1. Prüfe ob `oxmysql` läuft: `ensure oxmysql`
2. Prüfe ob die Tabellen existieren:
   ```sql
   SHOW TABLES LIKE 'sc_playtime%';
   ```
3. Prüfe ob ESX `users` Tabelle Einträge hat
4. Aktiviere Debug-Modus in `config.js`

### Problem: Message wird nicht aktualisiert

**Lösung:**
1. Prüfe ob die Message ID korrekt gespeichert wurde:
   ```sql
   SELECT * FROM sc_playtime_config WHERE `key` = 'leaderboard_message_id';
   ```
2. Prüfe Discord API Rate Limits (max. 5 Updates/5 Sekunden)
3. Prüfe Server-Logs auf Fehler

### Problem: Namen werden nicht angezeigt

**Lösung:**
1. Prüfe ob die ESX `users` Tabelle `firstname` und `lastname` Spalten hat
2. Prüfe ob Spieler in der `users` Tabelle existieren:
   ```sql
   SELECT identifier, firstname, lastname FROM users LIMIT 5;
   ```
3. Stelle sicher dass Spieler sich mindestens einmal eingeloggt haben

---

## 📊 Datenbank-Struktur

### Tabelle: `sc_playtime`

| Spalte | Typ | Beschreibung |
|--------|-----|--------------|
| `id` | INT | Primärschlüssel |
| `identifier` | VARCHAR(60) | ESX Identifier (license:xxx) |
| `playtime_seconds` | BIGINT | Gesamte Spielzeit in Sekunden |
| `last_seen` | TIMESTAMP | Letzter Login |
| `created_at` | TIMESTAMP | Erster Login |

### Tabelle: `sc_playtime_config`

| Spalte | Typ | Beschreibung |
|--------|-----|--------------|
| `key` | VARCHAR(50) | Konfigurations-Schlüssel |
| `value` | TEXT | Konfigurations-Wert |
| `updated_at` | TIMESTAMP | Letzte Änderung |

---

## 🔧 Erweiterte Konfiguration

### Message ID Speicherung ändern

In [`config.js`](config.js):

```javascript
// In Datenbank speichern (empfohlen)
messageIdStorage: 'database',

// In config.js speichern
messageIdStorage: 'config',
messageId: 'DEINE_MESSAGE_ID_HIER',
```

### Periodisches Speichern anpassen

In [`config.js`](config.js):

```javascript
// Aktive Sessions alle 60 Sekunden speichern (Standard)
saveInterval: 60000,

// Alle 30 Sekunden
saveInterval: 30000,

// Alle 2 Minuten
saveInterval: 120000,
```

**Hinweis:** Kürzere Intervalle = mehr Datenbank-Queries, aber genauere Spielzeit bei Server-Crashes.

---

## 📝 Logs

### Wichtige Log-Meldungen

```bash
# Erfolgreicher Start
[sc_playtime] sc_playtime v1.0.0 wird gestartet...
[sc_playtime] Discord Bot eingeloggt als: BotName#0
[sc_playtime] Channel gefunden: leaderboard
[sc_playtime] Existierende Leaderboard-Message gefunden
[sc_playtime] sc_playtime erfolgreich gestartet!

# Spieler-Events
[sc_playtime] Spieler verbunden: Justin Gamer (license:xxx)
[sc_playtime] Spieler getrennt: Justin Gamer

# Updates
[sc_playtime] Leaderboard aktualisiert (15 Spieler)
[sc_playtime] 5 aktive Session(s) gespeichert
```

### Fehler-Meldungen

```bash
# Kein Bot Token
[sc_playtime] FEHLER: Kein Bot Token gefunden!

# Keine Channel ID
[sc_playtime] FEHLER: Keine Channel ID in config.js gesetzt!

# Discord API Fehler
[sc_playtime] Discord API Error 403: Missing Permissions
[sc_playtime] Rate Limited, warte 5s... (Versuch 1/3)
```

---

## 🎯 Performance-Tipps

### Datenbank-Optimierung

Die Tabelle `sc_playtime` hat bereits Indizes für optimale Performance:

```sql
-- Prüfe Indizes
SHOW INDEX FROM sc_playtime;

-- Optimiere Tabelle (monatlich empfohlen)
OPTIMIZE TABLE sc_playtime;
```

### Update-Intervall

- **Empfohlen:** 15-30 Minuten
- **Minimum:** 5 Minuten (wegen Discord Rate Limits)
- **Maximum:** Unbegrenzt

### Speicher-Intervall

- **Empfohlen:** 60 Sekunden
- **Minimum:** 30 Sekunden
- **Maximum:** 300 Sekunden (5 Minuten)

---

## 🔐 Sicherheit

### Bot Token schützen

- ✅ **Niemals** den Bot Token in `config.js` hardcoden
- ✅ **Immer** über `server.cfg` setzen
- ✅ `server.cfg` in `.gitignore` aufnehmen
- ✅ Token regelmäßig rotieren

### Datenbank-Zugriff

- ✅ Verwendet `oxmysql` (sicher & performant)
- ✅ Prepared Statements gegen SQL Injection
- ✅ Keine direkten Credentials im Code

---

## 📈 Statistiken

### Beispiel-Abfragen

```sql
-- Gesamte Spielzeit aller Spieler (in Stunden)
SELECT SUM(playtime_seconds) / 3600 AS total_hours FROM sc_playtime;

-- Durchschnittliche Spielzeit (in Stunden)
SELECT AVG(playtime_seconds) / 3600 AS avg_hours FROM sc_playtime;

-- Top 10 Spieler
SELECT 
    CONCAT(u.firstname, ' ', u.lastname) AS name,
    p.playtime_seconds / 3600 AS hours
FROM sc_playtime p
JOIN users u ON p.identifier = u.identifier
ORDER BY p.playtime_seconds DESC
LIMIT 10;

-- Spieler mit über 100 Stunden
SELECT 
    CONCAT(u.firstname, ' ', u.lastname) AS name,
    p.playtime_seconds / 3600 AS hours
FROM sc_playtime p
JOIN users u ON p.identifier = u.identifier
WHERE p.playtime_seconds >= 360000
ORDER BY p.playtime_seconds DESC;
```

---

## 🆘 Support

Bei Problemen oder Fragen:

1. **Logs prüfen:** Aktiviere Debug-Modus in `config.js`
2. **Datenbank prüfen:** Stelle sicher dass Tabellen existieren
3. **Discord Bot prüfen:** Teste Bot-Permissions im Channel
4. **Server-Logs:** Prüfe `server.log` auf Fehler

### Häufige Fehler

| Fehler | Ursache | Lösung |
|--------|---------|--------|
| `Missing Permissions` | Bot hat keine Rechte | Permissions im Channel prüfen |
| `Unknown Channel` | Channel ID falsch | Channel ID in `config.js` prüfen |
| `Invalid Token` | Bot Token falsch | Token in `server.cfg` prüfen |
| `Rate Limited` | Zu viele Requests | Update-Intervall erhöhen |

---

## 📄 Lizenz

Entwickelt für **Sona City Roleplay**

---

## 🎉 Credits

- **Framework:** ESX Legacy
- **Datenbank:** MySQL via oxmysql
- **Discord API:** v10
- **Entwickelt von:** Sona City Development

---

## 🔄 Changelog

### Version 1.0.0 (09.02.2026)
- ✅ Initiales Release
- ✅ Spielzeit-Tracking System
- ✅ Discord Bot Integration
- ✅ Top 15 Leaderboard
- ✅ Persistente Message
- ✅ Konfigurierbare Texte & Design
- ✅ Performance-Optimierungen
- ✅ Umfangreiche Dokumentation

---

**Viel Erfolg mit sc_playtime! 🚀**

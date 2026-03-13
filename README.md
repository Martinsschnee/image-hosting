# sc_grpadmin

Admin-/Support-Resource für FiveM mit TXAdmin-ähnlichem Workflow, NUI-Panel, Quick-Controls, Audit-Logging und Ticket-System.

## Funktionen (kurz & in Stichpunkten)

### 1) Duty, Stealth & Admin-States
- **Alogin/Alogout/Aduty**: sauberer Duty-Status inkl. Synchronisation.
- **Noclip/Vanish/Spectate**: steuerbar per Command und per UI-Action-Layer.
- **Hard-Duty Restore**: Invincible/Fall/Ragdoll-States werden beim Deaktivieren wiederhergestellt.
- **HeadTag-Regeln**: „Administrator“-Anzeige nur bei `OnDuty && !Stealth` (noclip/vanish/spectate).

### 2) TX-ähnliche Quick Controls
- Overlay links oben über `/quick` oder Alias `/tq`.
- Tastatursteuerung:
  - Pfeil hoch/runter: Auswahl
  - Tab: Kategorie wechseln
  - Enter: Action ausführen
  - Esc: schließen
- Unterstützt Kernaktionen wie Duty/States/Teleport/Tickets/Maintenance.

### 3) NUI Quick Actions
- Dashboard-Quick-Actions für:
  - `alogin`, `alogout`, `aduty`
  - `noclip`, `vanish`, `ac-notify`
  - `goto`, `bring`, `bringback`, `spectate`
  - `smute`, `sunmute`
  - `healall`, `clearcars`, `setdim`, `house`
  - Screenshot-Request
- Einheitlicher Action-Endpoint serverseitig: `sc_grpadmin:ui:action`.

### 4) Ticket-Workflow (erweitert)
- Ticket erstellen inkl. **Priority** (`low/medium/high`) und **Labels**.
- Ticket-Liste mit Filtern (Status, Assignee, Priority, Label).
- Thread-Ansicht für Antworten.
- **Interne Team-Notizen** getrennt vom Player-Chat.
- Ticket-Zuweisung (Assignee Identifier/Name) direkt im Panel.

### 5) Spielerverwaltung / Moderation
- Aktionen im Player-Modal:
  - `goto`, `bring`, `spectate`
  - `heal`, `revive`
  - `freeze`, `unfreeze`
  - `warn`, `kick`, `ban`, `unban`
  - `smute`, `sunmute`
  - Screenshot
- Pro Spieler: Historie/Audit im Modal (14 Tage laut Config-Default).

### 6) Monitoring
- Live-Snapshots für onDuty-Team:
  - Player Count / OnDuty Count
  - Resource-States
  - Netzwerk-Basisdaten (`onesync`, max clients)
  - Healthchecks (DB / Screenshot-Service)

### 7) Feed & Audit
- Live-Feed für Team-Events.
- Feed-Filter/Query nach Kategorie/Actor/Target/Text.
- Drilldown (JSON-Detailansicht von Payload/Meta).
- Persistenz über DB-Logtabellen.

### 8) Permissions / Rollenmatrix
- Command-zu-MinRank Override im UI.
- Override-Historie (wer/was/wann).
- Export/Import von Overrides (JSON).
- Reset der Overrides im Panel (projektleitungsgeführt).

### 9) Kill-Logs
- Erfassung über `baseevents` + Client-Event-Pfad (`KILL_REPORT`) als Fallback.
- Speicherung in DB (`kill_logs`) + Live-Update im NUI-Tab.

### 10) Copy UX (TX-ähnlich)
- Copy-Buttons für:
  - Identifiers/Licenses (inkl. „Copy all“)
  - Screenshots (URL/Base64)
  - Permission-Export (JSON)

---

## Wichtige Projektdateien

- Manifest: `fxmanifest.lua`
- Config: `config.lua`
- Migrationen: `sc_grpadmin_migrations.sql`
- Serverseitig (Kern):
  - `modules/server/60_admin_commands.lua`
  - `modules/server/52_support.lua`
  - `modules/server/56_audit.lua`
  - `modules/server/57_bans.lua`
  - `modules/server/58_monitor.lua`
  - `modules/server/20_permissions.lua`
- Clientseitig (Kern):
  - `modules/client/80_aduty.lua`
  - `modules/client/85_admin_runtime.lua`
  - `modules/client/65_killlogs.lua`
- NUI:
  - `web/index.html`
  - `web/style.css`
  - `web/app.js`

---

## Code-Fakten (aktueller Snapshot)

Ermittelt per lokalem Code-Scan (ohne `web/capture_assets` und ohne `capture.js` Hilfsdateien):

- **Dateien gesamt (Code):** `44`
- **Zeilen gesamt:** `12,925`
- **Nach Sprache:**
  - Lua: `8,414`
  - JavaScript: `2,119`
  - CSS: `1,531`
  - HTML: `566`
  - SQL: `295`
- **Nach Layer:**
  - Server-Lua: `4,950`
  - Client-Lua: `2,680`
  - Shared-Lua: `350`
  - Web (HTML/CSS/JS): `2,796`

> Hinweis: Diese Zahlen sind ein Snapshot zum Zeitpunkt der Erstellung und können sich nach weiteren Änderungen ändern.

---

## Datenbank / Migration (Kurz)

Neue/erweiterte Tabellen u. a.:
- `tickets` (Assignee/Priority/Labels)
- `ticket_internal_notes`
- `player_actions_audit`
- `permission_overrides_history`
- `kill_logs`
- `admin_bans`
- `admin_feed_log`

Migration ausführen über:
- `sc_grpadmin_migrations.sql`

---

## Abhängigkeiten (Kurz)

- `es_extended`
- `esx_skin`
- `skinchanger`
- Screenshot-Integration via konfigurierbarer Resource (Default/Fallback über Config)


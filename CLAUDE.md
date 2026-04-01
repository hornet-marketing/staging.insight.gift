# CLAUDE.md – insight.gift

Dieses Dokument ist die verbindliche Arbeitsgrundlage für Claude Code bei jedem Start einer neuen Session. Es gilt ohne Ausnahme.

---

## Projekt

| | |
|---|---|
| **Name** | insight – Zelle für Zelle in die Balance |
| **Thema** | Epigenetik, Funktionelle Medizin, ganzheitliche Gesundheit |
| **Betreiberin** | Daniela Horne |
| **Ziel** | Kombination aus Informationsseite, Lead-Generierung und Shop |
| **Staging-URL** | https://staging.insight.gift |
| **Live-URL** | https://insight.gift (noch nicht aktiv) |
| **CMS** | Joomla! 5.3.4 |
| **PHP** | 8.3 (Minimum 8.1) |
| **Timezone** | Europe/Berlin |
| **Sprache** | Deutsch (Primär) |

---

## Zielgruppe & Positionierung

**Primäre Zielgruppe:** Gesundheitsbewusste Privatpersonen, insbesondere neurodiverse Menschen (ADS/ADHS/ASS), die ganzheitliche Lösungen suchen und mit konventionellen Ansätzen bisher keine dauerhaften Ergebnisse erzielt haben.

**Positionierung:** Wissenschaftlich fundiertes Wissen mit Energiearbeit verbinden – ganzheitlich auf Körper, Geist und Seele. Daniela Horne verknüpft modernste Epigenetik mit bewährten energetischen Methoden.

**Tonalität:** Persönlich-empathisch, wissenschaftlich fundiert, vertrauensbildend. Du-Ansprache. Keine medizinischen Heilsversprechen (rechtliche Absicherung beachten).

**Danielas Angebot:**
- Epigenetik und Gen-Analysen
- Epigenetische und funktionelle Analysen
- Ernährungsberatung und Darmgesundheit
- Blockadenlösung durch BodyCode und EmotionCode
- Coaching und Ursachenfindung
- Analyse des Mikrobioms und Hirngesundheit

---

## Design-System

**Template:** narioz (Helix Ultimate Framework, JoomShaper)
**Template-Verzeichnis:** `templates/narioz/`

### Farben (CI)
| Rolle | Hex | Verwendung |
|-------|-----|------------|
| Anthrazit | #595959 | Fließtext, Hintergründe |
| Türkis/Grün | #30c196 | Primärfarbe, CTAs, Akzente |
| Rose hell | #ffc1f9 | Akzent, Highlights |
| Rose dunkel | #ff82f2 | Akzent, Hover |
| Purple hell | #e4abff | Sekundärakzent |
| Purple dunkel | #b66bda | Sekundärakzent, Headings |

### Schriften
| Rolle | Schrift |
|-------|---------|
| Fließtext | Questrial |
| Überschriften/Display | Quigley |

### Anpassungen ausschließlich in:
- `templates/narioz/css/custom.css` – eigene CSS-Anpassungen
- `templates/narioz/html/` – Joomla Template Overrides
- `templates/narioz/` – Template-Parameter, Layout-Dateien

### NIEMALS modifizieren:
- `libraries/` – Joomla Framework (wird bei Updates überschrieben)
- `includes/` – Core-Dateien
- Joomla-Core-Dateien außerhalb von `templates/narioz/`

---

## Geplante Website-Struktur

### Startseite (Aufbau)
1. Hero: „Stell dir vor: Dein Körper hat wieder Kraft."
2. Mehrwerte des Angebots
3. Problem-Sektion: Warum bisherige Ansätze gescheitert sind
4. Vorstellung Daniela Horne
5. Produktvorstellung / Leistungen
6. 3-Schritte-Plan (Erstgespräch → Analyse → Veränderung)
7. Finaler CTA: Kostenloses Erstgespräch buchen
8. Kundenstimmen / Testimonials
9. Footer

### Weitere Seiten
- Über mich (Daniela Horne, Story, Qualifikationen)
- Leistungen / Angebote
- Blog / Wissen
- Kontakt
- Datenschutz, Impressum

---

## Aktive Extensions

| Extension | Zweck | Status |
|-----------|-------|--------|
| Helix Ultimate | Template-Framework | ✅ Aktiv |
| SP Page Builder | Drag-and-Drop Seiteneditor | ✅ Aktiv |
| Akeeba Backup Pro | Backup & Restore + Google Drive Sync | ✅ Aktiv |
| HikaShop | E-Commerce | ⚠️ Wird deinstalliert |
| WebP Converter (JoomIQ) | Automatische Bildoptimierung bei Upload | ⏳ Noch nicht installiert |

---

## Datenbank

| | |
|---|---|
| **Name** | danielahorne_jom |
| **Präfix** | jins_ |
| **Host** | 127.0.0.1:33010 (via SSH-Tunnel) |
| **Benutzer** | danielahorne_dbuser |
| **Core-Tabellen** | jins_content, jins_users, jins_menu, jins_extensions, jins_modules |

**Zugriff:** Ausschließlich via MCP MySQL. SSH-Tunnel muss aktiv sein (`tunnel`).
**Erlaubt:** SELECT, INSERT, UPDATE
**Verboten:** DELETE (im MCP deaktiviert)

---

## Entwicklungsumgebung

### Lokales Setup
| | |
|---|---|
| **Lokaler Pfad** | `/Users/manuelhorne/HorNet.Marketing/Web-Projekte/staging.insight.gift/` |
| **Git-Repository** | `git@github.com:hornet-marketing/staging.insight.gift.git` (Private) |
| **Branch** | main |
| **SSH-User Server** | ins-user1@staging.insight.gift |
| **Server-Pfad** | `/var/www/vhosts/insight.gift/httpdocs/staging/` |

### Shell-Aliases (in ~/.zshrc)

```bash
tunnel           # SSH-Tunnel zur DB öffnen (lokal Port 3306 → Server Port 33010)
staging          # Ins Projektverzeichnis wechseln + git pull
deploy "msg"     # DB-Dump erstellen + git add + commit + push → Webhook → Plesk deployed
pull-server      # Server → Lokal synchronisieren (rsync mit .gitignore-Filter)
sync-from-server # pull-server + git status (nach Backend-Änderungen ausführen)
```

> ⚠️ **WICHTIG:** Den SSH-Tunnel NIEMALS selbst per Bash-Befehl starten. Immer ausschließlich über den `tunnel`-Alias. Grund: Der korrekte Port-Mapping ist 3306→33010. Ein manuell gestarteter Tunnel mit falschem Port (z.B. 3306→3306) führt zu „Access denied"-Fehlern beim DB-Zugriff.

### MCP-Server

| Server | Zweck | Voraussetzung |
|--------|-------|---------------|
| mcp_server_mysql | DB lesen/schreiben | `tunnel` muss aktiv sein |
| playwright | Browser-Testing, Screenshots | – |

MCP-Status prüfen: `/mcp` im Claude Code Chat

### CLI Commands

```bash
php cli/joomla.php list           # Alle verfügbaren Befehle
php cli/joomla.php akeeba:backup  # Manuelles Backup auslösen
php cli/joomla.php cache:clean    # Cache leeren
```

---

## Backup-Strategie

### Automatisch (Akeeba Pro)
| Typ | Zeitplan | Ziel |
|-----|----------|------|
| Inkrementell | Täglich 06:00 | Google Drive (Ordner: 10 – Backups) |
| Komplett | Donnerstags 05:00 | Google Drive (Ordner: 10 – Backups) |

Lokale Quota: 3 Backups behalten.

### Bei jedem deploy() automatisch
`db-backup/dump.sql.gz` wird erstellt und mit committet → DB-Stand zu jedem Git-Commit wiederherstellbar.

### Vor kritischen Änderungen
Akeeba CLI-Backup auslösen:
```bash
ssh ins-user1@staging.insight.gift \
  "cd /var/www/vhosts/insight.gift/httpdocs/staging && php cli/joomla.php akeeba:backup"
```

Kritische Änderungen: Joomla-Updates, Extension-Installationen, DB-Strukturänderungen, Template-Umbau.

---

## Medien & Bilder

- Bilder liegen **ausschließlich auf dem Server** unter `/images/`
- NIEMALS lokal speichern, syncen oder ins Git-Repository committen
- `images/` ist in `.gitignore` ausgeschlossen
- Bildpfade in Templates/CSS immer relativ zum Joomla-Root
- Dateinamen auf Server abfragen:
  ```bash
  ssh ins-user1@staging.insight.gift \
    "find /var/www/vhosts/insight.gift/httpdocs/staging/images/ -type f"
  ```
- Neue Bilder: lokal mit ImageOptim/Squoosh optimieren → via Joomla Media Manager hochladen
- WebP Converter Plugin konvertiert automatisch bei Upload (sobald installiert)

---

## Browser-Testing Screenshots

- Speicherort: `/Users/manuelhorne/HorNet.Marketing/Web-Projekte/staging.insight.gift/screenshots/`
- Format: `YYYY-MM-DD_beschreibung.png`
- NIEMALS in `/tmp/` – wird bei Neustart geleert
- `screenshots/` ist in `.gitignore` (nicht versioniert)

---

## SEO

- Tool: claude-seo (installiert unter `~/.claude/skills/seo/`)
- Aufruf in Claude Code Chat: `Use the seo skill to analyze https://staging.insight.gift`
- Alle Seiten: Meta-Title und Meta-Description pflegen
- Strukturierte Daten (JSON-LD) für Artikel, Leistungen, Kontakt implementieren
- SEF URLs sind aktiv, `.htaccess` routet über `index.php`

---

## Content-Erstellung

- Claude schreibt Entwurf → Manuel/Daniela finalisiert
- Tonalität: Persönlich-empathisch, wissenschaftlich fundiert
- Zielgruppe: Gesundheitsbewusste, neurodiverse Privatpersonen
- Alle Texte auf Deutsch
- **Keine medizinischen Heilsversprechen** (rechtliche Absicherung)
- Bei Unsicherheit über rechtliche Unbedenklichkeit: Hinweis geben, nicht selbst entscheiden

---

## DSGVO

- Cookie-Banner: ⏳ Status prüfen und ggf. einrichten
- Datenschutzerklärung: ⏳ Auf Aktualität prüfen
- Impressum: ⏳ Auf Vollständigkeit prüfen

---

## Joomla Architektur

### Entry Points
- **Frontend:** `index.php` → `includes/app.php` → `libraries/bootstrap.php` → `SiteApplication`
- **Admin:** `administrator/index.php`
- **REST API:** `api/index.php`
- **CLI:** `cli/joomla.php`

### Joomla MVC Pattern
Extensions folgen `Component → Controller → Model → View`:
- Frontend: `components/com_*/src/{Controller,Model,View}/`
- Backend: `administrator/components/com_*/`
- Views: `tmpl/` Verzeichnisse

---

## Arbeitsregeln (NIEMALS verletzen)

### Vor jedem Vorschlag prüfen:
1. Gibt es eine elegantere Lösung die Redundanz vermeidet?
2. Widerspricht der Vorschlag Entscheidungen in dieser CLAUDE.md?
3. Ist der Vorschlag konsistent mit dem bestehenden Setup?
4. Gibt es ein etabliertes Tool/Pattern das besser passt als ein manueller Workaround?

### Technische Regeln:
- `configuration.php` NIEMALS committen, anzeigen oder verändern
- `images/` und alle Bilddateien NIEMALS committen
- Alle Dateien aus `.gitignore` NIEMALS committen
- DB-Änderungen NUR via MCP MySQL – niemals direkt via SSH-Befehle
- SSH-Tunnel NIEMALS selbst per Bash starten – ausschließlich über den `tunnel`-Alias (korrektes Port-Mapping 3306→33010)
- `deploy()` immer mit aussagekräftiger Commit-Nachricht aufrufen
- Vor jedem `deploy`: `git status` prüfen
- `libraries/` und Core-Dateien NIEMALS modifizieren
- NIEMALS direkt auf dem Server arbeiten (kein SSH-Edit, kein FTP)

### Workflow-Regeln:
- Nach Backend-Änderungen im Joomla-Admin: `sync-from-server` → prüfen → `deploy "beschreibung"`
- Für DB-Zugriff: `tunnel` starten
- Ins Projektverzeichnis wechseln: `staging`
- Vor kritischen Änderungen: Akeeba-Backup auslösen
- Bilder ausschließlich via Joomla Media Manager hochladen

### Inhaltliche Regeln:
- Keine medizinischen Heilsversprechen in Texten
- Bei rechtlicher Unsicherheit: Hinweis geben, nicht selbst entscheiden
- Alle Texte auf Korrektheit, Tonalität und Zielgruppen-Fit prüfen

---

## Offene Aufgaben (TODO)

- [ ] HikaShop deinstallieren
- [ ] WebP Converter Plugin (JoomIQ) installieren und konfigurieren
- [ ] DSGVO prüfen (Cookie-Banner, Datenschutz, Impressum)
- [ ] Demo-Platzhalter ersetzen (Kontaktdaten, Telefon, E-Mail)
- [ ] Startseite aufbauen (Struktur oben definiert)
- [ ] Über-mich-Seite erstellen
- [ ] Leistungsseiten erstellen
- [ ] Blog einrichten
- [ ] SEO-Grundoptimierung (Meta-Tags, Sitemap, Schema.org)
- [ ] Kundenstimmen/Testimonials einpflegen
- [ ] Launch-Workflow (configuration.php für Live, neues Git-Repo/Webhook)
# CLAUDE.md – Projektkontext insight.gift

> **Verbindliche Referenz** für alle Arbeiten an diesem Projekt.
> Änderungen an gesetzten Inhalten und Entscheidungen nur auf explizite Anweisung.

---

## Projektübersicht

**Website:** https://staging.insight.gift (Staging) / https://insight.gift (Live, noch nicht aktiv)
**Betreiberin:** Daniela Horne
**Thema:** Epigenetik, Funktionelle Medizin, ganzheitliche Gesundheit – „Zelle für Zelle in die Balance"
**Ziel:** Informationsseite + Lead-Generierung (kostenloses Erstgespräch)
**Zielgruppe:** Gesundheitsbewusste Privatpersonen, neurodiverse Menschen (ADS/ADHS/ASS)
**Tonalität:** Persönlich-empathisch, wissenschaftlich fundiert, Du-Ansprache
**Sprache:** Deutsch

---

## Technische Infrastruktur

**CMS:** Joomla! 5.3.4
**PHP:** 8.3 (Minimum 8.1)
**Datenbank:** MySQL – Name: `danielahorne_jom`, Präfix: `jins_`, Host: `127.0.0.1:33010`
**Timezone:** Europe/Berlin
**Template:** Narioz (Helix Ultimate Framework, JoomShaper) – aktiver Style: **„insight – Standard"** (home=1)
**Page Builder:** SP Page Builder (`com_sppagebuilder`)
**Backup:** Akeeba Backup Pro

**Lokaler Pfad:** `/Users/manuelhorne/HorNet.Marketing/Web-Projekte/staging.insight.gift/`
**Git:** `git@github.com:hornet-marketing/staging.insight.gift.git` (Private, Branch: main)
**Screenshots:** Immer in `screenshots/` im Projektverzeichnis speichern – **niemals in /tmp/**. Dateiname: `YYYY-MM-DD_beschreibung.png`

### CLI
```bash
php cli/joomla.php <command>   # Joomla CLI-Tasks
php cli/joomla.php list        # Verfügbare Befehle
```
Kein Build-Pipeline. Kein top-level `composer.json`.

### Deployment-Workflow
```bash
tunnel            # SSH-Tunnel zur DB öffnen (Pflicht vor DB-Zugriff)
staging           # Ins Projektverzeichnis + git pull
sync-from-server  # Server → lokal (nach Backend-Änderungen im Joomla-Admin)
deploy "msg"      # DB-Dump + git push → Webhook → Plesk auto-deploy
```
**Regel:** Nach Backend-Änderungen immer: `sync-from-server` → `deploy "beschreibung"`
**Regel:** Vor jedem `deploy` git status prüfen.

---

## Arbeitsregeln – NIEMALS verletzen

### Vor jedem Vorschlag prüfen:
1. Gibt es eine elegantere Lösung, die Redundanz vermeidet?
2. Widerspricht der Vorschlag getroffenen Entscheidungen in dieser CLAUDE.md?
3. Ist der Vorschlag konsistent mit dem bestehenden Setup?
4. Gibt es ein etabliertes Tool/Pattern, das besser passt als ein manueller Workaround?

### Technische Regeln:
- `configuration.php` **niemals** committen, anzeigen oder verändern
- Dateien aus `.gitignore` **niemals** committen
- DB-Änderungen **nur via MCP MySQL** – niemals direkt via SSH-Befehle
- `deploy()` immer mit aussagekräftiger Commit-Nachricht aufrufen
- **Niemals** direkt auf dem Server arbeiten (kein „Option C")

### Content-Regeln:
- **Keine medizinischen Heilsversprechen** – Formulierungen wie „heilt", „kuriert", „behandelt" vermeiden
- **Texte nicht eigenständig umformulieren** – alle Wordings sind freigegeben und liegen kanonisch in der Datenbank
- **Texte vor Überschreiben schützen:** Bei jeder Aufforderung zur Texteingabe zuerst prüfen, ob für dieses Element bereits ein Text in der DB existiert. Falls ja: **nachfragen, ob der bestehende Text geändert werden soll** – niemals stillschweigend überschreiben
- **Platzhalter für Fotos lassen** – niemals durch Stock-Fotos ersetzen
- **Alle Texte auf Deutsch**

### Medien & Bilder:
- Bilder liegen **ausschließlich** auf dem Server unter `/images/` – niemals lokal speichern oder committen
- `images/` ist in `.gitignore` – **niemals** committen
- Bildpfade immer relativ zum Joomla-Root
- Dateinamen abfragen via SSH:
  `ssh ins-user1@staging.insight.gift "find /var/www/vhosts/insight.gift/httpdocs/staging/images/ -type f"`

---

## Architecture

### Entry Points
- **Frontend:** `index.php` → `includes/app.php` → `libraries/bootstrap.php` → `SiteApplication`
- **Admin:** `administrator/index.php`
- **REST API:** `api/index.php`
- **CLI:** `cli/joomla.php`

### Key Directories
- `configuration.php` — Haupt-Site-Config (niemals anfassen)
- `templates/narioz/` — Aktives Custom-Template (Helix Ultimate)
- `templates/narioz/css/custom.css` — Projektspezifische CSS-Anpassungen (hier arbeiten)
- `templates/narioz/css/overrides.css` — Zusätzliche Overrides
- `templates/narioz/html/` — Joomla Template-Overrides (nicht ohne Grund anfassen)
- `libraries/src/` — Joomla Framework Core (niemals ändern)
- `libraries/vendor/` — Composer Dependencies (niemals ändern)
- `media/` — Frontend Assets per Komponente/Plugin

### CSS-Dateien (templates/narioz/css/)
| Datei | Zweck | Anfassen? |
|-------|-------|-----------|
| `custom.css` | Projektspezifische Anpassungen | ✅ Ja |
| `overrides.css` | Zusätzliche Overrides | ✅ Ja |
| `template.css` | Haupt-Template-Styles | ⚠️ Nur wenn nötig |
| `bootstrap.min.css`, `font-awesome.min.css`, `flaticon.css`, `linear.css`, `v4-shims.min.css` | Bibliotheken | ❌ Niemals |

### Template-Overrides (templates/narioz/html/)
Nicht ohne explizite Anweisung anfassen:
`com_contact`, `com_content`, `com_finder`, `com_media`, `com_search`, `com_tags`, `com_users`, `mod_articles_categories`, `mod_articles_latest`, `mod_breadcrumbs`, `mod_languages`, `mod_login`, `mod_menu`, `mod_search`

### Datenbank
Prefix: `jins_`. Core-Tabellen: `jins_content`, `jins_users`, `jins_menu`, `jins_extensions`, `jins_modules`.
SEF-URLs aktiv – `.htaccess` routet alle Requests via `index.php`.

---

## Website-Stand – nicht unaufgefordert ändern

### SP Page Builder – Seiten (jins_sppagebuilder)
| ID | Titel | Verwendung |
|----|-------|------------|
| 1 | Home Page 01 | Template-Demo |
| 2 | Default Top Details | Modul `top1` – Header-Topbar |
| 3 | Default Footer | Modul `footer1` – Footer |
| 4 | Bottom Clients | Modul `bottom1` – Logos-Bereich |
| 5 | Home Page 02 | Template-Demo |
| 6 | Home 02 Header Search, Cart and Phone | Modul `position1` – Header |
| 7–13 | About Us, Services, Service Details, Team Grid, Team Details, Testimonial, FAQ | Template-Demos |
| 14 | Page 404 | Fehlerseite |
| 15 | Contact | Kontaktseite |
| 16 | Sidebar | Modul `right` |
| 17 | Shop Sidebar | Modul `left` |
| **18** | **insight – Home** | **Aktive Startseite** (Menü-Home → id=18) |

### Menüstruktur (mainmenu)
- **Home** → SP Page Builder id=18
- **Pages / Shop / News (2)** → Platzhalter, noch leer. Shop deinstalliert – nicht befüllen.
- **Contact (2)** → SP Page Builder id=15
- Suffix `(2)` = Duplikate der Template-Demo-Einträge aus `mainmenutmpl`

### Module (frontend, published)
| Position | Modul | SP-Seite |
|----------|-------|----------|
| `top1` | mod_sppagebuilder | id=2 |
| `position1` | mod_sppagebuilder | id=6 |
| `footer1` | mod_sppagebuilder | id=3 |
| `bottom1` | mod_sppagebuilder | id=4 |
| `breadcrumb` | mod_breadcrumbs | — |
| `right` | mod_sppagebuilder | id=16 |
| `left` | mod_sppagebuilder | id=17 |

---

## Corporate Design

### Farben (im Helix Ultimate Template Manager konfigurieren)
```css
--maincolor:   #30c196;  /* Grün/Türkis – Primärfarbe */
--secondcolor: #ff82f2;  /* Rose dunkel */
--thirdcolor:  #e4abff;  /* Purple hell */
--fourthcolor: #ffc1f9;  /* Rose hell */
--fifthcolor:  #b66bda;  /* Purple dunkel */
/* Fließtext: #595959 (Anthrazit) */
```

### Schriften
- **Fließtext:** Questrial (Google Fonts)
- **Headings:** Quigley (Custom Font – Verfügbarkeit auf Server prüfen, bei Fehlen melden)

### Logos (auf dem Server unter /images/)
- Hell/Dunkel-Hintergrund: `Logo_insight_komplett_weiss_transp_500x250_72dpi.png`
- Heller Hintergrund: `Logo_insight_komplett_farbig_transp_500x250_72dpi.png`

### Favicon
Liegt auf dem Server – nicht ersetzen ohne explizite Anweisung.

---

## Kontakt & Rechtliches

```
Daniela Horne · Am Berg 7 · 64732 Bad König
Tel.: +49 173 5883568 · daniela@insight.gift
```
- Freiberuflich (Coach/Beraterin), kein Gewerbe
- Kleinunternehmerregelung §19 UStG
- Keine Heilpraktiker-Zulassung

**Booking-Link (alle CTAs):** https://zeeg.me/insight/erstgespraech – immer `target="_blank"`

---

## Offene Aufgaben

### Sofort umsetzbar
- [ ] Quigley-Schrift auf Server prüfen – bei Fehlen melden, nicht eigenständig ersetzen
- [ ] Farben & Schriften im Helix Ultimate Template Manager konfigurieren
- [ ] Demo-Platzhalter im Header ersetzen: Telefon `+92 (8800) - 98670` → `+49 173 5883568`, E-Mail `help@company.com` → `daniela@insight.gift`
- [ ] Startseite (id=18) in SP Page Builder aufbauen
- [ ] Impressum als Joomla-Artikel anlegen
- [ ] Favicon prüfen
- [ ] Demo-Menüeinträge mit Suffix `(2)` bereinigen

### Wartet auf Zulieferung – Platzhalter lassen
- [ ] Profilfoto(s) Daniela
- [ ] Weitere Testimonials (3–5)
- [ ] Social Media Handles (Facebook, LinkedIn, XING)
- [ ] Newsletter-Anbieter

### Nächste Bauphasen
- [ ] Datenschutzerklärung
- [ ] Über-mich-Seite
- [ ] Blog (Demo-Artikel ersetzen)
- [ ] Kontaktseite
- [ ] Menüstruktur bereinigen
- [ ] SEO-Grundoptimierung (`~/.claude/skills/seo/`)

### Nicht geplant
- Shop: deinstalliert, wird nicht genutzt
- WebP-Optimierung: wird extern erledigt

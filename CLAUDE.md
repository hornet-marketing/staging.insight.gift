# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Joomla! 5.3.4** installation powering "insight – Zelle für Zelle in die Balance durch Epigenetik und Funktionelle Medizin" — a German-language health/epigenetics e-commerce site.

- **PHP**: 8.3 (minimum 8.1)
- **Database**: MySQL (`danielahorne_jom`, prefix `jins_`, host `127.0.0.1:33010`)
- **Timezone**: Europe/Berlin

## CLI Commands

```bash
# Run Joomla CLI tasks (updates, GC, etc.)
php cli/joomla.php <command>

# List available CLI commands
php cli/joomla.php list
```

No build pipeline — static assets are compiled externally and committed. Composer is used only by vendor packages (no top-level composer.json to run).

## Architecture

### Entry Points
- **Frontend**: `index.php` → `includes/app.php` → `libraries/bootstrap.php` → `SiteApplication`
- **Admin**: `administrator/index.php`
- **REST API**: `api/index.php`
- **CLI**: `cli/joomla.php`

### Key Directories
- `configuration.php` — main site config (DB credentials, cache, mail, SEF settings)
- `templates/narioz/` — active custom template (Helix Ultimate framework, JoomShaper)
- `components/` — frontend MVC components (HikaShop, SP Page Builder, content, etc.)
- `administrator/components/` — backend admin components
- `plugins/` — event-driven extensions (system, user, task, workflow, webservices)
- `modules/` — reusable page blocks (frontend + admin)
- `libraries/src/` — Joomla framework core classes
- `libraries/vendor/` — Composer dependencies (Symfony, PayPal SDK, scssphp)
- `media/` — frontend assets (JS, CSS, images per component/plugin)

### Key Extensions
- **HikaShop** (`com_hikashop`) — e-commerce (products, orders, payments, shipping)
- **SP Page Builder** (`com_sppagebuilder`) — drag-and-drop frontend page editor
- **Akeeba Backup** (`com_akeeba`) — site backup/restore
- **Helix Ultimate** (system plugin + `narioz` template) — template framework

### Joomla MVC Pattern
Extensions follow `Component → Controller → Model → View` structure:
- Frontend: `components/com_*/` with `src/Controller/`, `src/Model/`, `src/View/`
- Backend: `administrator/components/com_*/`
- Views render via layout files in `tmpl/` directories

### Database
Table prefix is `jins_`. Core tables: `jins_content`, `jins_users`, `jins_menu`, `jins_extensions`, `jins_modules`. HikaShop uses `jins_hikashop_*` tables.

### URL Routing
SEF URLs are enabled. `.htaccess` handles Apache rewrites to route all requests through `index.php`.

## Customization Locations

When making site changes, work in:
- `templates/narioz/` — theme overrides, CSS, JS, template params
- `templates/narioz/html/` — Joomla template overrides (copy core views here to override)
- `plugins/system/` — site-wide behavior hooks
- Custom component or module directories under `components/` / `modules/`

Avoid modifying files under `libraries/` or core `includes/` — these are Joomla framework files that will be overwritten on updates.

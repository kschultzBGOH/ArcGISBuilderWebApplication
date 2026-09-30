# ArcGIS Runner Config Site

## What this project is

ArcGIS Runner Config Site is a **PHP + React web application** that lets you visually
configure which ArcGIS feature layers and tables should be exposed in the ArcGIS Runner
widget (the paired Experience Builder widget), and what fields should be visible/editable
for each layer.

Workflow:
1. You log in with your ArcGIS Portal credentials
2. The site introspects your Portal and lists available webmaps, feature services, and
   feature/table layers within them
3. For each layer you want to expose in Runner, you configure:
   - Which fields are displayed in the list view (and in what order)
   - Which fields are editable in the add/edit form
   - Custom field labels/aliases
   - Default sort field/order and page size
4. The site generates a **JSON config file** and stores it on your org's network drive
5. The paired ArcGIS Runner widget loads that JSON file and uses it to drive its CRUD UI

This config site is the **single source of truth** for layer/field configuration. The widget
simply reads and obeys the JSON — it does not have a settings panel of its own.

## Locked-in architecture decisions

- **Backend**: PHP 8.4.25, handles Portal authentication and generates/writes JSON config
  files to a network drive. No complex business logic — mostly API proxying to Portal and
  file I/O.
- **Frontend**: React (TypeScript), handles UI for layer picking, field selection, and
  configuration. Does NOT hit Portal/feature services directly — all data flows through
  the PHP backend.
- **Authentication**: Portal OAuth2 flow (handled by PHP backend). The Portal token is
  kept in the PHP session and never sent to the browser; the frontend only calls PHP endpoints.
- **Config storage**: one JSON file per webmap on an org network drive, e.g.
  `{CONFIG_ROOT}/{webmapId}.json`. `CONFIG_ROOT` is an env var pointing at the
  mounted share (the PHP server's service account needs write access).
- **Config delivery**: browsers cannot read UNC/network-drive paths, so the widget
  never touches the drive. The PHP backend exposes a read-only
  `GET /api/configs/{webmapId}` endpoint (CORS allowed for the ExB host origin), and the
  widget's only design-time setting is that config URL.
- **Data flow**:
  - Frontend → PHP API (webmaps, layers, field metadata, save config)
  - PHP → Portal REST API (introspect webmaps/layers with the user's token)
  - PHP → network drive (write/read JSON)
  - ExB widget → `GET /api/configs/{webmapId}` (read JSON over HTTP)

## Open decisions (resolve before Phase 1 build)

- PHP framework: Slim 4 (lightweight, recommended) vs Laravel vs plain PHP
- Frontend build tooling: Vite (recommended)
- Portal version (Enterprise 10.9 / 11.x) and whether an OAuth app registration exists
- Who may save configs — any Portal user, or a Portal group/role check?

## Repo layout

```
/CLAUDE.md                          <- this file (brain doc)
/docs/
  TASKS.md                          <- living backlog
  CONFIG_OUTPUT_SCHEMA.md           <- the JSON schema the widget consumes (source of truth)
  NETWORK_DRIVE_SETUP.md            <- instructions for setting up network drive access
/backend/                           <- PHP 8.4.25 API server
  .env.example
  composer.json
  src/
    controllers/
      AuthController.php
      LayerController.php
      ConfigController.php
    services/
      PortalService.php             <- Portal auth + layer introspection
      ConfigService.php             <- generates JSON config
      StorageService.php            <- writes to network drive
    middleware/
    models/
      WebmapModel.php
      LayerModel.php
      ConfigModel.php
/frontend/                          <- React + TypeScript UI
  package.json
  tsconfig.json
  src/
    pages/
      LoginPage.tsx
      WebmapSelector.tsx
      LayerConfigurator.tsx
      ConfigPreview.tsx
    components/
      FieldPicker.tsx
      FieldOrder.tsx
      FieldLabels.tsx
      ConfigForm.tsx
    hooks/
      useAuth.ts
      useWebmaps.ts
      useLayers.ts
    services/
      api.ts                        <- HTTP client for backend endpoints
```

## Core data model

One JSON file per webmap (`RunnerConfig`, versioned by `schemaVersion`) holding an
ordered list of `LayerConfig` entries: enable flag, ordered visible fields, ordered
editable fields, label overrides, sort, page size, and a capability snapshot.

**`docs/CONFIG_OUTPUT_SCHEMA.md` is the single authoritative definition** — it is
the contract with the widget repo. Don't redefine the shape anywhere else.

## Phasing

**Phase 1 — Authentication & Portal introspection**
- PHP backend: Portal OAuth2 setup, token refresh
- Frontend: Login page
- Backend: Webmap/layer listing endpoints
- Frontend: Webmap selector UI

**Phase 2 — Layer inspection & field configuration UI**
- Backend: Layer schema introspection (fields, types, domains, editability)
- Frontend: Layer picker
- Frontend: Field visibility checkbox list (with drag-to-reorder)
- Frontend: Field editability checkbox list

**Phase 3 — Config generation & storage**
- Backend: Generate JSON config from form input
- Backend: Write JSON to network drive
- Backend: Public read endpoint `GET /api/configs/{webmapId}` with CORS for the ExB origin
- Frontend: Config preview/review before save
- Frontend: Save button, success feedback

**Phase 4 — Polish**
- Session/token management (refreshes, timeouts)
- Error handling & validation
- i18n (if needed)
- Network drive path configuration (settings/env vars)

## How work gets done

Same as ArcGIS Runner widget: this chat is the **brain**, and builder sessions
are spawned for individual tasks. CLAUDE.md is the single source of truth —
keep it current.

## Conventions

- TypeScript strict, no `any` unless unavoidable.
- PHP: PSR-4 autoloading, dependency injection where reasonable.
- No comments unless *why* is non-obvious.
- Config JSON schema is the contract between this site and the widget — keep
  `docs/CONFIG_OUTPUT_SCHEMA.md` in sync with what the backend actually outputs.

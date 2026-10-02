# ArcGIS Builder Web Application

## What this project is

ArcGIS Builder Web Application is a **Laravel (PHP 8.4) + React web application** that lets you visually
configure which ArcGIS feature layers and tables should be exposed in the ArcGIS Runner
widget (the paired Experience Builder widget), and what fields should be visible/editable
for each layer.

Workflow:
1. You log in with your ArcGIS Portal credentials (only members of the designated
   Portal group may use the site)
2. The site introspects your Portal and lists available webmaps, feature services, and
   feature/table layers within them
3. For each layer you want to expose in Runner, you configure:
   - Which fields are displayed in the list view (and in what order)
   - Which fields are editable in the add/edit form
   - Custom field labels/aliases
   - Default sort field/order and page size
4. The site generates a **JSON config file** and stores it on your org's network drive
5. The paired ArcGIS Runner widget loads that JSON file and uses it to drive its CRUD UI

This builder app is the **single source of truth** for layer/field configuration. The widget
simply reads and obeys the JSON — it does not have a settings panel of its own.

## Locked-in architecture decisions

- **Backend**: Laravel (current major release supporting PHP 8.4) on PHP 8.4.25, on the
  org's internal servers. Mostly Portal API proxying and file I/O, no heavy business logic.
- **Frontend**: React + TypeScript single-page app living in Laravel's `resources/js`,
  built with Vite (`laravel-vite-plugin`), and Calcite Components for UI. It is served
  by Laravel from the same origin, so one deploy and plain session cookies with no
  token handling in the browser. The SPA never calls Portal or feature services
  directly; everything goes through Laravel `/api` routes.
- **Portal**: ArcGIS Enterprise **12.0**. All Portal calls go through one `PortalClient`
  service (`{PORTAL_URL}/sharing/rest/...`) so version-specific quirks stay in one place.
- **Authentication**: Portal OAuth2 authorization-code flow using an OAuth app the
  org registers in Portal (client id + secret in `.env`; redirect URI
  `{APP_URL}/auth/callback`). Laravel holds the Portal access/refresh tokens in the
  server-side session, and they never reach the browser.
- **Authorization**: only members of one Portal group may sign in. At login, and
  again on every save, Laravel checks the user's groups via
  `/sharing/rest/community/self` against `PORTAL_ALLOWED_GROUP_ID`. Non-members get
  a clear "not authorized" page and no session.
- **Config storage**: a dedicated Laravel filesystem disk `runner_configs` whose
  root is `CONFIG_ROOT` (the mounted network share; the PHP service account needs
  write access). One file per webmap: `{webmapId}.json`. Writes are atomic
  (temp file + rename).
- **Config delivery**: browsers can't read network-drive paths, so the widget never
  touches the drive. Laravel serves `GET /api/configs/{webmapId}` read-only, with CORS
  restricted to `RUNNER_ALLOWED_ORIGINS` (the ExB host). The widget's only design-time
  setting is that URL.
- **Data flow**:
  - SPA → Laravel `/api` (webmaps, layers, field metadata, save config)
  - Laravel → Portal 12.0 REST (as the signed-in user, with their token)
  - Laravel → network share (read/write JSON)
  - ExB widget → `GET /api/configs/{webmapId}`

### Environment variables (`.env`)

| Var | Purpose |
|---|---|
| `PORTAL_URL` | e.g. `https://gis.example.org/portal` |
| `PORTAL_OAUTH_CLIENT_ID` / `PORTAL_OAUTH_CLIENT_SECRET` | from the Portal OAuth app registration |
| `PORTAL_ALLOWED_GROUP_ID` | Portal group whose members may use the site |
| `CONFIG_ROOT` | mount path of the network share for config JSON |
| `RUNNER_ALLOWED_ORIGINS` | comma-separated ExB origins allowed to read configs |

## Repo layout

```
/CLAUDE.md                         <- this file (brain doc)
/docs/
  TASKS.md                         <- living backlog
  CONFIG_OUTPUT_SCHEMA.md          <- JSON contract with the widget (source of truth)
  DEPLOYMENT.md                    <- server, network share mount, Portal OAuth app setup
/app/
  Http/
    Controllers/
      AuthController.php           <- login redirect, callback, logout
      WebmapController.php
      LayerController.php
      ConfigController.php         <- save (auth) + public read
    Middleware/
      EnsurePortalGroupMember.php
  Services/
    PortalClient.php               <- all Portal REST calls + token refresh
    ConfigBuilder.php              <- form input -> RunnerConfig JSON
    ConfigStore.php                <- runner_configs disk read/atomic write
/config/runner.php                 <- typed access to the env vars above
/routes/
  web.php                          <- /auth/*, SPA catch-all
  api.php                          <- /api/*
/resources/js/                     <- React + TS SPA (Vite)
  pages/        (Login, WebmapSelector, LayerConfigurator, ConfigPreview)
  components/   (FieldPicker, FieldOrder, FieldLabels, ...)
  api/          (typed client for /api)
/tests/                            <- PHPUnit/Pest feature tests (Portal faked via Http::fake)
```

## Core data model

One JSON file per webmap (`RunnerConfig`, versioned by `schemaVersion`) holding an
ordered list of `LayerConfig` entries: enable flag, ordered visible fields, ordered
editable fields, label overrides, sort, page size, and a capability snapshot.

**`docs/CONFIG_OUTPUT_SCHEMA.md` is the single authoritative definition** — it is
the contract with the widget repo. Don't redefine the shape anywhere else.

## Phasing

**Phase 1 — Authentication & Portal introspection**
- Portal OAuth2 login/callback/logout, token refresh, group-membership gate
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
- Session timeout UX
- Error handling & validation
- i18n (if needed)
- Network drive path configuration (settings/env vars)

## How work gets done

Same as ArcGIS Runner widget: this chat is the **brain**, and builder sessions
are spawned for individual tasks. CLAUDE.md is the single source of truth —
keep it current.

## Conventions

- TypeScript strict, no `any` unless unavoidable.
- PHP: Laravel conventions (controllers thin, logic in `app/Services`, constructor
  injection). Portal is faked in tests with `Http::fake()`; nothing hits a real Portal in CI.
- No comments unless *why* is non-obvious.
- Config JSON schema is the contract between this site and the widget — keep
  `docs/CONFIG_OUTPUT_SCHEMA.md` in sync with what the backend actually outputs.

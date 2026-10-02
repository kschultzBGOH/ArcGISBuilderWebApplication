# ArcGIS Builder Web Application

## What this project is

A **Laravel (PHP 8.4) + React** web app, in the spirit of PHPRunner, for building
ArcGIS Runner apps without code. You walk through a 7-step wizard against a
Portal 12.0 webmap, and the result is one JSON config per webmap. The paired
**ArcGIS Runner** Experience Builder widget (`kschultzBGOH/ArcGISRunner`) loads
that config and renders the list/add/edit/view/delete pages it describes.

Only members of one designated Portal group may sign in.

This app does two jobs:
1. **Builder** — the wizard UI and the config files it produces.
2. **Runtime backend for the widget** — serves configs and is the single path
   for all widget *writes* (add/update/delete), so server-side PHP hooks and
   config rules are enforced on every edit.

## The wizard

Each step is a page in the SPA. Progress autosaves to a server-side **draft**;
**Publish** (end of step 7) writes the live config the widget reads.

1. **Select webmap** — search the webmaps the signed-in user can access in Portal.
2. **Layers & fields** — auto-detect every feature layer and table in the webmap
   (group layers flattened). Show each one's fields with their field types.
   Select which layers and which fields to include.
3. **Pages** — for each selected layer, checkboxes: **List**, **Add**, **Edit**,
   **View**, **Delete**. A box is disabled when the service doesn't support it
   (Add needs `supportsAdd`, Edit needs `supportsUpdate`, Delete needs `supportsDelete`).
4. **Input types** — for each field, choose how it's entered/displayed. Choices
   are filtered to what's valid for the field type (see "Input types").
5. **Designer** — per layer, per page: List page = ordered columns (plus default
   sort and page size); Add/Edit/View pages = fields grouped into titled
   **sections**, drag to reorder sections and the fields inside them.
6. **Custom CSS** — one stylesheet per config, applied only inside the widget.
7. **Custom code** — per layer:
   - **JavaScript** event handlers, written in the builder, run by the widget in
     the browser (see "Custom JavaScript").
   - **PHP hook** — choose one hook class from the code that's deployed. The PHP
     itself is never typed into the builder (see "PHP hooks").

Then **Review & Publish**: show the config as JSON and publish it.

## Locked-in architecture decisions

- **Backend**: Laravel (current major release supporting PHP 8.4) on PHP 8.4.25,
  on the org's internal servers.
- **Frontend**: React + TypeScript SPA in `resources/js`, built with Vite
  (`laravel-vite-plugin`), Calcite Components for UI. Served by Laravel from the
  same origin, using session cookies. The SPA never calls Portal directly.
- **Portal**: ArcGIS Enterprise **12.0**. Every Portal/feature-service call goes
  through `PortalClient` so version quirks live in one place.
- **Builder authentication**: Portal OAuth2 authorization-code flow with the
  org's registered OAuth app (redirect URI `{APP_URL}/auth/callback`). Portal
  tokens stay in the server session and never reach the browser.
- **Builder authorization**: only members of `PORTAL_ALLOWED_GROUP_ID`, checked at
  login and again on every save/publish via `/sharing/rest/community/self`.
- **Widget authentication**: widget users are ordinary Portal users, not
  necessarily in the builder group. The widget sends the user's own Portal token
  (`Authorization: Bearer`). Laravel verifies it against Portal and forwards it to
  the feature service, so service permissions and editor tracking apply to that user.
- **Config storage**: Laravel disk `runner_configs`, root `CONFIG_ROOT` (the mounted
  network share). Live configs are `{webmapId}.json`; drafts are
  `drafts/{webmapId}.json`. Writes are atomic (temp file + rename).
- **Widget endpoints** (CORS limited to `RUNNER_ALLOWED_ORIGINS`, Portal token required):
  - `GET /api/runtime/configs/{webmapId}` — the live config
  - `POST /api/runtime/edits/{webmapId}/{layerId}` — add/update/delete. Laravel
    rejects anything the config doesn't allow (page disabled, field not
    selected). It then runs the layer's PHP `before*` hook, calls the feature
    service `applyEdits` with the user's token, and runs the `after*` hook.
- **Reads** (List/View queries) go straight from the widget to the feature service
  through `@arcgis/core`. Only writes go through Laravel.

## Input types

Each input type is a key such as `text`, plus optional `inputOptions`. The
registry in the builder (`app/Runner/InputTypes.php`) and the widget's
renderers must use the same keys. New types get added over time; adding one is
a schema change, so update `docs/CONFIG_OUTPUT_SCHEMA.md` and the widget too.

Starting set:

| Key | Valid for |
|---|---|
| `text` | string |
| `textarea` | string |
| `number` | integer, small integer, double, single |
| `date` | date, date-only |
| `datetime` | date |
| `dropdown` | any field with a coded-value domain |
| `readonly` | any field (shown, never editable) |

## Custom JavaScript

Handlers are stored as source text in the config. The widget runs each one as a
function that receives a `ctx` object (layer, page, feature attributes,
`setValue`, `cancel(message)`). The event set and `ctx` shape are defined in
`docs/CONFIG_OUTPUT_SCHEMA.md`.

**Risk:** this code runs in every widget user's browser with their Portal
session, so anyone in the builder group can run code as other users. Keep the
group small. Also, Experience Builder's Content-Security-Policy may block
running code from text. That gets an early spike task before any widget work
depends on it.

## PHP hooks

Hooks are PHP classes in `app/Hooks/` that implement `App\Runner\LayerHook`
(`beforeAdd`, `afterAdd`, `beforeUpdate`, `afterUpdate`, `beforeDelete`,
`afterDelete`). `before*` hooks can change attributes or throw
`HookRejected($message)`, and that message is shown to the widget user.

Hooks go through git and code review, and are deployed like any other code. The
builder discovers the deployed hook classes and lets you pick one per layer.
**The server never runs PHP text that came from a config or the browser.**

### Environment variables (`.env`)

| Var | Purpose |
|---|---|
| `PORTAL_URL` | e.g. `https://gis.example.org/portal` |
| `PORTAL_OAUTH_CLIENT_ID` / `PORTAL_OAUTH_CLIENT_SECRET` | Portal OAuth app registration |
| `PORTAL_ALLOWED_GROUP_ID` | group whose members may use the builder |
| `CONFIG_ROOT` | mount path of the network share |
| `RUNNER_ALLOWED_ORIGINS` | comma-separated Experience Builder origins allowed to call the runtime endpoints |

## Repo layout

```
/CLAUDE.md
/docs/
  TASKS.md                     <- backlog
  CONFIG_OUTPUT_SCHEMA.md      <- JSON contract with the widget (only definition)
  DEPLOYMENT.md                <- server, share mount, Portal OAuth app
/app/
  Http/Controllers/
    AuthController.php         <- login, callback, logout
    Builder/                   <- webmaps, layers, drafts, publish (group-gated)
    Runtime/                   <- configs + edits for the widget (Portal-token-gated)
  Http/Middleware/
    EnsurePortalGroupMember.php
    VerifyPortalToken.php
  Runner/
    InputTypes.php             <- input type registry
    LayerHook.php, HookRejected.php, HookRegistry.php
    EditGate.php               <- enforces config rules on incoming edits
  Hooks/                       <- per-layer PHP hooks (reviewed code)
  Services/
    PortalClient.php
    ConfigStore.php            <- drafts + live, atomic writes
/config/runner.php
/routes/web.php, /routes/api.php
/resources/js/
  wizard/steps/                <- one folder per wizard step
  components/
  api/
/tests/                        <- Pest/PHPUnit, Portal faked with Http::fake()
```

## Core data model

One `RunnerConfig` per webmap, versioned by `schemaVersion`: selected layers,
selected fields with input types, enabled pages, layout per page, custom CSS,
and per-layer JS handlers and PHP hook choice.
**`docs/CONFIG_OUTPUT_SCHEMA.md` is the only definition** — don't redefine it.

## Phasing

1. **Foundation** — scaffold, Portal sign-in + group check, drafts, step 1.
2. **Wizard steps 2–4** — layer/field detection, pages, input types.
3. **Steps 5 & 6** — designer (sections + drag reorder), custom CSS.
4. **Publish + runtime** — review/publish, runtime config endpoint, edit endpoint
   with config enforcement.
5. **Step 7** — JS handlers (after the CSP spike), PHP hook system.
6. **Polish** — drift warnings (saved fields/layers no longer in the service),
   error handling, session timeout UX.

## How work gets done

The planning chat (the "brain") covers both this repo and the widget repo.
It keeps the CLAUDE.md files and backlogs current and hands each backlog task
to a separate builder session. Each builder session reads this file first,
does one task, and checks it off in `docs/TASKS.md`.

## Conventions

- TypeScript strict; no `any` unless unavoidable.
- Laravel conventions: thin controllers, logic in `app/Services` / `app/Runner`,
  constructor injection. Portal is faked with `Http::fake()` in tests; tests
  never hit a real Portal.
- Comments only for a non-obvious *why*.
- Any change to config shape updates `docs/CONFIG_OUTPUT_SCHEMA.md` and bumps
  `schemaVersion` once the widget has shipped against it.

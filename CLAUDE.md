# ArcGIS Builder Web Application

## What this project is

A **Laravel (PHP 8.4) + React** web app, in the spirit of PHPRunner, for
generating Experience Builder widget functionality without writing code.

- You build **profiles** in a wizard. A profile is a named, published
  configuration of a given **kind** (the first kind is `crud`: list/add/edit/view/delete
  over feature layers and tables; more kinds come later).
- There is **one** Experience Builder widget, **ArcGIS Runner**
  (`kschultzBGOH/ArcGISRunner`), registered in Portal once. When an app author
  adds it to an experience, they pick a profile in its settings panel. The
  widget loads that profile and renders the matching kind.
- Deploying a new "widget" means publishing a profile. No rebuild, no Portal
  registration, no file copying.

Only members of one designated Portal group may use the builder.

This app does three jobs:
1. **Builder** — the wizard UI and the profiles it produces.
2. **Widget host** — serves the compiled Runner widget files that Portal's
   registered widget item points at.
3. **Runtime backend** — serves published profiles to the widget and is the
   single path for every widget *write*, so config rules and PHP hooks are
   enforced on every edit.

## Kinds

A kind is a plug-in on both sides, sharing a `kind` key such as `crud`:
- **Builder** (`app/Runner/Kinds/{Kind}/` + `resources/js/wizard/kinds/{kind}/`):
  its wizard steps, its settings validator, and any runtime endpoints it needs.
- **Widget** (`widgets/arcgis-runner/src/runtime/kinds/{kind}/` in the widget repo):
  its React UI, loaded only when a profile of that kind is used.

Shared across all kinds: name, kind, target webmap, custom CSS, custom
JavaScript, publish/draft lifecycle. Adding a kind means adding both halves,
plus its settings shape in `docs/CONFIG_OUTPUT_SCHEMA.md`. A widget that
doesn't know a profile's kind shows "update the Runner widget" instead of
guessing.

## The wizard

Progress autosaves to a server-side **draft**. **Publish** makes the profile
available to the widget. Republishing updates every experience using that
profile.

Common steps (every kind):
- **Name & kind** — profile name and kind.
- **Select webmap** — search webmaps the signed-in user can access in Portal.
- *(kind-specific steps)*
- **Custom CSS** — one stylesheet per profile, applied only inside the widget.
- **Custom code** — JavaScript event handlers (events are defined by the kind)
  and, for kinds that write data, a PHP hook per layer.
- **Review & Publish** — show the profile JSON, validate it, publish.

`crud` kind steps:
1. **Layers & fields** — auto-detect every feature layer and table in the webmap
   (group layers flattened), show their fields and field types, select which
   to include.
2. **Pages** — per layer: **List**, **Add**, **Edit**, **View**, **Delete**.
   A box is disabled if the service doesn't support it.
3. **Input types** — per field, filtered to what's valid for its field type.
4. **Designer** — List: column order, sort, page size. Add/Edit/View: titled
   sections, drag to reorder sections and fields.

## Locked-in architecture decisions

- **Backend**: Laravel (current major supporting PHP 8.4) on PHP 8.4.25, on the
  org's internal servers.
- **Frontend**: React + TypeScript SPA in `resources/js`, built with Vite
  (`laravel-vite-plugin`), Calcite Components. Same-origin session cookies. The
  SPA never calls Portal directly.
- **Portal**: ArcGIS Enterprise **12.0** (Experience Builder 1.18, ArcGIS Maps SDK
  for JavaScript 4.33). All Portal calls go through `PortalClient`.
- **Builder auth**: Portal OAuth2 authorization-code flow (redirect
  `{APP_URL}/auth/callback`). Tokens stay in the server session.
- **Builder authorization**: members of `PORTAL_ALLOWED_GROUP_ID`, checked at
  login and on every save/publish.
- **Widget auth**: the widget sends the current user's Portal token
  (`Authorization: Bearer`). Laravel verifies it with Portal and uses it for that
  user's edits, so service permissions and editor tracking apply.
- **Storage** (Laravel disk `runner_configs`, root `CONFIG_ROOT` on the network share):
  - `profiles/{profileId}.json` — published
  - `drafts/{profileId}.json` — draft
  - Atomic writes (temp file + rename). `profileId` is a generated slug
    that never changes after creation.
- **Widget hosting**: the compiled widget (Developer Edition 1.18 build output) lives
  in `public/widgets/arcgis-runner/`, copied there by a deploy script and not
  committed. Portal's widget item points at
  `{APP_URL}/widgets/arcgis-runner/manifest.json`. The web server (not Laravel)
  must send CORS headers for that folder to the Portal origin.
- **Runtime endpoints** (Portal token required, CORS limited to `RUNNER_ALLOWED_ORIGINS`):
  - `GET /api/runtime/profiles?webmapId=` — published profiles (id, name, kind,
    webmapId) for the widget's settings dropdown
  - `GET /api/runtime/profiles/{profileId}` — one published profile
  - `POST /api/runtime/profiles/{profileId}/edits/{layerId}` — `crud` writes: config
    check (`EditGate`), PHP `before*` hook, `applyEdits` as the user, `after*` hook
- **Reads** (crud List/View) go straight from the widget to the feature service.

## Input types (`crud`)

A key plus optional `inputOptions`. The keys in `app/Runner/Kinds/Crud/InputTypes.php`
and the widget's renderers must match. Adding a key is a schema change.

| Key | Valid for |
|---|---|
| `text`, `textarea` | string |
| `number` | integer, small integer, double, single |
| `date` | date, date-only |
| `datetime` | date |
| `dropdown` | any field with a coded-value domain |
| `readonly` | any field |

## Custom JavaScript

Stored as source text in the profile and run by the widget with a `ctx` object.
Each kind defines its own event set (see the schema doc).
**Risk:** this code runs in every widget user's browser with their Portal
session, so keep the builder group small. Whether Experience Builder's
Content-Security-Policy allows it is an early spike task.

## PHP hooks

Classes in `app/Hooks/` implementing `App\Runner\LayerHook` (`beforeAdd`,
`afterAdd`, `beforeUpdate`, `afterUpdate`, `beforeDelete`, `afterDelete`).
`before*` hooks can change attributes or throw `HookRejected($message)`. Hooks
are reviewed in git and deployed like any other code. The builder only picks a
hook class per layer. **The server never runs PHP text from a profile or the browser.**

### Environment variables (`.env`)

| Var | Purpose |
|---|---|
| `PORTAL_URL` | e.g. `https://gis.example.org/portal` |
| `PORTAL_OAUTH_CLIENT_ID` / `PORTAL_OAUTH_CLIENT_SECRET` | Portal OAuth app |
| `PORTAL_ALLOWED_GROUP_ID` | group allowed to use the builder |
| `CONFIG_ROOT` | mount path of the network share |
| `RUNNER_ALLOWED_ORIGINS` | origins where experiences run (normally the Portal host) |

## Repo layout

```
/CLAUDE.md
/docs/
  TASKS.md
  CONFIG_OUTPUT_SCHEMA.md      <- profile JSON contract with the widget (only definition)
  DEPLOYMENT.md                <- server, share mount, OAuth app, widget hosting + CORS, Portal registration
/app/
  Http/Controllers/
    AuthController.php
    Builder/                   <- webmaps, layers, profiles, drafts, publish (group-gated)
    Runtime/                   <- profiles + edits for the widget (Portal-token-gated)
  Http/Middleware/
    EnsurePortalGroupMember.php
    VerifyPortalToken.php
  Runner/
    KindRegistry.php           <- kind key -> settings validator + runtime handlers
    Kinds/Crud/                <- InputTypes, EditGate, CrudSettingsValidator
    LayerHook.php, HookRejected.php, HookRegistry.php
  Hooks/                       <- PHP hooks (reviewed code)
  Services/
    PortalClient.php
    ProfileStore.php           <- drafts + published, atomic writes
/config/runner.php
/routes/web.php, /routes/api.php
/public/widgets/arcgis-runner/ <- compiled widget (deployed, gitignored)
/resources/js/
  wizard/common/               <- name & kind, webmap, CSS, code, review
  wizard/kinds/crud/           <- crud steps
  components/, api/
/tests/                        <- Pest, Portal faked with Http::fake()
```

## Core data model

A `RunnerProfile` holds the shared fields (id, name, kind, webmap, CSS, JS)
plus a kind-specific `settings` object. **`docs/CONFIG_OUTPUT_SCHEMA.md` is the
only definition.**

## Phasing

1. **Foundation** — scaffold, Portal sign-in + group check, profile store,
   wizard shell with common steps (name & kind, webmap).
2. **`crud` steps** — layers & fields, pages, input types, designer.
3. **Publish + runtime** — custom CSS step, review/publish, runtime profile
   endpoints, edit endpoint with `EditGate`, widget hosting.
4. **Custom code** — JS handlers (after the CSP spike), PHP hooks.
5. **Polish** — drift warnings, error handling, session timeout UX.
6. **Next kinds** — design each new kind here before building it.

## How work gets done

The planning chat (the "brain") covers this repo and the widget repo. It
keeps the CLAUDE.md files and backlogs current and hands each backlog task to
a separate builder session. Each builder session reads this file first, does
one task, and checks it off in `docs/TASKS.md`.

## Conventions

- TypeScript strict; no `any` unless unavoidable.
- Laravel conventions: thin controllers, logic in `app/Services` / `app/Runner`,
  constructor injection. Portal is faked with `Http::fake()` in tests; tests
  never hit a real Portal.
- Comments only for a non-obvious *why*.
- Any change to the profile shape updates `docs/CONFIG_OUTPUT_SCHEMA.md`.
  Once the widget ships, it also bumps `schemaVersion`.

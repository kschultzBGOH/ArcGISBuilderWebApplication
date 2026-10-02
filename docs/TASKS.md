# ArcGIS Builder Web Application — Task Backlog

One task = one builder session. Check off as work lands.

## Phase 0 — Decisions & scaffolding

- [x] CLAUDE.md, schema contract, backlog
- [x] Decisions: Laravel, Portal 12.0, OAuth app, single-group access, PHP hooks as reviewed code
- [x] Decisions: one registered widget + profiles; kinds (`crud` first); widget hosted by Laravel
- [ ] **User:** register OAuth app in Portal 12.0 (redirect `{APP_URL}/auth/callback`); provide group id
- [ ] Scaffold Laravel (PHP 8.4) + React/TS/Vite in `resources/js`, Calcite Components, `config/runner.php`, `.env.example`
- [ ] `runner_configs` disk + `ProfileStore` (drafts + published, atomic writes, slug ids)
- [ ] Test setup: Pest, `Http::fake()` Portal 12.0 fixtures
- [ ] `docs/DEPLOYMENT.md` (incl. widget hosting folder, web-server CORS, Portal widget registration)

## Phase 1 — Foundation

- [ ] `PortalClient`: authorize URL, code exchange, refresh, `community/self`
- [ ] `/auth/*` routes; tokens in server session only
- [ ] `EnsurePortalGroupMember` middleware + not-authorized page
- [ ] `KindRegistry` (PHP) + kind registry (SPA) with `crud` registered
- [ ] Profile list page (create, open, duplicate, delete draft)
- [ ] Wizard shell: common steps + kind steps, draft autosave/load
- [ ] Common steps: name & kind, select webmap

## Phase 2 — `crud` steps

- [ ] Layer detection: flatten operational layers + tables (incl. group layers), fetch schemas
- [ ] Layers & fields step
- [ ] Pages step (disabled by capabilities)
- [ ] `InputTypes` registry + input types step (system fields forced `readonly`)
- [ ] Designer: List layout (columns, sort, page size)
- [ ] Designer: form sections with drag reorder (Add/Edit/View)

## Phase 3 — Publish & runtime

- [ ] Custom CSS step with live preview
- [ ] Review & Publish: validate via kind validator, write published file
- [ ] `VerifyPortalToken` middleware
- [ ] `GET /api/runtime/profiles?webmapId=` and `GET /api/runtime/profiles/{id}` + CORS
- [ ] `EditGate` + `POST /api/runtime/profiles/{id}/edits/{layerId}` → `applyEdits` as the user
- [ ] Widget deploy script: copy Developer Edition 1.18 build output into `public/widgets/arcgis-runner/`

## Phase 4 — Custom code

- [ ] (Depends on widget repo CSP spike) Custom code step: JS handler editor per layer/event
- [ ] `LayerHook`, `HookRejected`, `HookRegistry` (discovers `app/Hooks/*`)
- [ ] Run hooks around `applyEdits`; hook picker in the custom code step
- [ ] Example hook + tests

## Phase 5 — Polish

- [ ] Drift warning: saved layers/fields no longer in the service
- [ ] Error handling, session timeout UX

## Phase 6 — Next kinds

- [ ] Decide and design the second kind (brain session) before any build

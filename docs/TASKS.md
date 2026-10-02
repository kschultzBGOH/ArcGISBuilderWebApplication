# ArcGIS Builder Web Application — Task Backlog

One task = one builder session. Check off as work lands.

## Phase 0 — Decisions & scaffolding

- [x] CLAUDE.md, schema contract, backlog
- [x] Decisions: Laravel, Portal 12.0, OAuth app, single-group access, 7-step wizard, PHP hooks as reviewed code
- [ ] **User:** register OAuth app in Portal 12.0 (redirect `{APP_URL}/auth/callback`); provide group id
- [ ] Scaffold Laravel (PHP 8.4) + React/TS/Vite in `resources/js`, Calcite Components, `config/runner.php`, `.env.example`
- [ ] `runner_configs` disk + `ConfigStore` (drafts + live, atomic writes)
- [ ] Test setup: Pest, `Http::fake()` Portal 12.0 fixtures
- [ ] `docs/DEPLOYMENT.md`

## Phase 1 — Foundation

- [ ] `PortalClient`: authorize URL, code exchange, refresh, `community/self`
- [ ] `/auth/*` routes; tokens in server session only
- [ ] `EnsurePortalGroupMember` middleware + not-authorized page
- [ ] Wizard shell: step navigation, draft autosave/load
- [ ] Step 1: webmap search + select

## Phase 2 — Steps 2–4

- [ ] Layer detection: flatten operational layers + tables (incl. group layers), fetch each layer's schema
- [ ] Step 2: layer + field selection UI (field types shown)
- [ ] Step 3: page checkboxes, disabled by capabilities
- [ ] `InputTypes` registry (key → valid field types, default per field type)
- [ ] Step 4: input type picker per field, filtered by field type; system fields forced to `readonly`

## Phase 3 — Steps 5–6

- [ ] Step 5: List layout (column order, sort, page size)
- [ ] Step 5: form designer — sections, drag reorder sections/fields, per page (Add/Edit/View)
- [ ] Step 6: CSS editor with live preview

## Phase 4 — Publish & runtime

- [ ] Review & Publish: validate draft against schema rules, write live file
- [ ] `VerifyPortalToken` middleware (widget users' Bearer token)
- [ ] `GET /api/runtime/configs/{webmapId}` + CORS
- [ ] `EditGate` + `POST /api/runtime/edits/{webmapId}/{layerId}` → `applyEdits` as the user

## Phase 5 — Step 7

- [ ] **Spike:** does Experience Builder's CSP allow running handler code from text (`new Function`)? Decide the fallback before building the JS editor
- [ ] Step 7: JS handler editor per layer/event
- [ ] `LayerHook` interface, `HookRejected`, `HookRegistry` discovery of `app/Hooks/*`
- [ ] Run hooks around `applyEdits` in the edit endpoint; step 7 hook picker
- [ ] Example hook + tests

## Phase 6 — Polish

- [ ] Drift warning: saved layers/fields no longer in the service
- [ ] Error handling, session timeout UX

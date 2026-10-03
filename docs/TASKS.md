# ArcGIS Builder Web Application — Task Backlog

One task = one coding session and one pull request. Details, steps, acceptance and sources for each id are in [`PLAN.md`](./PLAN.md); open items are in [`ISSUES.md`](./ISSUES.md). Check off as work lands.

## B0 — Builder Phase 0 — Decisions & scaffolding

- [x] **B0.1** CLAUDE.md, schema contract, backlog
- [x] **B0.2** Decisions: Laravel, Portal 12.0, OAuth app, single-group access, PHP hooks as reviewed code
- [x] **B0.3** Decisions: one registered widget + profiles; kinds (crud first); widget hosted by Laravel
- [ ] **B0.4** User: register the OAuth app in Portal 12.0 and provide the group id *(user)*
- [ ] **B0.5** Scaffold Laravel (PHP 8.4) + React/TS/Vite in resources/js, Calcite Components, config/runner.php, .env.example
- [ ] **B0.6** runner_configs disk + ProfileStore (drafts + published, atomic writes, slug ids)
- [ ] **B0.7** Test setup: Pest, Http::fake() Portal 12.0 fixtures
- [ ] **B0.8** docs/DEPLOYMENT.md: IIS, PHP FastCGI, URL Rewrite, app pool, share, OAuth app, widget CORS, Portal registration

## B1 — Builder Phase 1 — Foundation

- [ ] **B1.1** PortalClient: authorize URL, code exchange, refresh, community/self
- [ ] **B1.2** /auth/* routes with tokens in the server session only
- [ ] **B1.3** EnsurePortalGroupMember middleware and not-authorized page
- [ ] **B1.4** KindRegistry (PHP) and kind registry (SPA) with crud registered
- [ ] **B1.5** Profile list page: create, open, duplicate, delete draft
- [ ] **B1.6** Wizard shell: common steps + kind steps, draft autosave/load
- [ ] **B1.7** Common steps: name & kind, select webmap

## B2 — Builder Phase 2 — crud wizard steps

- [ ] **B2.1** Layer detection: flatten operational layers + tables (incl. group layers), fetch schemas
- [ ] **B2.2** Layers & fields step
- [ ] **B2.3** Pages step (disabled by capabilities)
- [ ] **B2.4** InputTypes registry + input types step (system fields forced readonly)
- [ ] **B2.5** Designer: List layout (columns, sort, page size)
- [ ] **B2.6** Designer: form sections with drag reorder (Add/Edit/View)

## B3 — Builder Phase 3 — Publish & runtime

- [ ] **B3.1** Custom CSS step with widget-only live preview
- [ ] **B3.2** Review & Publish: validate via kind validator, write published file
- [ ] **B3.3** ResolvePortalIdentity middleware (optional token → user or anonymous)
- [ ] **B3.4** WebmapAccess: check webmap visibility as the user or anonymously, short cache
- [ ] **B3.5** Runtime profile endpoints (listing by webmapId, one profile) + CORS
- [ ] **B3.6** EditGate + POST /api/runtime/profiles/{id}/edits/{layerId} → applyEdits; anonymous rate limit
- [ ] **B3.7** Widget deploy script: copy Developer Edition 1.18 build output into public/widgets/arcgis-runner/

## B4 — Builder Phases 4–6 — Custom code, polish, next kinds

- [ ] **B4.1** Custom code step: JS handler editor per layer/event (gated by widget CSP spike)
- [ ] **B4.2** `LayerHook`, `HookRejected`, `HookRegistry` (discovers `app/Hooks/*`)
- [ ] **B4.3** Run hooks around `applyEdits` in the edit endpoint
- [ ] **B4.4** Hook picker in the custom code step
- [ ] **B4.5** Example hook + tests
- [ ] **B4.6** Drift warning: saved layers/fields no longer in the service
- [ ] **B4.7** Error handling, session timeout UX
- [ ] **B4.8** Decide and design the second kind (brain session) before any build

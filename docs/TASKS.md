# ArcGIS Builder Web Application — Task Backlog

One task = one builder session. Check off as work lands.

## Phase 0 — Decisions & scaffolding

- [x] CLAUDE.md brain doc + output schema contract
- [x] Decisions: Laravel, Portal 12.0, Portal OAuth app (org creating), single-group access
- [ ] **User:** register the OAuth app in Portal 12.0 (redirect URI `{APP_URL}/auth/callback`), note the group id
- [ ] Scaffold Laravel app (PHP 8.4) + React/TS via Vite in `resources/js`, Calcite Components, `config/runner.php`, `.env.example` with all vars from CLAUDE.md
- [ ] `runner_configs` filesystem disk bound to `CONFIG_ROOT`
- [ ] Test setup (Pest/PHPUnit, `Http::fake()` Portal fixtures for 12.0 responses)
- [ ] `docs/DEPLOYMENT.md`: server requirements, share mount + permissions, OAuth app setup

## Phase 1 — Auth & Portal introspection

- [ ] `PortalClient` service: authorize URL, code exchange, refresh, `community/self`
- [ ] `/auth/login`, `/auth/callback`, `/auth/logout`; tokens in server session only
- [ ] `EnsurePortalGroupMember` middleware (`PORTAL_ALLOWED_GROUP_ID`); not-authorized page
- [ ] `GET /api/me`
- [ ] `GET /api/webmaps` (search webmaps the user can access)
- [ ] SPA: login screen + webmap selector

## Phase 2 — Layer inspection & field config UI

- [ ] `GET /api/webmaps/{id}/layers` — flatten operational layers + tables (incl. group layers)
- [ ] `GET /api/layers/schema?url=` — fields, domains, capabilities
- [ ] Frontend: layer list with enable toggle + drag reorder
- [ ] Frontend: visible fields picker (ordered)
- [ ] Frontend: editable fields picker (only service-editable, no system fields)
- [ ] Frontend: label overrides, sort, page size

## Phase 3 — Save & serve

- [ ] `PUT /api/configs/{webmapId}` — validate against schema, write to `CONFIG_ROOT` atomically (temp file + rename)
- [ ] `GET /api/configs/{webmapId}` — public read, CORS for ExB origin
- [ ] Load existing config when reopening a webmap (edit, not recreate)
- [ ] Frontend: JSON preview + save

## Phase 4 — Polish

- [ ] Config drift warning: fields/layers in saved config no longer in service
- [ ] Error handling, session timeout UX

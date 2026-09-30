# ArcGIS Runner Config Site — Task Backlog

One task = one builder session. Check off as work lands.

## Phase 0 — Decisions & scaffolding

- [x] CLAUDE.md brain doc + output schema contract
- [ ] Resolve "Open decisions" in CLAUDE.md (PHP framework, Portal version, OAuth app, save permissions)
- [ ] Scaffold `backend/` (PHP 8.4, Composer, chosen framework, `.env.example` with `PORTAL_URL`, `OAUTH_CLIENT_ID`, `CONFIG_ROOT`, `ALLOWED_ORIGINS`)
- [ ] Scaffold `frontend/` (React + TypeScript + Vite, Calcite Components)
- [ ] Local dev loop: frontend dev server proxies `/api` to PHP built-in server

## Phase 1 — Auth & Portal introspection

- [ ] Portal OAuth2 login/callback/logout endpoints; token kept in PHP session
- [ ] `GET /api/me` (current Portal user)
- [ ] `GET /api/webmaps` (search user's accessible webmaps)
- [ ] Frontend: login page + webmap selector

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

- [ ] Save permission check (Portal group/role)
- [ ] Config drift warning: fields/layers in saved config no longer in service
- [ ] Error handling, token refresh, session timeout UX

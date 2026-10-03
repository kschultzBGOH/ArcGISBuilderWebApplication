# ArcGIS Runner — Implementation Plan

Generated 2026-10-03 from the decision log, both CLAUDE.md files, both backlogs and the schema docs. Nothing here is new design: every task cites its sources, and anything the sources leave open is an issue in [`ISSUES.md`](./ISSUES.md), to be reviewed as coding starts.

This file is identical in both repos (`kschultzBGOH/ArcGISBuilderWebApplication` and `kschultzBGOH/ArcGISRunner`). The brain session keeps them in sync.

## How to read this

- Task ids are `<section>.<n>`: `B` = builder app, `W` = widget, then the backlog phase. `docs/TASKS.md` in each repo lists the same ids as checkboxes.
- **Verification** says how a task is proven: cloud tests (Pest with `Http::fake()` for the builder, Vitest on `jimu`-free `lib/` modules for the widget), a local checklist the user runs against real Portal / IIS / the share / Developer Edition 1.18, a user action, or review only.
- **Sources** cite requirement ids (`R###`, listed in [`REQUIREMENTS.md`](./REQUIREMENTS.md)), file:line references, or decision-log entries (`D` confirmed, `P` proposed and not objected, `O` pending or noted).
- **Issues** links the open items in `ISSUES.md` that affect the task.

## Milestones

| Milestone | Goal | Tasks |
|---|---|---|
| M1 Docs, decisions and repo setup | Record the finished planning docs and decisions in both repos, create main in the widget repo, and get the Portal OAuth values the builder needs. Nothing here writes app code. | B0.1, B0.2, B0.3, W0.1, W0.2, W0.3, W0.4, B0.4 |
| M2 Builder Phase 0 scaffolding and deployment doc | Lay the first builder code (Laravel + React/TS/Vite scaffold, Pest with Http::fake(), ProfileStore on the share) and write docs/DEPLOYMENT.md for the org's IIS servers. Builder app first, per the documented build order. | B0.5, B0.7, B0.6, B0.8 |
| M3 Widget Phase 0: prove deployment | Set up the cloud Vitest harness, confirm a minimal widget builds in Developer Edition 1.18, and run the deployment, auth, URL hash and CSP spikes against the real Portal 12.0. The CSP result gates the widget JS handler runner (W1.6) and the builder JS handler editor (B4.1). | W0.5, W0.6, W0.7, W0.8, W0.9, W0.10 |
| M4 Builder Phase 1: foundation | Give the builder Portal 12.0 sign-in, the single-group gate, the kind registries with crud registered, the profile list, the wizard shell with draft autosave, and the Name & kind and Select webmap steps. | B1.1, B1.2, B1.3, B1.4, B1.5, B1.6, B1.7 |
| M5 Builder Phase 2: crud wizard steps | Detect every feature layer and table in the webmap and build the four crud steps: Layers & fields, Pages, Input types, and the Designer (List layout and Add/Edit/View sections). | B2.1, B2.2, B2.3, B2.4, B2.5, B2.6 |
| M6 Builder Phase 3: publish and runtime | Finish the wizard with Custom CSS and Review & Publish, and add the runtime backend the widget calls: identity resolution, webmap access, profile endpoints with CORS, the single edit endpoint with EditGate and anonymous rate limit, and the widget deploy script. | B3.1, B3.2, B3.3, B3.4, B3.5, B3.6, B3.7 |
| M7 Widget Phase 1: shell | Replace the stale per-layer scaffold with the shell every kind shares: the { profileId, builderBaseUrl? } config, the settings dropdown, profile fetch and checks, the lazy kind registry, custom CSS injection, and the JS handler runner if the CSP spike passed. | W1.1, W1.2, W1.3, W1.4, W1.5, W1.6 |
| M8 Widget Phase 2: crud List and View | Build the read side of crud: layer matching through the Map widget's data sources, the layer picker, List and View screens, map clicks that open View, and #runner= record links. | W2.1, W2.2, W2.3, W2.4, W2.5, W2.6 |
| M9 Widget Phase 3: crud Add, Edit and Delete | Build the write side of crud: input renderers per inputType, Add/Edit forms from the section layouts, geometry with SketchViewModel, the edit client to the builder endpoint, and the Delete action. | W3.1, W3.2, W3.3, W3.4, W3.5 |
| M10 Custom code and PHP hooks | Add the builder's Custom code step (JS handler editor if the CSP spike passed, plus the PHP hook picker), the hook system run around applyEdits, crud JS events in the widget, and an example hook proven end to end. | B4.1, B4.2, B4.3, B4.4, W3.6, B4.5 |
| M11 Second kind design gate | In the brain session, decide and design the second kind in both repos before any build. It blocks no v1 task and starts no build; whether the design happens before or after v1 is open. | B4.8 |
| M12 Polish and v1 done | Finish v1: the builder drift warning and error/session-timeout UX, widget i18n and Developer Edition jest tests, and the brain's scope guard that keeps deferred and out-of-scope features out. | B4.6, B4.7, W4.1, W4.3, W4.4 |

### M1 — Docs, decisions and repo setup

Record the finished planning docs and decisions in both repos, create main in the widget repo, and get the Portal OAuth values the builder needs. Nothing here writes app code.

Exit criteria:

- CLAUDE.md, docs/CONFIG_OUTPUT_SCHEMA.md and docs/TASKS.md exist on main in the builder repo, and the builder README points to CLAUDE.md and docs/TASKS.md (B0.1).
- The builder decisions (Laravel, Portal 12.0, OAuth sign-in, one Portal group, PHP hooks as reviewed code, one registered widget, kinds with crud first, widget hosted by Laravel) are recorded in the builder CLAUDE.md (B0.2, B0.3).
- origin has a main branch at the head of claude/arcgis-runner-setup-uwxnyf when main was created (W0.3).
- GitHub shows main as the default branch of kschultzBGOH/ArcGISRunner (W0.4).
- An OAuth app for the builder exists in Portal 12.0 with redirect {APP_URL}/auth/callback (B0.4).
- The client id and secret are available for PORTAL_OAUTH_CLIENT_ID and PORTAL_OAUTH_CLIENT_SECRET, outside git (B0.4).
- The allowed group id is known for PORTAL_ALLOWED_GROUP_ID (B0.4).

User actions:

- Settle who creates main in the widget repo from claude/arcgis-runner-setup-uwxnyf (O2; actor open).
- Set main as the default branch in GitHub settings for kschultzBGOH/ArcGISRunner (W0.4).
- Confirm the builder site address that {APP_URL}/auth/callback needs (APP_URL issue).
- Register the builder OAuth app in Portal 12.0 with redirect {APP_URL}/auth/callback and provide the client id and secret outside git (B0.4).
- Pick the Portal group allowed to use the builder and provide its id (B0.4).
- Finish the line-by-line review of the widget CLAUDE.md (Conventions, Response Style) and review the builder CLAUDE.md (O4).

### M2 — Builder Phase 0 scaffolding and deployment doc

Lay the first builder code (Laravel + React/TS/Vite scaffold, Pest with Http::fake(), ProfileStore on the share) and write docs/DEPLOYMENT.md for the org's IIS servers. Builder app first, per the documented build order.

Exit criteria:

- Composer install and the Vite build succeed in a cloud session, Laravel serves a page that loads the React + TypeScript SPA, Calcite Components are available, and TypeScript runs strict (B0.5).
- .env.example lists every var in the CLAUDE.md env table with no secrets, and .env is not committed (B0.5).
- Pest runs and passes in a cloud session, tests use a temporary CONFIG_ROOT, and no test reaches a real Portal (B0.7).
- Drafts land at {CONFIG_ROOT}/drafts/{profileId}.json and published profiles at {CONFIG_ROOT}/profiles/{profileId}.json, through temp file + rename, with an immutable slug id (B0.6).
- The B0.6 local checklist passes against the real UNC share.
- docs/DEPLOYMENT.md covers the IIS site, PHP 8.4 NTS FastCGI, URL Rewrite + public/web.config, the app pool as a domain service account, UNC share permissions, the OAuth app, the widget folder web.config CORS, and Portal widget registration (B0.8).
- On the IIS server, a request that isn't a real file reaches Laravel through public/index.php, the app pool account can write to CONFIG_ROOT, and files under public/widgets/arcgis-runner/ come back with CORS headers for the Portal origin without sign-in (B0.8).

User actions:

- Run the B0.6 local checklist against the real UNC share and report on the PR before merging.
- Set up the IIS site per docs/DEPLOYMENT.md and run the B0.8 local checklist (PHP FastCGI, URL Rewrite, app pool account, share write, widget folder CORS).
- Merge each PR after brain review.

### M3 — Widget Phase 0: prove deployment

Set up the cloud Vitest harness, confirm a minimal widget builds in Developer Edition 1.18, and run the deployment, auth, URL hash and CSP spikes against the real Portal 12.0. The CSP result gates the widget JS handler runner (W1.6) and the builder JS handler editor (B4.1).

Exit criteria:

- npm test runs Vitest over lib/ tests in a cloud session, with TypeScript, Vitest and @arcgis/core 4.33 in /package.json and no jimu-* file compiled by the harness (W0.5).
- manifest.json declares exbVersion 1.18.0, and arcgis-runner builds in Developer Edition 1.18 with no errors (W0.6).
- Portal 12.0 accepts the hosted manifest.json as an Experience Builder widget item, Runner loads in an experience, and the props.context.folderUrl value is recorded (W0.7).
- Signed in, the widget reads the user's Portal token from the Experience Builder session; in a public experience opened signed out it loads, finds no token, and asks for no sign-in (W0.8).
- The URL hash spike records whether #runner= and Experience Builder's hash parameters survive each other's writes, page switches and back/forward, whether writing #runner= reloads the page, and whether the widget reads #runner= on open (W0.9).
- The CSP spike records whether a handler compiled with new Function ran or was blocked in a Portal-hosted experience, and which Portal context was tested (W0.10).

User actions:

- Run each spike's local checklist in Developer Edition 1.18 and in Portal 12.0, and report results on the PR before merging.
- As Portal admin, register the hosted manifest.json as an Experience Builder widget item (W0.7).
- Share a test experience publicly and record the sharing of each item used (W0.8).
- If the CSP spike shows handlers are blocked, choose fallback (a) built-in no-code actions or (b) self-hosted experiences (O3).

### M4 — Builder Phase 1: foundation

Give the builder Portal 12.0 sign-in, the single-group gate, the kind registries with crud registered, the profile list, the wizard shell with draft autosave, and the Name & kind and Select webmap steps.

Exit criteria:

- PortalClient builds the authorize URL, exchanges and refreshes tokens, and calls community/self against Http::fake() fixtures (B1.1).
- Sign-in redirects to Portal, {APP_URL}/auth/callback leaves the user signed in, and Portal tokens exist only in the server session (B1.2).
- Group members reach the builder, non-members see the not-authorized page, and every save and publish re-checks membership (B1.3).
- KindRegistry and the SPA kind registry both return the crud entry for the key crud (B1.4).
- The profile list creates, opens, duplicates and deletes drafts through ProfileStore (B1.5).
- The wizard shell places kind steps after Select webmap, autosaves to drafts/{profileId}.json and restores progress on reopen (B1.6).
- Name & kind saves name and kind, Select webmap lists webmaps the user can open in Portal, and the browser sends no Portal requests (B1.7).
- Pest tests pass, and each local checklist passes against the real servers.

User actions:

- Set PORTAL_URL, PORTAL_OAUTH_CLIENT_ID, PORTAL_OAUTH_CLIENT_SECRET and PORTAL_ALLOWED_GROUP_ID on the server the checklists run against.
- Run the sign-in, group-gate, profile list and wizard local checklists as a member and as a non-member, and report before merging.

### M5 — Builder Phase 2: crud wizard steps

Detect every feature layer and table in the webmap and build the four crud steps: Layers & fields, Pages, Input types, and the Designer (List layout and Add/Edit/View sections).

Exit criteria:

- Layer detection returns one flat list of feature layers (including those in group layers) and tables, with LayerConfig values, field schemas and supportsAdd/Update/Delete, and refuses non-members (B2.1).
- Layers & fields lists every detected layer and table with fields and field types, and the selection autosaves (B2.2).
- The Pages step disables and stores false for Add, Edit and Delete wherever the service lacks the capability (B2.3).
- InputTypes.php holds exactly text, textarea, number, date, datetime, dropdown and readonly, matching the schema doc; each field offers only valid keys; system fields are fixed to readonly (B2.4).
- Each layer has a List layout with ordered columns, optional sort and a page size defaulting to 25, with no sections (B2.5).
- Add, Edit and View keep independent titled section layouts with drag reorder (B2.6).
- Pest tests pass, and each local checklist passes against a real webmap.

User actions:

- Provide a real test webmap with a nested group layer, a standalone table, every geometry type, coded-value domains and a layer with editing off.
- Run each step's local checklist and report before merging.

### M6 — Builder Phase 3: publish and runtime

Finish the wizard with Custom CSS and Review & Publish, and add the runtime backend the widget calls: identity resolution, webmap access, profile endpoints with CORS, the single edit endpoint with EditGate and anonymous rate limit, and the widget deploy script.

Exit criteria:

- The Custom CSS step saves customCss to the draft and previews the widget only (B3.1).
- Review & Publish validates with the kind validator, writes nothing on failure, and writes profiles/{profileId}.json with the RunnerProfile fields on success; republish keeps the same id; non-members can't publish (B3.2).
- Runtime requests without a token reach controllers as anonymous, and a valid token resolves to that Portal user; no request is refused only for lacking a token (B3.3).
- WebmapAccess answers per identity and webmap and caches briefly (B3.4).
- The profile endpoints follow Portal sharing of the webmap, never serve drafts, and allow CORS only from RUNNER_ALLOWED_ORIGINS (B3.5).
- The edit endpoint refuses edits the profile or live service don't allow, never sends readonly or system fields, sends allowed edits to applyEdits as the caller, returns service messages, and rate-limits anonymous edits per IP (B3.6).
- After a Developer Edition build and a script run, IIS serves {APP_URL}/widgets/arcgis-runner/manifest.json with CORS headers, and git status shows no deployed widget files (B3.7).

User actions:

- Run the local checklists for B3.1, B3.2, B3.3, B3.5, B3.6 and B3.7 against the real Portal, share and IIS site.
- Decide the edit endpoint request and response contract and the bad-token rule with the brain before B3.6 and B3.3 start.
- Decide the deploy script form and location before B3.7 starts.
- Register {APP_URL}/widgets/arcgis-runner/manifest.json in Portal 12.0 if not done in W0.7.

### M7 — Widget Phase 1: shell

Replace the stale per-layer scaffold with the shell every kind shares: the { profileId, builderBaseUrl? } config, the settings dropdown, profile fetch and checks, the lazy kind registry, custom CSS injection, and the JS handler runner if the CSP spike passed.

Exit criteria:

- src/config.ts defines only profileId and optional builderBaseUrl, and the base URL comes from builderBaseUrl or folderUrl minus /widgets/arcgis-runner/ (W1.1).
- The settings panel lists only profiles built for the connected map's webmap and stores the chosen profileId, with no URL input (W1.2).
- The shell fetches the profile with Bearer when signed in and none when anonymous, and shows clear errors for unknown version, unknown kind, unreachable server and webmap mismatch (W1.3).
- registry.ts lazy-loads only the kind in use (W1.4).
- <head> holds one <style data-runner-profile="{id}"> per profile, kept on unmount, and the widget root has class arcgis-runner and data-profile (W1.5).
- If W0.10 passed, the runner compiles a function body and calls it with the kind's ctx (W1.6).
- lib/ tests pass in the cloud, and the widget builds in Developer Edition 1.18.

User actions:

- Run each local checklist in Developer Edition 1.18 against the builder app and report before merging.
- Run the deferred Portal-hosted checks for W1.2 and W1.3 once W0.7 and B3.7 have landed.

### M8 — Widget Phase 2: crud List and View

Build the read side of crud: layer matching through the Map widget's data sources, the layer picker, List and View screens, map clicks that open View, and #runner= record links.

Exit criteria:

- Profile layers match by layerId, falling back to url, using the Map widget's existing data sources (W2.1).
- The picker lists layers in settings.layers order, and screens change inside the widget (W2.2).
- The List shows the profile's columns, labels, sort and page size, reads straight from the feature service, and selecting a row highlights and zooms on the map, for every geometry type and tables (W2.3).
- View shows layouts.view sections read-only (W2.4).
- Clicking a profile-layer feature opens View, several hits show a pick list, and the click and leave decisions are covered by Vitest (W2.5).
- Opening View writes #runner={profileId}:{layerId}:view:{featureKey} without disturbing Experience Builder's hash, links reopen and zoom to the record, only the matching Runner responds, and links grant no access (W2.6).

User actions:

- Provide a published crud profile whose webmap has every geometry type, a table, and layers with and without GlobalID.
- Run each local checklist in Developer Edition 1.18, plus the Portal-hosted signed-out checks, and report before merging.

### M9 — Widget Phase 3: crud Add, Edit and Delete

Build the write side of crud: input renderers per inputType, Add/Edit forms from the section layouts, geometry with SketchViewModel, the edit client to the builder endpoint, and the Delete action.

Exit criteria:

- One renderer per inputType key matches InputTypes.php, and an unknown key falls back to readonly with a warning (W3.1).
- Add and Edit render their layouts, are offered only when the page is on and the live layer allows it, block map-click navigation, ask before leaving with unsaved changes, and Edit has Copy link (W3.2).
- Add and Edit draw or reshape geometry for every geometry type, and tables skip geometry (W3.3).
- Every save POSTs to {base}/api/runtime/profiles/{profileId}/edits/{layerId}, the widget never calls applyEdits(), and server, hook and service messages appear in the widget (W3.4).
- Delete appears on List and View only when pages.delete and live delete support allow it, asks for confirmation, and removes the feature (W3.5).

User actions:

- Run each local checklist in Developer Edition 1.18 with the builder edit endpoint running, signed in and anonymous, and report before merging.

### M10 — Custom code and PHP hooks

Add the builder's Custom code step (JS handler editor if the CSP spike passed, plus the PHP hook picker), the hook system run around applyEdits, crud JS events in the widget, and an example hook proven end to end.

Exit criteria:

- The Custom code step shows a handler editor per layer and crud event, saves customJs per layer, and the validator rejects unknown event keys; built only after W0.10 passes (B4.1).
- LayerHook, HookRejected and HookRegistry exist, and classes in app/Hooks/ are discovered with no other registration (B4.2).
- For a layer with phpHook set, the edit endpoint runs EditGate, before*, applyEdits, after*; HookRejected stops the edit and returns its message; no PHP text is executed (B4.3).
- The hook picker lists every HookRegistry key plus none and saves phpHook per layer (B4.4).
- The five crud events call the layer's handlers, and cancel in beforeSave and beforeDelete stops the request (W3.6).
- On the real servers, an edit through Runner on a layer with the example hook shows its effect or rejection message (B4.5).

User actions:

- If the CSP spike was blocked, the fallback choice (O3) replaces B4.1 and W3.6 before work starts.
- Agree the LayerHook signatures, key format and example hook behaviour with the brain.
- Run the local checklists for B4.1, B4.3, B4.4, W3.6 (locally and Portal-hosted) and B4.5, and report before merging.

### M11 — Second kind design gate

In the brain session, decide and design the second kind in both repos before any build. It blocks no v1 task and starts no build; whether the design happens before or after v1 is open.

Exit criteria:

- The second kind is named and recorded.
- docs/CONFIG_OUTPUT_SCHEMA.md holds its settings shape and JavaScript event set.
- Both CLAUDE.md files describe it, both backlogs list its build tasks, and the Project scope sections stay identical.
- No build task for the kind starts before the design is in the docs.

User actions:

- Name the second kind with the brain and approve its design.
- Decide how brain doc edits land (direct push or pull request).

### M12 — Polish and v1 done

Finish v1: the builder drift warning and error/session-timeout UX, widget i18n and Developer Edition jest tests, and the brain's scope guard that keeps deferred and out-of-scope features out.

Exit criteria:

- A builder-group member publishes a crud profile for a real webmap without writing code.
- An app author adds Runner to an experience in Portal 12.0 and picks that profile.
- End users can list, add, edit, view, and delete exactly as the profile allows, for every geometry type and for tables.
- Every edit runs the layer's PHP hook and respects the service's own permissions.
- Republishing the profile changes the experience with no widget rebuild and no Portal step.

User actions:

- Agree the drift warning behaviour and the error/session-timeout scope with the brain before B4.6 and B4.7 start.
- Run the B4.6, B4.7, W4.1 and W4.3 local checklists (including the jest run in Developer Edition 1.18) and report before merging.
- Run the end-to-end Done-when check in Portal 12.0: publish a real crud profile, add Runner to an experience, exercise every page per geometry type and table, confirm hooks run, then republish and confirm the change shows.

## Critical path

B0.5 → B0.7 → B1.1 → B1.2 → B1.3 → B1.5 → B1.6 → B1.7 → B2.2 → B2.5 → B2.6 → B3.2 → B3.5 → W1.2 → W1.3 → W1.4 → W2.1 → W2.2 → W2.3 → W2.5 → W3.2 → W3.3 → W3.4 → W3.5 → W4.1

## Cross-repo dependencies

| From | To | Reason |
|---|---|---|
| B3.7 | W0.6 | The deploy script copies client/dist/widgets/arcgis-runner/, which exists only once a minimal widget builds in Developer Edition 1.18. |
| B3.7 | W0.7 | The deploy script follows what the deployment spike proved about hosting, CORS and Portal registration. |
| B4.1 | W0.10 | The CSP spike gates the builder's JS handler editor; if handlers are blocked, the user picks a fallback first (O3). |
| B4.5 | W3.4 | The only documented way to send an edit end to end is Runner's edit client. |
| B4.5 | W0.7 | The end-to-end hook check needs the widget registered in Portal 12.0, which the deployment spike sets up. |
| W1.2 | B3.2 | The settings dropdown lists published profiles, which only exist after Review & Publish. |
| W1.2 | B3.5 | The dropdown loads from GET /api/runtime/profiles?webmapId=. |
| W1.3 | B3.5 | The shell fetches GET /api/runtime/profiles/{profileId}. |
| W1.5 | B3.1 | customCss is authored in the builder's Custom CSS step. |
| W1.5 | B3.2 | customCss reaches the widget only in a published profile. |
| W2.1 | B3.2 | Layer matching needs a published crud profile to test against. |
| W2.1 | B3.5 | Layer matching reads the profile served by the runtime profile endpoint. |
| W2.3 | B3.7 | The List's Portal-hosted sharing check needs the widget deployed from the builder app. |
| W2.4 | B3.7 | The View's Portal-hosted sharing check needs the widget deployed from the builder app. |
| W3.1 | B2.4 | Widget renderers must use the same inputType keys as the builder's InputTypes registry. |
| W3.2 | B2.6 | Add/Edit forms render the section layouts built in the builder's Designer. |
| W3.4 | B3.6 | Writes go only to POST /api/runtime/profiles/{id}/edits/{layerId}, so the builder enforces the profile and runs PHP hooks. |
| W3.6 | B3.7 | crud JS events must be checked in a Portal-hosted experience, where the CSP applies, which needs the deployed widget. |
| W3.6 | B4.1 | Handler text reaches profiles only through the builder's Custom code step. |

## B0 — Builder Phase 0 — Decisions & scaffolding

B0 records three finished items: the docs (CLAUDE.md, schema contract and backlog, with the README pointing to them) and two sets of locked decisions. It also holds the user's Portal step, which is to register the builder's OAuth app and provide the allowed group id. After that it lays the first builder code: the Laravel + React/TypeScript/Vite scaffold with Calcite Components, the Pest test setup with Portal faked by Http::fake(), and the runner_configs disk with ProfileStore, whose tests need Pest. It ends with docs/DEPLOYMENT.md, which covers hosting the builder app and the widget build on the org's IIS servers, the OAuth app, and registering the widget in Portal. Every code or doc task follows the per-task flow in CLAUDE.md. The brain picks the next unstarted task (or asks the user to pick) and spawns one cloud session with a self-contained prompt. The session reads CLAUDE.md, branches from main, never pushes to main, checks the box in docs/TASKS.md and opens a pull request. The brain reviews it. The user runs any local checklist against the real servers, then merges.

### B0.1 CLAUDE.md, schema contract, backlog (done)

**Repo:** builder · **Verification:** review only

Write the builder CLAUDE.md, the profile JSON contract, and the backlog.

Files: `CLAUDE.md`, `docs/CONFIG_OUTPUT_SCHEMA.md`, `docs/TASKS.md`, `README.md`

Issues: ISS-01, ISS-05

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:7, /home/user/arcgisbuilderwebapplication/README.md:6-8, R030, R045, R046, R077, R251, R252

### B0.2 Decisions: Laravel, Portal 12.0, OAuth app, single-group access, PHP hooks as reviewed code (done)

**Repo:** builder · **Verification:** review only

Lock in Laravel, Portal 12.0, Portal OAuth sign-in, access limited to one Portal group, and PHP hooks as reviewed code in git.

Files: `CLAUDE.md`

Issues: ISS-01, ISS-05

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:8, R075, R210, R211, R212, R242, decision-log:D7, decision-log:D9

### B0.3 Decisions: one registered widget + profiles; kinds (crud first); widget hosted by Laravel (done)

**Repo:** builder · **Verification:** review only

Lock in one registered Runner widget that runs profiles, kinds with crud first, and the widget build hosted by the Laravel app.

Files: `CLAUDE.md`

Issues: ISS-01, ISS-05

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:9, R072, R073, R074, decision-log:D14, decision-log:D15, decision-log:D16

### B0.4 User: register the OAuth app in Portal 12.0 and provide the group id

**Repo:** user · **Verification:** user action

The user registers the builder's OAuth app in Portal 12.0 with redirect {APP_URL}/auth/callback and provides the id of the one Portal group allowed to use the builder.

Steps:

1. Confirm the builder site's address, which the redirect URI {APP_URL}/auth/callback needs (see issue on APP_URL).
2. In Portal 12.0, register an OAuth app for the builder with redirect URI {APP_URL}/auth/callback.
3. Provide the app's client id and secret, the values behind PORTAL_OAUTH_CLIENT_ID and PORTAL_OAUTH_CLIENT_SECRET. Keep them out of git. Where they are handed over and kept is open (see issue).
4. Pick the Portal group whose members may use the builder and provide its id, the value behind PORTAL_ALLOWED_GROUP_ID.
5. Check the box in docs/TASKS.md once the values exist. Who checks it for a user-only task is open (see issue).

Acceptance:

- An OAuth app for the builder exists in Portal 12.0 with redirect {APP_URL}/auth/callback.
- The client id and secret are available for PORTAL_OAUTH_CLIENT_ID and PORTAL_OAUTH_CLIENT_SECRET, outside git.
- The allowed group id is known for PORTAL_ALLOWED_GROUP_ID.

Issues: ISS-16, ISS-17, ISS-18, ISS-19, ISS-20

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:10, /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-129, /home/user/arcgisbuilderwebapplication/CLAUDE.md:197-198, R212, R226, R246, R247, R440, R441, R442, R048, decision-log:D7, decision-log:O1

### B0.5 Scaffold Laravel (PHP 8.4) + React/TS/Vite in resources/js, Calcite Components, config/runner.php, .env.example

**Repo:** builder · **Verification:** cloud tests

Create the Laravel app on PHP 8.4 that serves a React + TypeScript SPA from resources/js, built with Vite (laravel-vite-plugin) and using Calcite Components, plus config/runner.php and .env.example.

Files: `resources/js/`, `config/runner.php`, `.env.example`, `routes/web.php`

Steps:

1. Brain: pick this task as the next unstarted one in docs/TASKS.md (or ask the user to pick), then spawn one cloud session with a self-contained prompt covering the task, the relevant parts of CLAUDE.md, and the files to touch.
2. Read CLAUDE.md first. Branch from main. Never push to main.
3. Create a Laravel app on the current Laravel major that supports PHP 8.4 (version to confirm; see issue).
4. Add a React + TypeScript SPA under resources/js, built with Vite through laravel-vite-plugin. Turn on TypeScript strict mode. Use no `any` unless a typing gap forces it, with a comment saying why.
5. Add Calcite Components to the SPA (package and version to confirm; see issue).
6. Have Laravel serve the SPA page. The SPA uses same-origin session cookies and makes no Portal calls. Portal calls go through PortalClient in Phase 1.
7. Create config/runner.php. Its contents and keys are open (see issue).
8. Write .env.example with every var in the CLAUDE.md env table: PORTAL_URL, PORTAL_OAUTH_CLIENT_ID, PORTAL_OAUTH_CLIENT_SECRET, PORTAL_ALLOWED_GROUP_ID, CONFIG_ROOT, RUNNER_ALLOWED_ORIGINS, RUNNER_ANON_EDITS_PER_MINUTE. Use the documented examples for PORTAL_URL (https://gis.example.org/portal) and CONFIG_ROOT (\\fileserver\gis\runner). Put no real secrets in it. The sources use {APP_URL} only as a placeholder. Treating it as Laravel's APP_URL variable is an assumption to verify (see issue on APP_URL).
9. Keep the existing .gitignore entries (node_modules/, dist/, vendor/, .env, *.log, .DS_Store) so .env stays out of git.
10. Write comments only for a non-obvious why, per the CLAUDE.md conventions and Response Style section.
11. Check the box in docs/TASKS.md and open a pull request into main. The brain reviews it and the user merges. If the work shows the plan was wrong, the brain updates CLAUDE.md.

Acceptance:

- Composer install and the Vite build succeed in a cloud session. This is a build check: no test suite exists until Pest is set up (see issue on Pest ordering).
- Laravel serves a page that loads the React + TypeScript SPA from resources/js.
- Calcite Components are available to the SPA.
- TypeScript runs in strict mode.
- config/runner.php exists.
- .env.example lists every var in the CLAUDE.md env table, with no secrets.
- .env is not committed.

Issues: ISS-04, ISS-07, ISS-17, ISS-21, ISS-22, ISS-23, ISS-24, ISS-25, ISS-26, ISS-27, ISS-28, ISS-29, ISS-30, ISS-142

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11, /home/user/arcgisbuilderwebapplication/CLAUDE.md:115, /home/user/arcgisbuilderwebapplication/CLAUDE.md:121-123, /home/user/arcgisbuilderwebapplication/CLAUDE.md:192-201, /home/user/arcgisbuilderwebapplication/CLAUDE.md:228-229, /home/user/arcgisbuilderwebapplication/CLAUDE.md:260-265, /home/user/arcgisbuilderwebapplication/CLAUDE.md:274-278, R217, R221, R222, R223, R224, R269, R300, R301, R245, R246, R247, R248, R249, R250, R021, R022, R025, R026, R048, R010, R011, R012, R013, R014, R015, R446, decision-log:P1, decision-log:D24

### B0.6 runner_configs disk + ProfileStore (drafts + published, atomic writes, slug ids)

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B0.5, B0.7

Add the Laravel disk runner_configs rooted at CONFIG_ROOT and app/Services/ProfileStore.php, which stores drafts and published profiles as JSON with atomic writes and immutable slug ids.

Files: `app/Services/ProfileStore.php`, `tests/`

Steps:

1. Brain: pick this task as the next unstarted one in docs/TASKS.md (or ask the user to pick), then spawn one cloud session with a self-contained prompt covering the task, the relevant parts of CLAUDE.md, and the files to touch.
2. Read CLAUDE.md first. Branch from main. Never push to main.
3. Use the Pest setup from B0.7 for this task's tests (see issue on Pest ordering).
4. Register a Laravel disk named runner_configs whose root is CONFIG_ROOT. The sources don't name the config file that defines the disk.
5. Store published profiles at profiles/{profileId}.json and drafts at drafts/{profileId}.json on that disk. Drafts are builder-only. Only published files under profiles/ are what the runtime serves.
6. Write every file atomically: write a temp file, then rename it over the target. A republish replaces an existing profiles/{profileId}.json this way.
7. Generate profileId as a slug when a profile is created (example: "hydrant-inspections"). Never change it afterwards.
8. Keep the logic in app/Services/ProfileStore.php with constructor injection. Write comments only for a non-obvious why.
9. Write Pest tests that use a temporary local folder as CONFIG_ROOT.
10. Because the task touches the share, end the pull request with a local test checklist.
11. Check the box in docs/TASKS.md and open a pull request into main. The brain reviews it. The user runs the local checklist against the real servers, then merges. If the work shows the plan was wrong, the brain updates CLAUDE.md.

Acceptance:

- A draft saved through ProfileStore lands at {CONFIG_ROOT}/drafts/{profileId}.json.
- A published profile lands at {CONFIG_ROOT}/profiles/{profileId}.json.
- Drafts are never written under profiles/.
- Writes go through a temp file and a rename.
- Publishing an already-published profile again replaces profiles/{profileId}.json under the same id, through temp file + rename.
- A profile's id is a slug generated at creation and stays the same on later saves and on publish.
- Pest tests pass in the cloud with a temporary local CONFIG_ROOT.
- The local checklist passes against the real UNC share.

Local checklist (user runs):

- Set CONFIG_ROOT to the real UNC path of the share, not a mapped drive letter. Which machine and account run this checklist, and whether it waits for the IIS site and app pool documented in B0.8, are open (see issue).
- Save a draft and publish a profile through ProfileStore. How ProfileStore is exercised by hand in Phase 0 is open (see issue).
- Confirm drafts/{profileId}.json and profiles/{profileId}.json appear on the share. Their exact contents follow the open draft-shape issue.
- Publish the same profile again and confirm profiles/{profileId}.json is replaced under the same name.
- Confirm no temp files are left on the share after the writes.
- Save the same draft again and confirm its profileId and file name did not change.

Issues: ISS-04, ISS-08, ISS-25, ISS-30, ISS-31, ISS-32, ISS-33, ISS-34, ISS-35

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:12, /home/user/arcgisbuilderwebapplication/CLAUDE.md:44-46, /home/user/arcgisbuilderwebapplication/CLAUDE.md:143-147, /home/user/arcgisbuilderwebapplication/CLAUDE.md:227, /home/user/arcgisbuilderwebapplication/CLAUDE.md:235, /home/user/arcgisbuilderwebapplication/CLAUDE.md:260-270, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:8-9, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:16, R066, R114, R115, R116, R234, R235, R236, R248, R268, R274, R275, R020, R022, R026, R010, R011, R012, R013, R014, R015, R446, R449, decision-log:P3, decision-log:D24

### B0.7 Test setup: Pest, Http::fake() Portal 12.0 fixtures

**Repo:** builder · **Verification:** cloud tests · **Depends on:** B0.5

Set up Pest for the builder, with Portal 12.0 responses faked by Http::fake() fixtures and a temporary local folder for CONFIG_ROOT, so no test hits a real Portal.

Files: `tests/`

Steps:

1. Brain: pick this task as the next unstarted one in docs/TASKS.md (or ask the user to pick), then spawn one cloud session with a self-contained prompt covering the task, the relevant parts of CLAUDE.md, and the files to touch.
2. Read CLAUDE.md first. Branch from main. Never push to main.
3. Install and configure Pest under tests/, unless the scaffold already did (see issue on Pest ordering).
4. Add Http::fake() fixtures for Portal 12.0. The sources name these Portal and feature service calls: OAuth code exchange and refresh, community/self, webmap search, the webmap item for WebmapAccess, layer schemas, and applyEdits. Which of them belong in this task is open (see issues on the fixture list and on feature service calls).
5. Point CONFIG_ROOT at a temporary local folder during tests.
6. Make sure no test can reach a real Portal (mechanism to verify; see issue).
7. Write comments only for a non-obvious why.
8. Run the suite in the cloud session.
9. Check the box in docs/TASKS.md and open a pull request into main. The brain reviews it and the user merges. If the work shows the plan was wrong, the brain updates CLAUDE.md.

Acceptance:

- Pest runs and passes in a cloud session.
- Portal 12.0 calls made in tests are answered by Http::fake() fixtures. What this can show before PortalClient exists is open (see issue).
- Tests use a temporary local folder as CONFIG_ROOT.
- No test reaches a real Portal.

Issues: ISS-07, ISS-21, ISS-22, ISS-30, ISS-36, ISS-37, ISS-38, ISS-39

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:13, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:28, /home/user/arcgisbuilderwebapplication/CLAUDE.md:50, /home/user/arcgisbuilderwebapplication/CLAUDE.md:135-138, /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, /home/user/arcgisbuilderwebapplication/CLAUDE.md:235, /home/user/arcgisbuilderwebapplication/CLAUDE.md:260-268, /home/user/arcgisbuilderwebapplication/CLAUDE.md:276-278, R016, R018, R022, R274, R275, R276, R010, R011, R012, R013, R014, R015, R446, decision-log:P18, decision-log:D24

### B0.8 docs/DEPLOYMENT.md: IIS, PHP FastCGI, URL Rewrite, app pool, share, OAuth app, widget CORS, Portal registration

**Repo:** builder · **Verification:** local checklist · **Depends on:** B0.4, B0.5

Write docs/DEPLOYMENT.md so the builder app runs on the org's IIS servers with access to the share, and the widget build is served with CORS and registered in Portal once.

Files: `docs/DEPLOYMENT.md`

Steps:

1. Brain: pick this task as the next unstarted one in docs/TASKS.md (or ask the user to pick), then spawn one cloud session with a self-contained prompt covering the task, the relevant parts of CLAUDE.md, and the files to touch.
2. Read CLAUDE.md first. Branch from main. Never push to main.
3. Write docs/DEPLOYMENT.md per the CLAUDE.md Response Style section.
4. IIS site: describe the builder app's IIS site. Its web root is public/, since public/widgets/arcgis-runner/ is served at {APP_URL}/widgets/arcgis-runner/.
5. PHP: describe PHP 8.4 (8.4.25), non-thread-safe x64 build, running as FastCGI.
6. URL Rewrite: describe the module and the committed public/web.config, which sends every request that isn't a real file to public/index.php.
7. App pool: describe running the application pool as a domain service account with write access to the network share.
8. Share: describe the UNC share permissions for that account and set CONFIG_ROOT to the UNC path (example \\fileserver\gis\runner), because mapped drive letters aren't visible to the app pool.
9. OAuth app: describe registering the OAuth app in Portal 12.0 with redirect {APP_URL}/auth/callback. Its values back PORTAL_OAUTH_CLIENT_ID, PORTAL_OAUTH_CLIENT_SECRET and PORTAL_ALLOWED_GROUP_ID.
10. Widget hosting: describe deploying the Developer Edition 1.18 build output (client/dist/widgets/arcgis-runner/, path to verify; see issue) to public/widgets/arcgis-runner/, which is not committed. IIS serves it as static files, and a web.config in that folder adds CORS headers for the Portal origin.
11. Portal registration: describe a Portal admin registering {APP_URL}/widgets/arcgis-runner/manifest.json once (Add Item → Experience Builder widget).
12. Because the task touches IIS and the share, end the pull request with a local test checklist.
13. Check the box in docs/TASKS.md and open a pull request into main. The brain reviews it. The user runs the local checklist against the real servers, then merges. If the work shows the plan was wrong, the brain updates CLAUDE.md.

Acceptance:

- docs/DEPLOYMENT.md covers the IIS site, PHP 8.4 NTS FastCGI, URL Rewrite + public/web.config, the app pool as a domain service account, UNC share permissions, the OAuth app, the widget folder web.config CORS, and Portal widget registration.
- On the org's IIS server, a request that isn't a real file reaches Laravel through public/index.php. This waits on how the builder app itself gets onto IIS (see issue).
- The app pool's domain service account can write to CONFIG_ROOT on the share through its UNC path. How the write is triggered in Phase 0 is open (see issue).
- Static files under public/widgets/arcgis-runner/ come back with CORS headers for the Portal origin and load without signing in.
- Once a widget build exists in public/widgets/arcgis-runner/, a Portal admin can register {APP_URL}/widgets/arcgis-runner/manifest.json as an Experience Builder widget. This depends on the widget minimal build and deployment spike or the Phase 3 deploy script (see issue on widget hosting ordering).

Local checklist (user runs):

- On the IIS server, follow docs/DEPLOYMENT.md to create the site with the B0.5 scaffold deployed. How the app is put on the server is open (see issue on deploying the builder app).
- Confirm the site runs PHP 8.4 NTS x64 as FastCGI.
- Request a URL that isn't a real file and confirm Laravel answers it through public/index.php.
- Confirm the app pool runs as the domain service account.
- Confirm the app, running as that account, can write a file under the CONFIG_ROOT UNC path. How the write is triggered is open (see issue).
- Put a file in public/widgets/arcgis-runner/ next to the folder web.config, request it from the Portal origin, and confirm the response carries the CORS headers.
- Request a file under {APP_URL}/widgets/arcgis-runner/ with no sign-in and confirm it is served.
- Only once a widget build is in public/widgets/arcgis-runner/: sign in to Portal 12.0 as an admin, add {APP_URL}/widgets/arcgis-runner/manifest.json as an Experience Builder widget item, and confirm the item is created.
- Confirm the OAuth app section matches the app registered in B0.4.

Issues: ISS-03, ISS-07, ISS-16, ISS-17, ISS-18, ISS-19, ISS-26, ISS-27, ISS-29, ISS-33, ISS-34, ISS-52, ISS-55, ISS-56, ISS-60, ISS-61, ISS-62, ISS-63, ISS-64, ISS-65, ISS-66, ISS-67, ISS-68, ISS-69, ISS-141

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14, /home/user/arcgisbuilderwebapplication/CLAUDE.md:91, /home/user/arcgisbuilderwebapplication/CLAUDE.md:115-120, /home/user/arcgisbuilderwebapplication/CLAUDE.md:130-131, /home/user/arcgisbuilderwebapplication/CLAUDE.md:148-153, /home/user/arcgisbuilderwebapplication/CLAUDE.md:199, /home/user/arcgisbuilderwebapplication/CLAUDE.md:210, /home/user/arcgisbuilderwebapplication/CLAUDE.md:260-270, /home/user/arcgisbuilderwebapplication/CLAUDE.md:284, CLAUDE.md:75-79, R025, R067, R089, R090, R091, R111, R112, R113, R199, R200, R209, R218, R219, R220, R248, R253, R254, R255, R256, R257, R258, R325, R439, R020, R010, R011, R012, R013, R014, R015, R446, R449, decision-log:D12, decision-log:D18, decision-log:D22, decision-log:D24, decision-log:P20

## B1 — Builder Phase 1 — Foundation

B1 gives the builder app Portal 12.0 sign-in, the single-group gate, and the kind registries on both the PHP and SPA sides, with crud registered. A group member signs in, sees the profile list, creates, opens, duplicates or deletes a draft, and fills the Name & kind and Select webmap steps. The wizard autosaves progress to a server-side draft on the share. All Portal calls go through PortalClient, and Portal tokens stay in the server session. Each task follows the documented workflow: branch from main, check its box in docs/TASKS.md, open a pull request into main, never push to main; the brain reviews and the user merges. Every B1 pull request that touches sign-in or the share ends with a local test checklist against the real servers, as builder CLAUDE.md requires. The B0 dependencies (scaffold B0.5, ProfileStore B0.6, test setup B0.7, DEPLOYMENT.md B0.8, OAuth app B0.4) assume the TASKS.md phase placement, which conflicts with builder CLAUDE.md (see issues).

### B1.1 PortalClient: authorize URL, code exchange, refresh, community/self

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B0.5, B0.7, B0.4

Build app/Services/PortalClient.php as the only path to Portal. It covers the OAuth2 authorization-code calls the builder sign-in needs and the community/self call used for the group check.

Files: `app/Services/PortalClient.php`, `tests/`

Steps:

1. Branch from main.
2. Read the Portal base URL and OAuth app credentials from PORTAL_URL, PORTAL_OAUTH_CLIENT_ID and PORTAL_OAUTH_CLIENT_SECRET.
3. Add a method that builds the Portal OAuth2 authorize URL for the authorization-code flow, with redirect {APP_URL}/auth/callback.
4. Add a method that exchanges an authorization code for tokens.
5. Add a method that refreshes tokens.
6. Add a method that calls /sharing/rest/community/self with the user's token and returns the result used for the group membership check.
7. Keep PortalClient a service in app/Services, injected by constructor where it is used.
8. Write Pest tests for each method against the Http::fake() Portal 12.0 fixtures. No test calls a real Portal.
9. End the pull request with the local test checklist below, because this task touches sign-in.
10. Check the PortalClient box in docs/TASKS.md Phase 1 and open a pull request into main. Never push to main directly. The brain reviews the pull request and the user merges it after running the checklist.

Acceptance:

- PortalClient returns an authorize URL for the authorization-code flow whose redirect is {APP_URL}/auth/callback.
- PortalClient exchanges a code for tokens against the faked Portal token response.
- PortalClient refreshes tokens against the faked Portal response.
- PortalClient calls /sharing/rest/community/self and returns the user data needed for the group check.
- Portal base URL and OAuth app credentials come from PORTAL_URL, PORTAL_OAUTH_CLIENT_ID and PORTAL_OAUTH_CLIENT_SECRET.
- Pest tests pass with Http::fake(); nothing hits a real Portal.
- The pull request ends with a local test checklist.

Local checklist (user runs):

- Confirm the Portal 12.0 OAuth app exists with redirect {APP_URL}/auth/callback (task B0.4).
- Set PORTAL_URL, PORTAL_OAUTH_CLIENT_ID and PORTAL_OAUTH_CLIENT_SECRET in the .env of the server the checklist runs against.
- Compare the authorize URL, token endpoint and community/self call that PortalClient and its Http::fake() fixtures use with the Portal 12.0 instance and its OAuth app. Note any difference on the pull request.
- The full sign-in round trip has no route until B1.2; run it in the B1.2 checklist (see open issue).
- Report the results on the pull request before merging.

Issues: ISS-01, ISS-04, ISS-07, ISS-16, ISS-19, ISS-36, ISS-37, ISS-66, ISS-78, ISS-79, ISS-80, ISS-93

Sources: R302, R303, R304, R305, R225, R226, R228, R245, R246, R267, R274, R275, R276, R026, R208, R020, R449, R441, R012, R013, R014, R027, R028, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18, /home/user/arcgisbuilderwebapplication/CLAUDE.md:268-270

### B1.2 /auth/* routes with tokens in the server session only

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B1.1, B0.4, B0.8

Add AuthController and the /auth/* routes for the Portal OAuth2 authorization-code flow. Portal tokens stay in the server session, and the SPA uses same-origin session cookies and never calls Portal.

Files: `app/Http/Controllers/AuthController.php`, `routes/web.php or routes/api.php (decided in task)`, `tests/`

Steps:

1. Branch from main.
2. Add app/Http/Controllers/AuthController.php as a thin controller over PortalClient.
3. Register the /auth/* routes, including the route that sends the user to PortalClient's authorize URL and {APP_URL}/auth/callback (route file decided in the task).
4. In the callback, exchange the code through PortalClient and store the Portal tokens in the server session.
5. After the callback, return the signed-in user to the builder SPA.
6. Keep Portal tokens out of every response to the browser; the SPA relies on the same-origin Laravel session cookie.
7. Use PortalClient's refresh call to renew the session's tokens.
8. Write Pest tests with Http::fake() for the redirect and the callback, asserting tokens land in the session and appear in no response body.
9. End the pull request with the local test checklist below.
10. Check the /auth/* box in docs/TASKS.md Phase 1 and open a pull request into main. Never push to main directly. The brain reviews the pull request and the user merges it after running the checklist.

Acceptance:

- Starting sign-in redirects the browser to Portal's authorize URL.
- {APP_URL}/auth/callback exchanges the code and leaves the user signed in to the builder.
- Portal tokens exist only in the server session; no response to the browser contains them.
- The SPA talks only to Laravel over same-origin session cookies and never calls Portal directly.
- Pest tests pass with Http::fake().

Local checklist (user runs):

- Confirm the Portal 12.0 OAuth app exists with redirect {APP_URL}/auth/callback (task B0.4).
- Set PORTAL_URL, PORTAL_OAUTH_CLIENT_ID and PORTAL_OAUTH_CLIENT_SECRET in the .env of the server the checklist runs against (how a PR branch reaches that server is an open issue).
- Open {APP_URL} in a browser and start sign-in. Expect a redirect to the Portal 12.0 sign-in page.
- Sign in with a Portal account. Expect a return through {APP_URL}/auth/callback to the builder, signed in.
- In browser dev tools, check cookies, local storage and network responses. Expect the Laravel session cookie and no Portal access or refresh token.
- In the network tab, confirm the SPA sends no requests to the Portal host.
- Report the results on the pull request before merging.

Issues: ISS-07, ISS-16, ISS-19, ISS-27, ISS-66, ISS-78, ISS-80, ISS-81, ISS-82

Sources: R306, R226, R227, R223, R224, R259, R270, R213, R020, R449, R441, R253, R012, R013, R014, R027, R028, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:19, /home/user/arcgisbuilderwebapplication/CLAUDE.md:121-127

### B1.3 EnsurePortalGroupMember middleware and not-authorized page

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B1.1, B1.2, B0.4, B0.8

Limit the builder to members of PORTAL_ALLOWED_GROUP_ID, checked via community/self at login and on every save/publish. Signed-in non-members see a not-authorized page.

Files: `app/Http/Middleware/EnsurePortalGroupMember.php`, `app/Http/Controllers/Builder/`, `tests/`, `TBD in task`

Steps:

1. Branch from main.
2. Add app/Http/Middleware/EnsurePortalGroupMember.php.
3. At login, check membership in PORTAL_ALLOWED_GROUP_ID through PortalClient's community/self call.
4. On every save and publish request, re-check membership through community/self.
5. Attach the middleware to the group-gated builder controllers in app/Http/Controllers/Builder/ (webmaps, layers, profiles, drafts, publish).
6. Refuse requests without a signed-in builder session at the group-gated builder controllers (what the visitor sees is an open issue).
7. Keep the middleware and any sign-in requirement off the runtime routes, because runtime access follows Portal sharing.
8. Add the not-authorized page for signed-in users outside the group (file location decided in the task).
9. Write Pest tests with Http::fake() for a member, a non-member, a request with no builder session, and a save that re-checks membership.
10. End the pull request with the local test checklist below.
11. Check the EnsurePortalGroupMember box in docs/TASKS.md Phase 1 and open a pull request into main. Never push to main directly. The brain reviews the pull request and the user merges it after running the checklist.

Acceptance:

- A signed-in member of PORTAL_ALLOWED_GROUP_ID reaches the builder.
- A signed-in non-member sees the not-authorized page and cannot use the builder controllers.
- A request without a signed-in builder session cannot use the group-gated builder controllers.
- Every save and publish request re-checks membership via community/self.
- Pest tests pass with Http::fake().

Local checklist (user runs):

- Set PORTAL_ALLOWED_GROUP_ID to the group id provided in task B0.4.
- Sign in as a member of that group. Expect the builder to open.
- Sign out, then sign in as a Portal account outside that group. Expect the not-authorized page.
- Report the results on the pull request before merging.

Issues: ISS-07, ISS-16, ISS-20, ISS-66, ISS-79, ISS-82, ISS-83, ISS-84, ISS-132

Sources: R262, R307, R212, R213, R228, R247, R260, R261, R091, R442, R020, R449, R012, R013, R014, R027, R028, R446, /home/user/arcgisbuilderwebapplication/CLAUDE.md:18, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:20, /home/user/arcgisbuilderwebapplication/CLAUDE.md:128-131

### B1.4 KindRegistry (PHP) and kind registry (SPA) with crud registered

**Repo:** builder · **Verification:** cloud tests · **Depends on:** B0.5, B0.7

Make kind a plug-in on the builder side. app/Runner/KindRegistry.php maps a kind key to its settings validator and runtime handlers, the SPA registry maps a kind key to its wizard steps, and both register crud.

Files: `app/Runner/KindRegistry.php`, `resources/js/wizard/kinds/crud/`, `tests/`, `TBD in task`

Steps:

1. Branch from main.
2. Add app/Runner/KindRegistry.php mapping a kind key to its settings validator and runtime handlers.
3. Register the crud kind key in KindRegistry.
4. Add the SPA kind registry mapping a kind key to its wizard steps, which live in resources/js/wizard/kinds/{kind}/ (SPA registry file location decided in the task).
5. Register crud in the SPA registry, pointing at resources/js/wizard/kinds/crud/.
6. Write the SPA code as strict TypeScript with no `any` unless unavoidable, with a comment saying why.
7. Write Pest tests for looking up crud in KindRegistry.
8. Check the KindRegistry box in docs/TASKS.md Phase 1 and open a pull request into main. Never push to main directly. The brain reviews the pull request and the user merges it.

Acceptance:

- KindRegistry returns the crud entry for the key crud (Pest).
- The SPA kind registry returns the crud entry for the key crud (checked in brain review; no SPA test tool is named, see open issue).
- Both registries use the same kind key, crud (checked in brain review).
- SPA code is strict TypeScript with no unexplained `any`.
- Pest tests pass.

Issues: ISS-04, ISS-28, ISS-85, ISS-86

Sources: R263, R308, R117, R072, R277, R272, R070, R148, R023, R021, R274, R012, R013, R014, R027, R028, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:21, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:13, /home/user/arcgisbuilderwebapplication/CLAUDE.md:28-40, /home/user/arcgisbuilderwebapplication/CLAUDE.md:274

### B1.5 Profile list page: create, open, duplicate, delete draft

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B0.6, B1.3, B0.8

Give group members a page that lists profiles and lets them create, open, duplicate and delete drafts. All storage goes through ProfileStore on the runner_configs disk.

Files: `app/Http/Controllers/Builder/`, `app/Services/ProfileStore.php`, `resources/js/components/`, `resources/js/api/`, `tests/`

Steps:

1. Branch from main.
2. Add thin, group-gated profile and draft endpoints in app/Http/Controllers/Builder/ that call ProfileStore.
3. List profiles from ProfileStore.
4. Create a new draft with a generated slug id that never changes after creation, stored at drafts/{profileId}.json.
5. Make Open route to the wizard entry point for that profileId; the wizard itself loads the profile in B1.6.
6. Duplicate a profile into a new draft under a new slug id.
7. Delete a draft by removing drafts/{profileId}.json.
8. Route every write through ProfileStore's atomic writes (temp file + rename).
9. Build the list page in the SPA with Calcite Components, calling the endpoints through resources/js/api/.
10. Write the SPA code as strict TypeScript with no `any` unless unavoidable, with a comment saying why.
11. Write Pest tests against a temporary local CONFIG_ROOT for list, create, duplicate and delete.
12. End the pull request with the local test checklist below.
13. Check the profile list box in docs/TASKS.md Phase 1 and open a pull request into main. Never push to main directly. The brain reviews the pull request and the user merges it after running the checklist.

Acceptance:

- The profile list page shows the profiles in ProfileStore.
- Create writes a new draft at {CONFIG_ROOT}/drafts/{profileId}.json with an immutable slug id.
- Open routes to the wizard entry point for that profileId.
- Duplicate writes a second draft under a new slug id.
- Delete draft removes the draft file.
- Only group members can use these endpoints, and saves re-check membership.
- SPA code is strict TypeScript with no unexplained `any`.
- Pest tests pass with a temporary CONFIG_ROOT.

Local checklist (user runs):

- Set CONFIG_ROOT to the UNC path of the network share, and confirm the IIS app pool's domain service account can write to it.
- Sign in as a group member and open the profile list page.
- Create a profile. Expect a new file under drafts/ on the share.
- Duplicate that profile. Expect a second draft file under a different id.
- Delete a draft. Expect its file to disappear from drafts/ on the share.
- Report the results on the pull request before merging.

Issues: ISS-04, ISS-08, ISS-16, ISS-31, ISS-32, ISS-66, ISS-82, ISS-87, ISS-88

Sources: R309, R116, R234, R235, R236, R268, R260, R228, R066, R069, R221, R222, R021, R026, R020, R449, R248, R012, R013, R014, R027, R028, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:22, /home/user/arcgisbuilderwebapplication/CLAUDE.md:143-147

### B1.6 Wizard shell: common steps + kind steps, draft autosave/load

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B1.4, B1.5, B0.6, B0.8

Build the wizard shell that runs the common steps and inserts the kind's steps from the SPA kind registry. Progress autosaves to a server-side draft and loads back when the profile is opened.

Files: `resources/js/wizard/common/`, `resources/js/wizard/kinds/crud/`, `resources/js/api/`, `app/Http/Controllers/Builder/`, `tests/`

Steps:

1. Branch from main.
2. Build the wizard shell in resources/js/wizard/ with Calcite Components.
3. Hold the documented target order Name & kind, Select webmap, the kind-specific steps, Custom CSS, Custom code, Review & Publish. Run the common steps that exist; whether steps from later phases show yet is an open issue.
4. Insert the kind-specific steps from the SPA kind registry for the draft's kind, after Select webmap.
5. On open, load the draft through a group-gated drafts endpoint in app/Http/Controllers/Builder/ that reads drafts/{profileId}.json via ProfileStore.
6. Autosave wizard progress to the server-side draft through the same drafts endpoint, using ProfileStore's atomic writes.
7. Apply the save-time group re-check to autosave only as settled by the open issue on whether autosave counts as a save.
8. Write the SPA code as strict TypeScript with no `any` unless unavoidable, with a comment saying why.
9. Write Pest tests for draft load and save against a temporary CONFIG_ROOT.
10. End the pull request with the local test checklist below.
11. Check the wizard shell box in docs/TASKS.md Phase 1 and open a pull request into main. Never push to main directly. The brain reviews the pull request and the user merges it after running the checklist.

Acceptance:

- The shell places kind steps after Select webmap, taken from the SPA kind registry for the draft's kind.
- Opening a profile from the list page loads its draft into the wizard.
- Progress autosaves to {CONFIG_ROOT}/drafts/{profileId}.json through ProfileStore.
- Reopening a profile restores the saved progress from the draft.
- SPA code is strict TypeScript with no unexplained `any`.
- Pest tests pass with a temporary CONFIG_ROOT.

Local checklist (user runs):

- Sign in as a group member and open a draft from the profile list page. Expect the wizard to load it.
- Report the results on the pull request before merging. The autosave and reload checks run in B1.7, once steps exist to change.

Issues: ISS-04, ISS-08, ISS-16, ISS-28, ISS-31, ISS-35, ISS-82, ISS-83, ISS-86, ISS-88, ISS-89, ISS-90, ISS-91, ISS-92

Sources: R310, R278, R281, R118, R214, R271, R272, R235, R236, R228, R221, R222, R021, R051, R020, R449, R012, R013, R014, R027, R028, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:23, /home/user/arcgisbuilderwebapplication/CLAUDE.md:42-58, /home/user/arcgisbuilderwebapplication/CLAUDE.md:121-122

### B1.7 Common steps: name & kind, select webmap

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B1.6, B1.4, B1.1, B1.3, B0.8

Build the two common steps every kind shares. Name & kind records the profile name and kind, and Select webmap searches the webmaps the signed-in user can access in Portal, through Laravel.

Files: `resources/js/wizard/common/`, `app/Http/Controllers/Builder/`, `app/Services/PortalClient.php`, `resources/js/api/`, `tests/`

Steps:

1. Branch from main.
2. Build the Name & kind step in resources/js/wizard/common/ with Calcite Components, offering the kinds in the SPA kind registry (crud in v1).
3. Save the profile name and kind into the draft (draft field naming depends on the open draft-shape issue).
4. Add a webmap search call to PortalClient that runs as the signed-in user, using the token in the server session.
5. Add a group-gated webmaps endpoint in app/Http/Controllers/Builder/ that calls that PortalClient method.
6. Build the Select webmap step in resources/js/wizard/common/, searching through the webmaps endpoint so the SPA never calls Portal.
7. Save the chosen webmap into the draft.
8. Write the SPA code as strict TypeScript with no `any` unless unavoidable, with a comment saying why.
9. Write Pest tests with Http::fake() for the webmaps endpoint, including a non-member being refused.
10. End the pull request with the local test checklist below.
11. Check the common steps box in docs/TASKS.md Phase 1 and open a pull request into main. Never push to main directly. The brain reviews the pull request and the user merges it after running the checklist.

Acceptance:

- The Name & kind step saves the profile name and a kind chosen from the kind registry into the draft.
- The Select webmap step lists webmaps the signed-in user can access in Portal.
- The browser sends no requests to Portal; the search goes through Laravel and PortalClient.
- The chosen webmap is saved in the draft.
- The wizard shows Name & kind, then Select webmap.
- SPA code is strict TypeScript with no unexplained `any`.
- Pest tests pass with Http::fake().

Local checklist (user runs):

- Sign in as a group member and open a draft.
- Check that the wizard shows Name & kind, then Select webmap.
- On Name & kind, enter a name and pick crud. Expect both saved in the draft file on the share.
- On Select webmap, search for a webmap you can open in Portal. Expect it in the results.
- Pick it. Expect the webmap saved in the draft file.
- Reload the browser and reopen the profile. Expect the name, kind and webmap to be restored.
- In the network tab, confirm no browser request goes to the Portal host.
- Report the results on the pull request before merging.

Issues: ISS-16, ISS-31, ISS-32, ISS-82, ISS-88, ISS-92, ISS-93, ISS-94

Sources: R279, R280, R271, R225, R224, R260, R147, R148, R149, R208, R072, R221, R021, R278, R020, R448, R449, R012, R013, R014, R027, R028, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:24, /home/user/arcgisbuilderwebapplication/CLAUDE.md:48-50

## B2 — Builder Phase 2 — crud wizard steps

B2 builds the four `crud` wizard steps on top of the Phase 1 wizard shell. A group-gated builder endpoint uses PortalClient to find every feature layer and standalone table in the selected webmap. It flattens group layers and fetches each schema. The SPA steps then let the author pick layers, turn pages on within the service's capabilities, choose an input type per field, and lay out the List columns and the Add/Edit/View sections. Every step autosaves to the draft, building toward the `CrudSettings` shape in CONFIG_OUTPUT_SCHEMA.md.

### B2.1 Layer detection: flatten operational layers + tables (incl. group layers), fetch schemas

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B0.7, B1.1, B1.2, B1.3

Give the crud steps a complete list of the selected webmap's feature layers and standalone tables, with group layers flattened and each layer's schema fetched from its service. The SPA gets this list from a group-gated builder endpoint and never calls Portal itself.

Files: `app/Http/Controllers/Builder/`, `app/Services/PortalClient.php`, `routes/ (web.php or api.php; see issue)`, `tests/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and docs/CONFIG_OUTPUT_SCHEMA.md, then branch from main.
2. Add a layers endpoint for a given webmapId under app/Http/Controllers/Builder/, behind EnsurePortalGroupMember. Keep the controller thin and put the logic in app/Services or app/Runner with constructor injection.
3. Fetch the webmap item's data through PortalClient.
4. Walk the operational layers, descending into group layers at every depth, and collect each feature layer. Collect each standalone table.
5. Fetch each collected layer's and table's service schema through PortalClient.
6. Return one entry per layer or table with the LayerConfig values the schema doc defines: layerId (the operational layer or table id in the webmap), url, kind ('layer' or 'table'), title, geometryType ('point' | 'multipoint' | 'polyline' | 'polygon', null for tables), objectIdField, globalIdField when present, and fields with name, type (esriFieldType*), nullable, editable, length and domain (CodedValueDomain or RangeDomain shape).
7. Include each entry's edit capabilities as supportsAdd, supportsUpdate and supportsDelete, for the Pages step.
8. Add Pest tests with Http::fake() Portal 12.0 fixtures covering nested group layers, a standalone table, each geometry type, coded-value and range domains, a layer without add/update/delete support, and a request from a non-member.
9. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist.

Acceptance:

- Feature layers inside group layers, at any depth, appear in one flat list.
- Standalone tables appear with kind 'table' and geometryType null.
- Each entry carries the LayerConfig and FieldConfig values listed in the steps, with field types as esriFieldType* strings and domains in the schema doc's CodedValueDomain / RangeDomain shapes.
- Each entry reports supportsAdd, supportsUpdate and supportsDelete.
- A signed-in user outside PORTAL_ALLOWED_GROUP_ID is refused.
- All Portal requests go through PortalClient; the SPA makes none.
- Pest tests pass, and no test hits a real Portal.

Local checklist (user runs):

- This checklist can't run until the user has registered the OAuth app and provided the allowed group id (B0.4 / O1).
- Deploy the PR branch where it can reach the real Portal 12.0, and sign in to the builder as a member of the allowed group.
- Pick a real webmap that has a group layer (nested, if one exists) and a standalone table.
- Call the layers endpoint for that webmap as a signed-in group member. The method and path depend on the open question about the endpoint path.
- Confirm every feature layer, including those inside group layers, and every table appears in the list.
- For two layers, compare fields, field types, domains, objectId and globalId fields against each service's REST page.
- If a layer with editing turned off exists, confirm its supportsAdd, supportsUpdate and supportsDelete match the service.
- Sign in as a user outside the group and confirm the endpoint refuses.

Issues: ISS-01, ISS-06, ISS-16, ISS-36, ISS-37, ISS-66, ISS-82, ISS-95, ISS-96, ISS-97, ISS-98, ISS-99, ISS-100, ISS-101, ISS-104

Sources: R290, R291, R289, R225, R224, R260, R156, R157, R158, R159, R160, R161, R162, R163, R168, R169, R171, R172, R173, R174, R175, R183, R184, R212, R026, R274, R275, R276, R306, R441, R442, R012, R027, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:10, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:19, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:28, /home/user/arcgisbuilderwebapplication/CLAUDE.md:61-63, /home/user/arcgisbuilderwebapplication/CLAUDE.md:214, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:44-81, decision-log:O1

### B2.2 Layers & fields step

**Repo:** builder · **Verification:** local checklist · **Depends on:** B1.4, B1.6, B1.7, B2.1

Crud step 1. Show every detected feature layer and table in the draft's webmap with its fields and field types, and let the author select which to include. The selection autosaves to the draft.

Files: `resources/js/wizard/kinds/crud/`, `resources/js/api/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and docs/CONFIG_OUTPUT_SCHEMA.md, then branch from main.
2. Add the Layers & fields step to the crud kind in resources/js/wizard/kinds/crud/ as the first kind step, after Select webmap.
3. Load the detected layers for the draft's webmap from the B2.1 endpoint through resources/js/api/.
4. Show each detected layer and table with its fields and field types, using Calcite Components.
5. Let the author select which layers and tables to include. Field-level selection is an open question.
6. Store each included layer in the draft as a LayerConfig with the detected layerId, url, kind, title, geometryType, objectIdField, globalIdField and fields. Later steps fill inputType, pages and layouts.
7. Save through the wizard shell's draft autosave.
8. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist.

Acceptance:

- The step appears right after Select webmap as the first crud step.
- Every feature layer and table in the webmap, including those in group layers, is listed with its fields and field types.
- The author's selection autosaves to the draft and is restored when the draft reopens.
- Later crud steps work only on the included layers.
- The SPA sends no requests to Portal.
- TypeScript strict; no `any` unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Deploy the PR branch where it can reach the real Portal 12.0 and the network share, and sign in as a member of the allowed group.
- Create or open a crud draft and select a real webmap with a group layer and a standalone table.
- Confirm the Layers & fields step comes right after Select webmap and lists every feature layer (including grouped ones) and the table, each with its fields and field types.
- Select some layers, reload the browser, reopen the draft, and confirm the selection is still there.
- In the browser's network tab, confirm the SPA sends no requests to the Portal host.

Issues: ISS-06, ISS-16, ISS-28, ISS-31, ISS-92, ISS-98, ISS-101, ISS-102, ISS-103, ISS-107

Sources: R289, R032, R281, R278, R154, R163, R272, R273, R221, R224, R021, R051, R070, R087, R012, R027, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:29, /home/user/arcgisbuilderwebapplication/CLAUDE.md:61-63, /home/user/arcgisbuilderwebapplication/CLAUDE.md:44, /home/user/arcgisbuilderwebapplication/CLAUDE.md:274, decision-log:D8

### B2.3 Pages step (disabled by capabilities)

**Repo:** builder · **Verification:** local checklist · **Depends on:** B2.1, B2.2

Crud step 2. Each included layer gets checkboxes for List, Add, Edit, View and Delete. A box is disabled when the service doesn't support it.

Files: `resources/js/wizard/kinds/crud/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and docs/CONFIG_OUTPUT_SCHEMA.md, then branch from main.
2. Add the Pages step as crud step 2 in resources/js/wizard/kinds/crud/.
3. For each included layer, show List, Add, Edit, View and Delete checkboxes using Calcite Components.
4. Disable Add when supportsAdd is false, Edit when supportsUpdate is false, and Delete when supportsDelete is false, using the capabilities from layer detection.
5. Store the choices as the layer's pages (Record<PageKey, boolean>) in the draft. Never store pages.add, pages.edit or pages.delete as true when the matching capability is false.
6. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist.

Acceptance:

- Each included layer and table shows List, Add, Edit, View and Delete boxes.
- Add, Edit and Delete are disabled and stored as false wherever the service lacks the matching capability.
- The choices autosave to the draft and reload with it.
- TypeScript strict; no `any` unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Deploy the PR branch where it can reach the real Portal 12.0 and the network share, and sign in as a member of the allowed group.
- Open a draft whose webmap has an editable layer and a layer or table with some editing turned off.
- Confirm each included layer shows the five boxes, and the disabled boxes match the service's capabilities on its REST page.
- Toggle some boxes, reload, reopen the draft, and confirm the choices persisted.

Issues: ISS-10, ISS-15, ISS-31, ISS-96, ISS-97, ISS-104, ISS-105

Sources: R292, R293, R164, R155, R196, R168, R086, R076, R021, R012, R027, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:30, /home/user/arcgisbuilderwebapplication/CLAUDE.md:64-65, /home/user/arcgisbuilderwebapplication/CLAUDE.md:274, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:54, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:133, decision-log:D8, decision-log:D10

### B2.4 InputTypes registry + input types step (system fields forced readonly)

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B2.2

Define the crud input type keys, and the field types each key is valid for, in app/Runner/Kinds/Crud/InputTypes.php. Add crud step 3, where the author picks an input type per field from the valid keys. System fields are forced to readonly.

Files: `app/Runner/Kinds/Crud/InputTypes.php`, `resources/js/wizard/kinds/crud/`, `tests/`, `docs/CONFIG_OUTPUT_SCHEMA.md`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and docs/CONFIG_OUTPUT_SCHEMA.md, then branch from main.
2. Create app/Runner/Kinds/Crud/InputTypes.php with exactly the keys text, textarea, number, date, datetime, dropdown and readonly, matching the InputType union in docs/CONFIG_OUTPUT_SCHEMA.md.
3. Encode which field types each key is valid for: text and textarea for string; number for integer, small integer, double and single; date for date and date-only; datetime for date; dropdown for any field with a coded-value domain; readonly for any field.
4. Encode the system field rule: objectId, globalId, editor-tracking and Shape__Area/Length fields only get readonly.
5. Add Pest tests for each key's valid field types, dropdown on coded-value domains, readonly on every field, and the system field rule.
6. Add the Input types step as crud step 3 in resources/js/wizard/kinds/crud/. For each included layer, list each field with an input type choice filtered to the keys valid for that field.
7. Show system fields fixed to readonly with no other choice.
8. Store the choice as the field's inputType in the draft.
9. If the key list or inputOptions changes from what docs/CONFIG_OUTPUT_SCHEMA.md states, update docs/CONFIG_OUTPUT_SCHEMA.md in the same PR. Adding an input type key is a schema change.
10. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist.

Acceptance:

- InputTypes.php holds the seven keys and no others, spelled as in the schema doc's InputType union.
- The key list in InputTypes.php equals the InputType union in docs/CONFIG_OUTPUT_SCHEMA.md.
- Each field offers only the keys the input types table allows for its type. A field with a coded-value domain also offers dropdown.
- System fields show readonly and can't be changed.
- Choices autosave to the draft and reload with it.
- Pest tests pass, and no test hits a real Portal.
- TypeScript strict; no `any` unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Deploy the PR branch where it can reach the real Portal 12.0 and the network share, and sign in as a member of the allowed group.
- Open a draft with a layer that has string, numeric and date fields and a field with a coded-value domain.
- Confirm each field's options match the input types table in CLAUDE.md.
- Confirm the layer's objectId, globalId, editor-tracking and Shape__Area/Shape__Length fields are fixed to readonly.
- Change some choices, reload, reopen the draft, and confirm they persisted.

Issues: ISS-10, ISS-14, ISS-31, ISS-85, ISS-96, ISS-102, ISS-106, ISS-107, ISS-108, ISS-109, ISS-110, ISS-111

Sources: R294, R295, R296, R126, R127, R128, R129, R130, R131, R132, R133, R134, R135, R136, R176, R177, R194, R195, R264, R078, R079, R021, R012, R027, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:31, /home/user/arcgisbuilderwebapplication/CLAUDE.md:66, /home/user/arcgisbuilderwebapplication/CLAUDE.md:162-174, /home/user/arcgisbuilderwebapplication/CLAUDE.md:274, /home/user/arcgisbuilderwebapplication/CLAUDE.md:279, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:79-83, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:130-132, decision-log:P5, decision-log:P16, decision-log:D8

### B2.5 Designer: List layout (columns, sort, page size)

**Repo:** builder · **Verification:** local checklist · **Depends on:** B2.2

Crud step 4, List part. For each included layer, the author sets the List columns and their order, an optional sort field and order, and the page size (default 25). The List layout has columns only, no sections.

Files: `resources/js/wizard/kinds/crud/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and docs/CONFIG_OUTPUT_SCHEMA.md, then branch from main.
2. Add the Designer step as crud step 4 in resources/js/wizard/kinds/crud/, with a List layout per included layer.
3. Let the author choose columns from the layer's fields and set their order.
4. Let the author set an optional sortField from the layer's fields and a sortOrder of asc or desc.
5. Add a page size setting that starts at 25.
6. Store the result as layouts.list (ListLayout: columns, sortField, sortOrder, pageSize) in the draft. Columns and sortField must be names in the layer's fields.
7. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist.

Acceptance:

- Each included layer has a List layout with ordered columns, an optional sort field with asc or desc, and a page size that defaults to 25.
- Only names from the layer's fields can be columns or the sort field.
- The List layout has no sections.
- The layout autosaves to the draft and reloads with the column order intact.
- TypeScript strict; no `any` unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Deploy the PR branch where it can reach the real Portal 12.0 and the network share, and sign in as a member of the allowed group.
- Open the Designer for a draft with at least one included layer.
- Confirm the page size starts at 25.
- Pick and reorder columns, set a sort field and order, and change the page size.
- Reload, reopen the draft, and confirm the List layout persisted in the same order.

Issues: ISS-10, ISS-28, ISS-31, ISS-101, ISS-102, ISS-112, ISS-113, ISS-114, ISS-115

Sources: R297, R178, R179, R180, R181, R197, R193, R165, R299, R021, R012, R027, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:32, /home/user/arcgisbuilderwebapplication/CLAUDE.md:67, /home/user/arcgisbuilderwebapplication/CLAUDE.md:274, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:85-90, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:129, decision-log:P7, decision-log:D11

### B2.6 Designer: form sections with drag reorder (Add/Edit/View)

**Repo:** builder · **Verification:** local checklist · **Depends on:** B2.2, B2.5

Crud step 4, form part. Add, Edit and View each get their own layout of titled sections, and the author drags to reorder sections and fields.

Files: `resources/js/wizard/kinds/crud/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and docs/CONFIG_OUTPUT_SCHEMA.md, then branch from main.
2. Extend the Designer step with Add, Edit and View layouts per included layer, each stored separately.
3. Let the author create titled sections and place fields from the layer's fields into them.
4. Let the author drag to reorder sections, and drag to reorder fields.
5. Store each as layouts.add, layouts.edit and layouts.view (FormLayout: sections, each with title and fields) in the draft. Every field name must be in the layer's fields.
6. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist.

Acceptance:

- Add, Edit and View each keep an independent layout.
- Each section has a title.
- Dragging reorders sections and fields, and the new order autosaves to the draft and reloads.
- Only names from the layer's fields appear in sections.
- TypeScript strict; no `any` unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Deploy the PR branch where it can reach the real Portal 12.0 and the network share, and sign in as a member of the allowed group.
- Open the Designer for an included layer. On Add, create two titled sections and place fields in them.
- Drag to reorder the sections and the fields, and confirm the order changes.
- Open Edit and View and confirm each has its own layout, unaffected by the Add changes.
- Reload, reopen the draft, and confirm all three layouts persisted.

Issues: ISS-31, ISS-102, ISS-112, ISS-113, ISS-114

Sources: R298, R299, R182, R165, R193, R021, R012, R027, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:33, /home/user/arcgisbuilderwebapplication/CLAUDE.md:67-68, /home/user/arcgisbuilderwebapplication/CLAUDE.md:274, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:92-94, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:129, decision-log:D8, decision-log:D11

## B3 — Builder Phase 3 — Publish & runtime

This phase finishes the wizard with the Custom CSS and Review & Publish steps, so a builder-group member can publish a crud profile to the network share. It adds the runtime backend the widget calls. That backend resolves identity from an optional Portal token, checks webmap access the way Portal sharing allows, serves profile endpoints, and runs the single edit endpoint with EditGate and an anonymous rate limit. It also adds the script that copies the Developer Edition 1.18 widget build into the builder app's public folder, after the widget deployment spike (W0.7) has proved hosting. Each task is delivered on its own branch from main with a pull request into main; the brain reviews and the user merges. Open questions named in a task are resolved by the brain or the user on that issue before the task starts, not inside the task session.

### B3.1 Custom CSS step with widget-only live preview

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B1.6, B2.2

Add the common Custom CSS wizard step. The author writes one stylesheet per profile (`customCss`) and sees a live preview of the widget only. The step tells the author that the rules apply to the whole experience.

Files: `resources/js/wizard/common/`

Steps:

1. Add the Custom CSS step to resources/js/wizard/common/ and place it after the kind-specific steps in the common step order.
2. Give the step one stylesheet editor. Store its text as the profile's `customCss` string in the draft, so draft autosave keeps it.
3. Show a note in the step that the rules apply to the whole experience.
4. Render a live preview that applies the current `customCss` to the widget only, not the full experience. What the preview renders is open; the brain resolves it before the task starts (see issues 'Custom CSS preview: what it renders and how it is isolated' and 'Builder SPA rendering the jimu-based widget').
5. Build the step's controls with Calcite Components.
6. Deliver on a branch from main: check the matching box in docs/TASKS.md and open a pull request into main. Never push to main. The brain reviews the pull request and the user merges it.

Acceptance:

- The wizard shows a Custom CSS step after the kind-specific steps.
- Text typed in the step is saved as `customCss` in the draft and is still there after the draft is reopened.
- The step shows a note that the rules apply to the whole experience.
- The live preview changes as the CSS changes and shows the widget only, not a full experience.
- Pest with a temporary CONFIG_ROOT shows that a draft saved with `customCss` reads back unchanged.

Local checklist (user runs):

- Sign in to the builder with a Portal account in the allowed group.
- Open or create a crud draft and go to the Custom CSS step. Confirm it comes after the crud steps.
- Confirm the note says the rules apply to the whole experience.
- Type a rule that starts with `.arcgis-runner`. Confirm the preview changes as you type and shows only the widget.
- Leave the wizard, reopen the draft, and confirm the CSS is still there.
- Open `drafts/{profileId}.json` under CONFIG_ROOT on the share and confirm it holds the `customCss` text.

Issues: ISS-01, ISS-10, ISS-16, ISS-28, ISS-116, ISS-117

Sources: R033, R118, R120, R121, R122, R214, R222, R278, R281, R282, R283, R284, R020, R275, R012, R013, R014, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:29, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:37, /home/user/arcgisbuilderwebapplication/CLAUDE.md:44, /home/user/arcgisbuilderwebapplication/CLAUDE.md:49-55, /home/user/arcgisbuilderwebapplication/CLAUDE.md:232, /home/user/arcgisbuilderwebapplication/CLAUDE.md:261-265, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:23, decision-log:D19, decision-log:P7

### B3.2 Review & Publish: validate via kind validator, write published file

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B0.6, B1.3, B1.4, B1.6, B2.2, B2.3, B2.4, B2.5, B2.6, B3.1

Add the Review & Publish step. It shows the profile JSON, validates it with the kind's validator, and on publish writes the published file that makes the profile available to the widget. Republishing updates every experience that uses the profile.

Files: `resources/js/wizard/common/`, `app/Http/Controllers/Builder/`, `app/Runner/KindRegistry.php`, `app/Runner/Kinds/Crud/ (CrudSettingsValidator)`, `app/Services/ProfileStore.php`, `/routes/web.php`, `/routes/api.php`

Steps:

1. Add the Review & Publish step to resources/js/wizard/common/. It shows the profile JSON built from the draft. Build its controls with Calcite Components.
2. Add a publish action in app/Http/Controllers/Builder/. Gate it like the other builder actions, so group membership in PORTAL_ALLOWED_GROUP_ID is checked again via community/self on publish.
3. Keep the controller thin. Put validation in app/Runner and storage in app/Services/ProfileStore.php.
4. Look up the profile's kind in KindRegistry and run that kind's settings validator (CrudSettingsValidator for crud). Which rules it checks is open (see issues).
5. If validation fails, write nothing to profiles/. How the failure is shown to the author is open (see issues).
6. If validation passes, fill the shared RunnerProfile fields from CONFIG_OUTPUT_SCHEMA.md: schemaVersion 1, id (the immutable slug), name, kind, webmapId, portalUrl, publishedAt (ISO 8601), publishedBy (the signed-in Portal username), customCss, settings. Where portalUrl and the capabilities snapshot come from is open (see issues).
7. Store each layer's `phpHook` only as a HookRegistry key or null. Never accept or store PHP text in the profile.
8. Write `{CONFIG_ROOT}/profiles/{profileId}.json` through ProfileStore with an atomic write (temp file + rename).
9. On republish, overwrite the same file under the same profileId.
10. Deliver on a branch from main: check the matching box in docs/TASKS.md and open a pull request into main. Never push to main. The brain reviews the pull request and the user merges it.

Acceptance:

- The Review & Publish step shows the profile JSON.
- Publishing a profile that fails the kind validator writes nothing to profiles/.
- Publishing a valid profile writes `profiles/{profileId}.json` with the RunnerProfile fields defined in CONFIG_OUTPUT_SCHEMA.md.
- In the published file, publishedBy is the signed-in Portal username and publishedAt is an ISO 8601 timestamp.
- The published profile holds no PHP text; each `phpHook` is a HookRegistry key or null.
- A signed-in user who is no longer in PORTAL_ALLOWED_GROUP_ID can't publish.
- Republishing replaces the file and keeps the same profileId.
- Pest with Http::fake() for community/self and a temporary CONFIG_ROOT covers each case above.

Local checklist (user runs):

- Sign in as a member of the allowed group and complete a crud draft for a real webmap.
- Open Review & Publish and confirm the profile JSON is shown.
- If the wizard lets you build a draft that breaks a validator rule, publish it. Confirm no file appears under `profiles/` on the share.
- Publish a valid profile. Confirm `{CONFIG_ROOT}/profiles/{profileId}.json` appears on the UNC share.
- Change the draft and republish. Confirm the same file updates and the profileId is unchanged.
- While signed in, remove the test user from the allowed group in Portal and try to publish. Confirm it's refused.

Issues: ISS-08, ISS-16, ISS-28, ISS-31, ISS-33, ISS-82, ISS-85, ISS-94, ISS-104, ISS-118, ISS-119, ISS-120, ISS-205

Sources: R033, R068, R106, R110, R114, R115, R116, R145, R146, R147, R148, R149, R150, R151, R152, R153, R167, R168, R193, R194, R195, R196, R216, R222, R228, R235, R236, R244, R260, R263, R264, R268, R271, R286, R287, R288, R026, R020, R274, R275, R012, R013, R014, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:20, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:28-33, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38, /home/user/arcgisbuilderwebapplication/CLAUDE.md:44-46, /home/user/arcgisbuilderwebapplication/CLAUDE.md:58, /home/user/arcgisbuilderwebapplication/CLAUDE.md:102, /home/user/arcgisbuilderwebapplication/CLAUDE.md:128-129, /home/user/arcgisbuilderwebapplication/CLAUDE.md:143-147, /home/user/arcgisbuilderwebapplication/CLAUDE.md:190, /home/user/arcgisbuilderwebapplication/CLAUDE.md:214, /home/user/arcgisbuilderwebapplication/CLAUDE.md:219-221, /home/user/arcgisbuilderwebapplication/CLAUDE.md:227, /home/user/arcgisbuilderwebapplication/CLAUDE.md:261-265, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:8, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:11-25, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:62, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:127-135, decision-log:D9, decision-log:P2, decision-log:P3

### B3.3 ResolvePortalIdentity middleware (optional token → user or anonymous)

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B1.1

Add middleware that turns the optional `Authorization: Bearer` Portal token on runtime requests into a Portal user, or into "anonymous" when no token is sent. It never adds a sign-in requirement for widget users. The pull request ends with a local checklist because it handles Portal tokens, which this plan counts as touching sign-in.

Files: `app/Http/Middleware/ResolvePortalIdentity.php`, `app/Services/PortalClient.php`

Steps:

1. Create ResolvePortalIdentity in app/Http/Middleware/.
2. When the request has no Authorization header, set the identity to anonymous and let the request continue.
3. When the request has `Authorization: Bearer <token>`, resolve the token to a Portal user through PortalClient. How the token is checked and what happens with a bad token follow the brain's resolution of the issue 'How ResolvePortalIdentity checks a token and handles a bad one', made before the task starts.
4. Make the identity (the user and their token, or anonymous) available to runtime controllers and services, so WebmapAccess and the edit endpoint can call Portal and the feature service as that identity.
5. Never reject a request only because it has no token.
6. End the pull request with a local test checklist the user runs against the real Portal before merging.
7. Deliver on a branch from main: check the matching box in docs/TASKS.md and open a pull request into main. Never push to main. The brain reviews the pull request and the user merges it.

Acceptance:

- Against Http::fake() fixtures, a runtime request without a token reaches the controller as anonymous.
- Against Http::fake() fixtures, a runtime request with a valid Portal token reaches the controller as that Portal user.
- No runtime request is refused only for lacking a token.
- Pest with Http::fake() covers the no-token case, the valid-token case, and the bad-token case as resolved by the brain on the issue 'How ResolvePortalIdentity checks a token and handles a bad one'.
- Against the real Portal 12.0, the token check resolves a real user's token to that username, as run by the user in the local checklist.

Local checklist (user runs):

- Deploy the branch to the builder app's IIS site, with PORTAL_URL set to the real Portal 12.0.
- Get a Portal token for a test user (how is unverified; see issue 'Laravel using the widget's Portal token server-side').
- On that server, run the token check that ResolvePortalIdentity uses through PortalClient with that token. Confirm it resolves to the test user's username.
- Run it with no token and confirm the identity is anonymous.
- Run it with an expired or altered token and confirm the result matches the brain's resolution of the bad-token issue.

Issues: ISS-66, ISS-70, ISS-121, ISS-122, ISS-123

Sources: R033, R090, R091, R092, R225, R229, R267, R274, R276, R020, R448, R449, R012, R013, R014, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:39, /home/user/arcgisbuilderwebapplication/CLAUDE.md:125, /home/user/arcgisbuilderwebapplication/CLAUDE.md:130-133, /home/user/arcgisbuilderwebapplication/CLAUDE.md:196, /home/user/arcgisbuilderwebapplication/CLAUDE.md:218, /home/user/arcgisbuilderwebapplication/CLAUDE.md:261-270, /home/user/ArcGISRunner/CLAUDE.md:84-88, /home/user/ArcGISRunner/CLAUDE.md:174-175, /home/user/ArcGISRunner/CLAUDE.md:201-202, decision-log:D18

### B3.4 WebmapAccess: check webmap visibility as the user or anonymously, short cache

**Repo:** builder · **Verification:** cloud tests · **Depends on:** B1.1, B3.3

Add a service that answers whether an identity (a Portal user or anonymous) can open a webmap. It asks Portal for the webmap item as that identity and caches the answer briefly.

Files: `app/Services/WebmapAccess.php`, `app/Services/PortalClient.php`

Steps:

1. Create WebmapAccess in app/Services/ and inject PortalClient through the constructor.
2. For a signed-in identity, request the webmap item from Portal through PortalClient with that user's token.
3. For an anonymous identity, request the webmap item without a token.
4. Answer yes when Portal returns the item and no when Portal denies it. How Portal 12.0 reports a denial is unverified (see issues).
5. Cache the answer briefly. The duration, cache key and cache store are open; the brain resolves them before the task starts (see issues).
6. Deliver on a branch from main: check the matching box in docs/TASKS.md and open a pull request into main. Never push to main. The brain reviews the pull request and the user merges it.

Acceptance:

- Against Http::fake() fixtures, for a publicly shared webmap, the answer is yes for anonymous and for signed-in identities.
- Against Http::fake() fixtures, for a webmap not shared publicly, the answer is no for anonymous.
- Against Http::fake() fixtures, for a webmap shared with a user, the answer is yes for that user.
- A repeat check inside the cache window doesn't call Portal again.
- Pest with Http::fake() Portal item fixtures covers each case above.
- The cloud tests cover only the faked Portal contract. The real-Portal sharing check is deferred to B3.5's local checklist.

Issues: ISS-27, ISS-70, ISS-123, ISS-124, ISS-125

Sources: R033, R090, R094, R225, R230, R026, R274, R276, R020, R448, R012, R013, R014, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:40, /home/user/arcgisbuilderwebapplication/CLAUDE.md:134-136, /home/user/arcgisbuilderwebapplication/CLAUDE.md:226, /home/user/arcgisbuilderwebapplication/CLAUDE.md:261-270, /home/user/ArcGISRunner/CLAUDE.md:201-202, decision-log:D18

### B3.5 Runtime profile endpoints (listing by webmapId, one profile) + CORS

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B0.6, B3.2, B3.3, B3.4

Serve published profiles to the widget. `GET /api/runtime/profiles?webmapId=` feeds the settings dropdown, and `GET /api/runtime/profiles/{profileId}` returns one profile. Access follows Portal sharing of the webmap, and CORS is limited to RUNNER_ALLOWED_ORIGINS. This task's local checklist is also the real-Portal check for B3.4 and the end-to-end check of B3.3.

Files: `app/Http/Controllers/Runtime/`, `/routes/web.php`, `/routes/api.php`, `app/Services/ProfileStore.php`, `app/Services/WebmapAccess.php`, `app/Http/Middleware/ResolvePortalIdentity.php`

Steps:

1. Add `GET /api/runtime/profiles?webmapId=` and `GET /api/runtime/profiles/{profileId}` in a controller under app/Http/Controllers/Runtime/. Put both behind ResolvePortalIdentity, with no sign-in gate.
2. For the listing, ask WebmapAccess whether the identity can open the webmapId. If it can, return the published profiles for that webmap in the listing shape from CONFIG_OUTPUT_SCHEMA.md (see the conflict issue about publishedAt). The response when it can't is open (see issues).
3. For one profile, read `profiles/{profileId}.json` through ProfileStore, ask WebmapAccess about that profile's webmapId, and return the profile only if the identity can open it.
4. Serve published profiles only. Never serve drafts.
5. Read RUNNER_ALLOWED_ORIGINS from configuration and allow cross-origin calls to the runtime routes only from those origins. The CORS mechanism is unverified (see issues).
6. Keep the controller thin. Put logic in app/Services and app/Runner.
7. Deliver on a branch from main: check the matching box in docs/TASKS.md and open a pull request into main. Never push to main. The brain reviews the pull request and the user merges it.

Acceptance:

- For a publicly shared webmap, an anonymous caller sees its published profiles in the listing and gets each profile as published from the single-profile endpoint.
- A caller who can't open the webmap gets no profile data from either endpoint.
- A signed-in caller with a valid token gets profiles for webmaps shared with them.
- Drafts never appear in either endpoint.
- Browser calls from an origin in RUNNER_ALLOWED_ORIGINS succeed. Calls from other origins are blocked by CORS.
- After a republish, the single-profile endpoint returns the new version.
- Pest with Http::fake() and a temporary CONFIG_ROOT covers each case above.

Local checklist (user runs):

- Deploy the builder app to its IIS site with RUNNER_ALLOWED_ORIGINS set to the Portal origin and CONFIG_ROOT set to the UNC share.
- Publish a profile for a webmap shared publicly. Without a token, call `GET {APP_URL}/api/runtime/profiles?webmapId={webmapId}` and confirm the profile is listed.
- Without a token, call `GET {APP_URL}/api/runtime/profiles/{profileId}` and confirm the JSON matches the file on the share.
- Publish a profile for a webmap shared only with the organization or a group. Call both endpoints without a token and confirm no profile data comes back.
- Repeat with `Authorization: Bearer <token>` for a user who can open that webmap and confirm the profile comes back. This also checks ResolvePortalIdentity and WebmapAccess end to end against the real Portal.
- Repeat with a token for a user who can't open the webmap and confirm no profile data comes back.
- Change the webmap's sharing in Portal. After the short cache window, confirm the endpoints follow the new sharing.
- From the browser console on a Portal page, fetch the listing with an Authorization header and confirm CORS allows it. From a page on another origin, confirm the browser blocks it.
- Republish the profile and fetch it again. Confirm the change shows.

Issues: ISS-09, ISS-25, ISS-26, ISS-31, ISS-67, ISS-68, ISS-82, ISS-123, ISS-126, ISS-127, ISS-128, ISS-129, ISS-130, ISS-131, ISS-132

Sources: R033, R090, R091, R092, R094, R107, R110, R115, R123, R124, R207, R209, R235, R237, R238, R249, R261, R026, R020, R274, R275, R012, R013, R014, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41, /home/user/arcgisbuilderwebapplication/CLAUDE.md:130-136, /home/user/arcgisbuilderwebapplication/CLAUDE.md:154-157, /home/user/arcgisbuilderwebapplication/CLAUDE.md:200, /home/user/arcgisbuilderwebapplication/CLAUDE.md:215, /home/user/arcgisbuilderwebapplication/CLAUDE.md:261-265, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:8-9, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:32-33, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:14-16, decision-log:P4, decision-log:D17, decision-log:D18

### B3.6 EditGate + POST /api/runtime/profiles/{id}/edits/{layerId} → applyEdits; anonymous rate limit

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B1.1, B1.4, B2.3, B2.4, B3.2, B3.3, B3.5

Add the single write path for crud. `POST /api/runtime/profiles/{profileId}/edits/{layerId}` checks each edit against the published profile with EditGate, then sends it to the feature service with applyEdits as the user or anonymously. Anonymous edits are rate-limited per IP by RUNNER_ANON_EDITS_PER_MINUTE. PHP hooks around applyEdits come later in B4.3.

Files: `app/Runner/Kinds/Crud/ (EditGate)`, `app/Http/Controllers/Runtime/`, `app/Runner/KindRegistry.php`, `app/Services/PortalClient.php`, `/routes/web.php`, `/routes/api.php`

Steps:

1. Add the POST route in a controller under app/Http/Controllers/Runtime/. Put it behind ResolvePortalIdentity and the same CORS rule as B3.5, with no sign-in gate.
2. Load the published profile and find the LayerConfig whose layerId matches the route. Refuse the edit if the layer isn't in the profile.
3. Hand the edit to crud's runtime handler through KindRegistry. The handler shape is open; the brain resolves it before the task starts (see issues).
4. In EditGate (app/Runner/Kinds/Crud/), apply the crud rules again. Refuse an add, update or delete when the matching `pages.add/edit/delete` is false or the matching capability is false.
5. In EditGate, allow attribute changes only to fields whose `editable` is true and whose `inputType` isn't `readonly`. Treat system fields (objectId, globalId, editor-tracking, Shape__Area/Length) as readonly. Whether disallowed fields reject the edit or are dropped is open (see issues).
6. Send allowed edits to the layer's feature service applyEdits with the user's token when signed in and with no token when anonymous. The service's own sharing and editing settings decide the result.
7. Return the service's result and error messages to the caller so the widget can show them. The request and response shapes follow the brain's resolution of the issue 'Edit endpoint request and response contract', made before the task starts.
8. Rate-limit anonymous edit requests per client IP to RUNNER_ANON_EDITS_PER_MINUTE, read from configuration.
9. Never execute PHP text from the profile or the request. Don't run PHP hooks in this task. B4.3 adds the before* and after* hooks around applyEdits.
10. Deliver on a branch from main: check the matching box in docs/TASKS.md and open a pull request into main. Never push to main. The brain reviews the pull request and the user merges it.

Acceptance:

- An edit for a layer that isn't in the profile is refused and never reaches the feature service.
- An add, update or delete that the profile's pages or capabilities don't allow is refused before applyEdits.
- A change to a field that is readonly, not editable, or a system field is not sent to the feature service.
- An allowed edit reaches applyEdits with the caller's token, or with no token for an anonymous caller.
- An edit the profile allows but the live service forbids is refused by the service, and the caller gets its message.
- When the feature service refuses an edit, the caller gets the service's message.
- No PHP text from the profile or the request is ever executed.
- Anonymous requests beyond RUNNER_ANON_EDITS_PER_MINUTE in one minute from one IP are refused.
- Pest with Http::fake() for applyEdits and community/self and a temporary CONFIG_ROOT covers each case above.

Local checklist (user runs):

- Deploy the builder app to IIS. Publish a profile with a layer whose service allows editing and whose pages allow add, edit and delete.
- Using the request body format resolved by the brain on the issue 'Edit endpoint request and response contract', POST an add with `Authorization: Bearer <token>` for a user who can edit the layer. Confirm the feature appears in the service, and if editor tracking is on, that it records that user.
- POST an update to a field the profile marks readonly. Confirm the field doesn't change in the service.
- Set `pages.delete` to false for the layer, republish, and POST a delete. Confirm it's refused and the feature still exists.
- Without a token, POST an edit to a layer that isn't editable anonymously. Confirm the service's error message comes back.
- Without a token, POST edits to a publicly editable layer more than RUNNER_ANON_EDITS_PER_MINUTE times within one minute from one machine. Confirm the extra requests are refused.
- Record which client IP the rate limit counts, and whether all clients share one IP (see issue 'Client IP seen by Laravel under IIS').
- From the browser console on a Portal page, POST an edit with an Authorization header. Confirm CORS allows it.

Issues: ISS-09, ISS-25, ISS-26, ISS-37, ISS-68, ISS-70, ISS-82, ISS-85, ISS-109, ISS-127, ISS-130, ISS-131, ISS-133, ISS-134, ISS-135, ISS-136, ISS-137, ISS-138, ISS-139

Sources: R033, R083, R084, R086, R093, R095, R098, R109, R125, R194, R195, R196, R216, R231, R232, R233, R239, R244, R250, R264, R311, R314, R388, R389, R026, R020, R274, R275, R012, R013, R014, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:30-31, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:42, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:49, /home/user/arcgisbuilderwebapplication/CLAUDE.md:24-26, /home/user/arcgisbuilderwebapplication/CLAUDE.md:137-142, /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, /home/user/arcgisbuilderwebapplication/CLAUDE.md:190, /home/user/arcgisbuilderwebapplication/CLAUDE.md:201, /home/user/arcgisbuilderwebapplication/CLAUDE.md:219-221, /home/user/arcgisbuilderwebapplication/CLAUDE.md:261-265, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:64-68, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:127-135, /home/user/ArcGISRunner/CLAUDE.md:135-140, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:17, decision-log:D9, decision-log:P4, decision-log:P13, decision-log:P14

### B3.7 Widget deploy script: copy Developer Edition 1.18 build output into public/widgets/arcgis-runner/

**Repo:** builder · **Verification:** local checklist · **Depends on:** B0.5, B0.8, W0.6, W0.7

Add a deploy script that copies the Developer Edition 1.18 build output (`client/dist/widgets/arcgis-runner/`) into the builder app's `public/widgets/arcgis-runner/`. IIS serves that folder as static files, and Portal's widget item points at its manifest.json. The deployed files aren't committed. The script follows what the widget deployment spike (W0.7) proved about hosting, CORS and Portal registration.

Files: `public/widgets/arcgis-runner/`, `.gitignore`, `Deploy script (location open; see issue 'Deploy script form, location and where it runs')`

Steps:

1. Add `public/widgets/arcgis-runner/` to the builder repo's .gitignore. The docs call the folder gitignored, but the current .gitignore doesn't list it.
2. Write the deploy script. Its form, location and the machine it runs on follow the brain's or user's resolution of the issue 'Deploy script form, location and where it runs', made before the task starts.
3. Have the script copy the contents of the Developer Edition checkout's `client/dist/widgets/arcgis-runner/` into `public/widgets/arcgis-runner/` of the builder app's IIS site.
4. Make sure the folder still holds the web.config that adds CORS headers for the Portal origin after a copy. Where that file comes from is open (see issues).
5. Use the same script for every redeploy, including the rebuild with the matching Developer Edition after a Portal upgrade.
6. Deliver on a branch from main: check the matching box in docs/TASKS.md and open a pull request into main. Never push to main. The brain reviews the pull request and the user merges it.

Acceptance:

- After a Developer Edition 1.18 build and a script run, IIS serves `{APP_URL}/widgets/arcgis-runner/manifest.json` from `public/widgets/arcgis-runner/`.
- Responses from that folder carry CORS headers for the Portal origin.
- `git status` in the builder repo shows no deployed widget files.
- An experience using the Portal widget item that points at `{APP_URL}/widgets/arcgis-runner/manifest.json` loads the newly deployed build.

Local checklist (user runs):

- In the Developer Edition 1.18 checkout, build the widget so `client/dist/widgets/arcgis-runner/` exists.
- Run the deploy script.
- Confirm the files and the web.config are in `public/widgets/arcgis-runner/` on the IIS site.
- Open `{APP_URL}/widgets/arcgis-runner/manifest.json` in a browser and confirm it loads.
- From the browser console on a Portal page, fetch the manifest. Confirm the response has CORS headers for the Portal origin.
- Run `git status` in the builder repo and confirm no deployed widget files show.
- If the widget isn't registered yet, register `{APP_URL}/widgets/arcgis-runner/manifest.json` in Portal 12.0 as a Portal admin (Add Item → Experience Builder widget).
- Open an experience that uses Runner and confirm it loads the newly deployed build.

Issues: ISS-52, ISS-54, ISS-55, ISS-56, ISS-60, ISS-69, ISS-140, ISS-141, ISS-142, ISS-143, ISS-208

Sources: R004, R033, R048, R074, R089, R111, R112, R113, R201, R202, R206, R253, R327, R328, R414, R439, R020, R012, R013, R014, R446, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, /home/user/arcgisbuilderwebapplication/CLAUDE.md:148-153, /home/user/arcgisbuilderwebapplication/CLAUDE.md:230, /home/user/arcgisbuilderwebapplication/CLAUDE.md:261-265, /home/user/arcgisbuilderwebapplication/.gitignore:1-6, /home/user/ArcGISRunner/CLAUDE.md:68-71, /home/user/ArcGISRunner/CLAUDE.md:75-79, /home/user/ArcGISRunner/docs/TASKS.md:11-12, decision-log:D12, decision-log:D14, decision-log:D22, decision-log:O6, decision-log:P20

## B4 — Builder Phases 4–6 — Custom code, polish, next kinds

This section adds the Custom code wizard step to the builder. It has two parts. The first is a JS handler editor for each layer and crud event, started only if the widget CSP spike (W0.10) passes. The second is a PHP hook system (LayerHook, HookRejected, HookRegistry) that runs a layer's hook around applyEdits when that layer has a phpHook set. Polish comes next: a drift warning for saved layers or fields that the service no longer has, then error handling and session timeout UX, whose scope the brain and the user agree first. The section ends with a design gate. The brain session decides and designs the second kind before anyone builds it.

### B4.1 Custom code step: JS handler editor per layer/event (gated by widget CSP spike)

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** W0.10, B1.6, B2.2, B3.2

In the common Custom code wizard step, give builder-group members a JavaScript handler editor for each included layer and each event the kind defines. Store each handler as a function body in that layer's `customJs`. Work starts only after the widget repo's CSP spike (W0.10) shows that handlers compiled from text can run in a Portal-hosted experience.

Files: `resources/js/wizard/common/`, `resources/js/wizard/kinds/crud/`, `app/Runner/Kinds/Crud/ (CrudSettingsValidator)`, `tests/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and branch from main.
2. Read the CSP spike result (W0.10, widget repo docs/TASKS.md Phase 0). If handlers are blocked, stop. The user picks fallback (a) or (b) in the brain session before any work here.
3. Have the crud kind module in resources/js/wizard/kinds/crud/ supply its event set to the SPA kind registry: onPageLoad, onFieldChange, beforeSave, afterSave, beforeDelete.
4. In resources/js/wizard/common/, build the Custom code step. For each layer in the draft's settings.layers, show one handler editor per event from the kind's event set.
5. Save each handler as a function body string in settings.layers[i].customJs[event], through the wizard's existing draft autosave.
6. In CrudSettingsValidator, limit customJs keys to CrudJsEvent values and require string values, so Review & Publish validation covers them.
7. Keep Laravel to storing the handler text. The widget runs it.
8. Write Pest tests with a temporary CONFIG_ROOT. A draft with customJs saves and loads. The validator rejects an unknown event key. The published file carries customJs.
9. Check the box in docs/TASKS.md. Open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- Work starts only after the CSP spike (W0.10) passes.
- The Custom code step shows a handler editor for each included layer and each crud event (onPageLoad, onFieldChange, beforeSave, afterSave, beforeDelete).
- The event list comes from the crud kind module, not from the common step.
- Handler text is saved to the draft as a function body under the event key in that layer's customJs, and it survives a reload.
- The published profile carries customJs per layer in the shape Partial<Record<CrudJsEvent, string>>.
- The kind validator rejects customJs keys that aren't CrudJsEvent values.
- Pest tests pass in the cloud.

Local checklist (user runs):

- Get the PR branch onto the real servers. The sources don't define how (see issue).
- Sign in to the builder as a member of the allowed Portal group.
- Open a crud draft that has at least one layer selected, then go to the Custom code step.
- Confirm that each selected layer shows an editor for onPageLoad, onFieldChange, beforeSave, afterSave and beforeDelete.
- Type a handler body for one event, leave the step, and reload the page. Confirm the text is still there.
- Publish the profile. Open {CONFIG_ROOT}/profiles/{profileId}.json on the share and confirm that layer's customJs holds the text under the event key.

Issues: ISS-01, ISS-03, ISS-11, ISS-20, ISS-28, ISS-66, ISS-75, ISS-76, ISS-77, ISS-91, ISS-119, ISS-200, ISS-201

Sources: R034, R088, R137, R138, R140, R141, R142, R143, R056, R166, R185, R285, R312, R012, R013, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47, /home/user/arcgisbuilderwebapplication/CLAUDE.md:56-57, /home/user/arcgisbuilderwebapplication/CLAUDE.md:176-182, /home/user/arcgisbuilderwebapplication/CLAUDE.md:263-264, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:61, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:100-124, /home/user/ArcGISRunner/docs/TASKS.md:15

### B4.2 `LayerHook`, `HookRejected`, `HookRegistry` (discovers `app/Hooks/*`)

**Repo:** builder · **Verification:** cloud tests · **Depends on:** B0.5, B0.7

Define the `App\Runner\LayerHook` interface, the `HookRejected` exception, and a `HookRegistry` that discovers hook classes in `app/Hooks/`. Hooks are reviewed PHP classes in git. The server never runs PHP text from a profile or the browser.

Files: `app/Runner/LayerHook.php`, `app/Runner/HookRejected.php`, `app/Runner/HookRegistry.php`, `app/Hooks/`, `tests/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and branch from main.
2. Agree the method signatures and the key format with the brain before coding (see issues).
3. Create app/Runner/LayerHook.php. It declares interface App\Runner\LayerHook with beforeAdd, afterAdd, beforeUpdate, afterUpdate, beforeDelete and afterDelete.
4. Create app/Runner/HookRejected.php. It is an exception thrown as HookRejected($message).
5. Create app/Runner/HookRegistry.php. It discovers the classes in app/Hooks/ that implement LayerHook, lists their keys, and resolves a key to its hook.
6. Create the app/Hooks/ folder.
7. Make HookRegistry available to services by constructor injection, following the Laravel conventions in CLAUDE.md.
8. Write Pest tests. Discovery lists the classes that implement LayerHook and ignores the others. A key resolves to its hook. HookRejected carries its message.
9. Check the box in docs/TASKS.md and open a pull request into main. Never push to main directly.

Acceptance:

- LayerHook declares beforeAdd, afterAdd, beforeUpdate, afterUpdate, beforeDelete and afterDelete.
- HookRejected($message) can be thrown and carries its message.
- A class added to app/Hooks/ that implements LayerHook appears in HookRegistry with no other registration step.
- HookRegistry resolves a listed key to its hook class.
- Pest tests pass in the cloud.

Issues: ISS-119, ISS-202, ISS-203

Sources: R240, R241, R242, R243, R244, R265, R266, R313, R026, R012, R013, /home/user/arcgisbuilderwebapplication/CLAUDE.md:184-190, /home/user/arcgisbuilderwebapplication/CLAUDE.md:222-223, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:48

### B4.3 Run hooks around `applyEdits` in the edit endpoint

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B4.2, B3.6

In `POST /api/runtime/profiles/{profileId}/edits/{layerId}`, run the layer's PHP hook around `applyEdits` when the layer's `phpHook` is set. The order is EditGate, the `before*` hook, then `applyEdits` as the user or anonymously, then the `after*` hook.

Files: `app/Http/Controllers/Runtime/`, `app/Runner/HookRegistry.php`, `app/Runner/Kinds/Crud/ (EditGate)`, `tests/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and branch from main.
2. Read the layer's phpHook from the published profile on the share for the requested profileId and layerId.
3. When phpHook is null, keep the existing flow (EditGate, then applyEdits).
4. Otherwise, resolve the hook through HookRegistry. Call EditGate, then the before* method, then applyEdits as the user or anonymously, then the after* method.
5. Map an add to beforeAdd/afterAdd, an update to beforeUpdate/afterUpdate, and a delete to beforeDelete/afterDelete.
6. Send any attribute changes made by a before* method on to applyEdits.
7. On HookRejected, skip applyEdits and return the hook's message to the widget.
8. Keep the controller thin. Put the hook orchestration in app/Runner.
9. Write Pest tests with Http::fake() for applyEdits and a test-only hook class. Cover the call order, a before* attribute change reaching applyEdits, HookRejected stopping the edit, phpHook null, and the same hook flow on an anonymous edit.
10. Don't check the TASKS.md:49 box on its own. That one line covers both B4.3 and B4.4 (see issue). Open a pull request into main that ends with the local test checklist, because the task reads the share. Never push to main directly.

Acceptance:

- For a layer with phpHook set, the edit endpoint calls EditGate, then before*, then applyEdits, then after*.
- Attribute changes made by a before* hook reach applyEdits.
- When a before* hook throws HookRejected, no applyEdits request reaches the feature service and the response carries the hook's message.
- A layer with phpHook null edits exactly as it did before this task.
- A hook runs on an anonymous edit the same way it runs on a signed-in edit.
- applyEdits is still sent as the user or anonymously, so the service's own permissions decide the edit.
- The RUNNER_ANON_EDITS_PER_MINUTE per-IP limit still applies to anonymous edits.
- Only HookRegistry classes run. No PHP text from a profile or request is executed.
- Pest tests with Http::fake() pass in the cloud.

Local checklist (user runs):

- Get the PR branch onto the real servers. The sources don't define how (see issue).
- Publish a crud profile from the builder so its file is on the share under {CONFIG_ROOT}/profiles/{profileId}.json, with phpHook null on an editable layer.
- Send an edit for that layer through the edit endpoint. The only documented way to send one is Runner's edit client (see issue). Confirm the edit reaches the feature service exactly as before this task.
- The edit with a hook set is checked end to end in B4.5's local checklist.

Issues: ISS-66, ISS-133, ISS-139, ISS-202, ISS-204, ISS-205, ISS-206, ISS-207, ISS-208

Sources: R020, R083, R090, R095, R109, R125, R231, R232, R233, R239, R241, R244, R314, R012, R013, /home/user/arcgisbuilderwebapplication/CLAUDE.md:24-26, /home/user/arcgisbuilderwebapplication/CLAUDE.md:137-142, /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, /home/user/arcgisbuilderwebapplication/CLAUDE.md:186-190, /home/user/arcgisbuilderwebapplication/CLAUDE.md:268-270, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:42, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:49, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:62, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:134-135

### B4.4 Hook picker in the custom code step

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B4.2, B1.6, B2.2, B3.2

In the Custom code step, for kinds that write data (crud), let the builder pick one PHP hook per layer from HookRegistry, or none. Store the choice as the layer's `phpHook` key. The builder only picks a hook class and never accepts PHP text.

Files: `resources/js/wizard/common/`, `app/Http/Controllers/Builder/`, `app/Runner/HookRegistry.php`, `app/Runner/Kinds/Crud/ (CrudSettingsValidator)`, `tests/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and branch from main.
2. Confirm with the brain whether the Custom code step already exists from B4.1 or this task creates it (see issue).
3. Make the HookRegistry key list available to the SPA from Laravel, under the group-gated Builder controllers (EnsurePortalGroupMember). The route isn't named in the sources (see issue).
4. In the Custom code step, show a hook picker for each layer, using Calcite Components. It lists every HookRegistry key plus a none choice.
5. Save the choice to settings.layers[i].phpHook (a key, or null for none) through the draft autosave.
6. In CrudSettingsValidator, accept phpHook only when it is null or a key that HookRegistry knows.
7. Write Pest tests. The key list matches HookRegistry, a non-member of the allowed group is refused, a draft with phpHook saves and loads from a temporary CONFIG_ROOT, and the validator rejects an unknown key.
8. Check the TASKS.md:49 box only if B4.3 has also landed (see issue). Open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- The hook list is served only to members of the allowed Portal group.
- The Custom code step shows a hook picker for each layer, listing every HookRegistry key plus none.
- The choice is saved in the draft as phpHook (key or null) and survives a reload.
- The published profile carries phpHook for each layer.
- The kind validator rejects a phpHook value that HookRegistry doesn't know.
- No part of the step accepts PHP text.
- Pest tests pass in the cloud.

Local checklist (user runs):

- Get the PR branch onto the real servers. The sources don't define how (see issue).
- Sign in to the builder as a member of the allowed Portal group.
- Open a crud draft and go to the Custom code step. Confirm each layer has a hook picker that lists the classes in app/Hooks/ plus none.
- Pick a hook for one layer and leave another layer on none. Reload, and confirm both choices are still there.
- Publish the profile. Open {CONFIG_ROOT}/profiles/{profileId}.json on the share. Confirm the first layer's phpHook holds the key and the second layer's is null.

Issues: ISS-28, ISS-119, ISS-200, ISS-203, ISS-204, ISS-205, ISS-209

Sources: R167, R215, R216, R228, R243, R260, R262, R285, R315, R012, R013, /home/user/arcgisbuilderwebapplication/CLAUDE.md:56-57, /home/user/arcgisbuilderwebapplication/CLAUDE.md:128-129, /home/user/arcgisbuilderwebapplication/CLAUDE.md:189-190, /home/user/arcgisbuilderwebapplication/CLAUDE.md:214, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:62, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:49

### B4.5 Example hook + tests

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B4.2, B4.3, B4.4, B3.7, W3.4, W0.7

Add one example hook class in `app/Hooks/` and Pest tests that run it through HookRegistry and the edit endpoint. Confirm on the real servers that an edit through Runner runs the layer's hook.

Files: `app/Hooks/`, `tests/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and branch from main.
2. Agree what the example hook does with the brain before coding (see issue).
3. Add one class in app/Hooks/ that implements App\Runner\LayerHook.
4. Write Pest tests. HookRegistry lists the example, and the picker's key list includes it. The edit endpoint runs it around a faked applyEdits (Http::fake()) and covers the before*/after* paths the example uses.
5. Check the box in docs/TASKS.md. Open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- An example hook class in app/Hooks/ implements LayerHook.
- HookRegistry lists the example hook and the hook picker offers it.
- Pest tests run the example around a faked applyEdits and pass in the cloud.
- On the real servers, an edit through Runner on a layer that uses the example hook shows the hook's effect or its rejection message.

Local checklist (user runs):

- Get the PR branch onto the real servers. The sources don't define how (see issue).
- In the builder, select the example hook for one layer of a test webmap and publish the profile.
- In a Portal 12.0 experience with Runner showing that profile, make an edit the example hook acts on.
- Confirm the result in the feature service matches what the example hook is written to do. If the hook rejects the edit, confirm Runner shows the hook's message and the feature is unchanged.

Issues: ISS-202, ISS-208, ISS-210

Sources: R109, R111, R125, R202, R274, R275, R276, R316, R388, R012, R013, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:50, /home/user/arcgisbuilderwebapplication/CLAUDE.md:110, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-270, /home/user/ArcGISRunner/docs/TASKS.md:12, /home/user/ArcGISRunner/docs/TASKS.md:40

### B4.6 Drift warning: saved layers/fields no longer in the service

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B2.1, B1.6

Warn the builder when a profile's saved layers or fields are no longer in the webmap's services.

Files: `app/Services/PortalClient.php`, `TBD in task`, `tests/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and branch from main.
2. Agree with the brain when the check runs, whether it blocks publish, how saved layers are matched, and what the warning names (see issue).
3. Fetch the webmap's current layers, tables and schemas through the existing layer detection, which uses PortalClient.
4. Compare the saved layers and saved field names (FieldConfig.name) with the current ones, using the matching rule agreed with the brain.
5. Show the warning in the SPA in the agreed form.
6. Write Pest tests with Http::fake() fixtures. One saved layer is missing, one saved field is missing, and one case has nothing missing.
7. Check the box in docs/TASKS.md. Open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- A warning appears when saved layers or fields are no longer in the service.
- A profile with nothing missing shows no warning.
- Portal reads go through PortalClient.
- Pest tests with Http::fake() pass in the cloud.

Local checklist (user runs):

- Get the PR branch onto the real servers (the sources don't define how; see issue) and sign in.
- Use a test webmap and service where you can remove a field or layer. Save a draft that includes it, then remove it from the service.
- Trigger the drift check at the point agreed with the brain and confirm the warning appears.
- Trigger the check for a draft with no removed layers or fields and confirm no warning appears.

Issues: ISS-13, ISS-66, ISS-211

Sources: R035, R198, R225, R290, R291, R012, R013, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:54, /home/user/arcgisbuilderwebapplication/CLAUDE.md:252, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:72

### B4.7 Error handling, session timeout UX

**Repo:** builder · **Verification:** cloud tests + local checklist · **Depends on:** B1.1, B1.2, B1.3

Handle errors in the builder, and the builder session timing out, with UX the brain and the user agree first. The sources name this item but don't define it.

Files: `TBD in task`, `tests/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md and branch from main.
2. Before coding, the brain and the user define which error cases are covered and how a session timeout should behave (see issue).
3. Implement the agreed behaviour in Laravel and the SPA.
4. Write Pest tests for the agreed server-side responses, with Http::fake() and a temporary CONFIG_ROOT.
5. Check the box in docs/TASKS.md. Open a pull request into main that ends with the local test checklist, because it touches sign-in. Never push to main directly.

Acceptance:

- Each agreed error case shows the agreed behaviour.
- An expired builder session shows the agreed behaviour.
- Builder tokens still stay in the server session only.
- The group check still runs at login and on every save/publish.
- Pest tests pass in the cloud.

Local checklist (user runs):

- Get the PR branch onto the real servers (the sources don't define how; see issue) and sign in.
- Let the builder session expire, then trigger a save. Confirm the agreed session-timeout behaviour.
- Trigger each agreed error case that is safe to trigger on the real servers. Confirm the agreed behaviour for each.

Issues: ISS-28, ISS-66, ISS-80, ISS-89, ISS-212

Sources: R035, R227, R228, R317, R318, R020, R012, R013, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:55, /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-129, /home/user/arcgisbuilderwebapplication/CLAUDE.md:252, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-270

### B4.8 Decide and design the second kind (brain session) before any build

**Repo:** both · **Verification:** review only

In the brain session, the user and the brain decide what the second kind is and design both halves and its settings shape. No code for it is built before that.

Files: `docs/CONFIG_OUTPUT_SCHEMA.md (builder repo)`, `/CLAUDE.md (builder and widget repos)`, `docs/TASKS.md (builder and widget repos)`

Steps:

1. The user and the brain name the second kind.
2. Define the builder half in app/Runner/Kinds/{Kind}/ and resources/js/wizard/kinds/{kind}/: its wizard steps, its settings validator, and any runtime endpoints it needs.
3. Define the widget half in widgets/arcgis-runner/src/runtime/kinds/{kind}/: its React UI, loaded only when a profile of that kind is used.
4. Write its settings shape and its JavaScript event set into docs/CONFIG_OUTPUT_SCHEMA.md in the builder repo first.
5. Decide whether the kind writes data, which decides whether the Custom code step offers a PHP hook per layer.
6. Decide schemaVersion handling under the rule that, once the widget ships, any change to the profile shape bumps schemaVersion.
7. Update the in-scope and out-of-scope kind lines identically in the Project scope section of both CLAUDE.md files, since 'Kinds other than crud' is out of scope for v1.
8. Update each CLAUDE.md's kind sections to describe the new kind: the builder's app/Runner/Kinds and the widget's kinds/registry.ts.
9. Add the build tasks to both repos' docs/TASKS.md.
10. Land the doc changes by the route agreed for brain doc edits (see issue).

Acceptance:

- The second kind is named and recorded.
- docs/CONFIG_OUTPUT_SCHEMA.md holds its settings shape and its JavaScript event set.
- Both CLAUDE.md files describe the new kind in their kind sections.
- Both backlogs list its build tasks.
- The Project scope sections are identical in both repos.
- No build task for the kind starts before the design is in the docs.

Issues: ISS-05, ISS-213, ISS-214

Sources: R036, R071, R072, R096, R117, R118, R119, R023, R003, R015, R078, R079, R080, R138, R285, R356, R360, decision-log:O10, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:59, /home/user/arcgisbuilderwebapplication/CLAUDE.md:28-40, /home/user/arcgisbuilderwebapplication/CLAUDE.md:97, /home/user/arcgisbuilderwebapplication/CLAUDE.md:253, /home/user/ArcGISRunner/CLAUDE.md:50, /home/user/ArcGISRunner/CLAUDE.md:161, /home/user/ArcGISRunner/CLAUDE.md:185-186, decision-log:D16

## W0 — Widget Phase 0 — Prove deployment

W0 creates main in the widget repo, sets up the cloud Vitest harness for jimu-free lib/ code, and confirms that a minimal widget at exbVersion 1.18.0 builds in Developer Edition 1.18. It then runs four spikes against the real Portal 12.0: deployment (W0.7), auth including a publicly shared experience (W0.8), the #runner= URL hash (W0.9), and CSP (W0.10). Their results show whether the Phase 1-3 design holds. The CSP result decides whether the widget's JS handler runner and the builder's JS handler editor go ahead, or which documented fallback the user picks.

### W0.1 Repo skeleton + CLAUDE.md (done)

**Repo:** widget · **Verification:** review only

Widget repo skeleton, brain doc, backlog, README pointers, .gitignore and the widget config doc exist.

Files: `/CLAUDE.md`, `/docs/TASKS.md`, `/README.md`, `/.gitignore`, `/docs/CONFIG_SCHEMA.md`

Sources: R005, R006, R038, R043, R044, R047, R347, /home/user/ArcGISRunner/docs/TASKS.md:7, /home/user/ArcGISRunner/README.md:8-9, /home/user/ArcGISRunner/.gitignore:1-4, CLAUDE.md:147

### W0.2 Decisions: one registered widget + profiles, kinds, Developer Edition 1.18, hosted by the builder app (done)

**Repo:** widget · **Verification:** review only

The deployment model, kinds, Developer Edition version and hosting decisions are recorded in CLAUDE.md.

Files: `/CLAUDE.md`

Sources: R072, R073, R074, R327, decision-log:D13, decision-log:D14, decision-log:D15, decision-log:D16, /home/user/ArcGISRunner/docs/TASKS.md:8

### W0.3 Create main from claude/arcgis-runner-setup-uwxnyf

**Repo:** widget · **Verification:** review only

Create main in the widget repo from branch claude/arcgis-runner-setup-uwxnyf so task sessions can branch from main and open pull requests into it. The user approved creating main ('before creating main'). Who creates it is open: O2 names no actor, TASKS.md:9 marks the item as the user's, and task sessions never push to main (see issue 'Creating main: actor, box check, and later doc pushes').

Files: TBD in task

Steps:

1. Settle who creates main (actor open; see issue 'Creating main: actor, box check, and later doc pushes').
2. Confirm the head of claude/arcgis-runner-setup-uwxnyf holds the current planning docs (CLAUDE.md, docs/TASKS.md, docs/CONFIG_SCHEMA.md, README.md).
3. The chosen actor creates main on origin from that branch head.
4. Confirm main exists so the user can make it the default branch (W0.4).

Acceptance:

- origin has a main branch at the same commit as the head of claude/arcgis-runner-setup-uwxnyf when main was created.

Issues: ISS-01, ISS-02, ISS-05, ISS-40, ISS-41, ISS-42

Sources: R055, R061, R012, R013, decision-log:O2, decision-log:O10, /home/user/ArcGISRunner/docs/TASKS.md:9, CLAUDE.md:182-184

### W0.4 Make main the default branch (user, GitHub settings)

**Repo:** user · **Verification:** user action · **Depends on:** W0.3

The user sets main as the default branch of kschultzBGOH/ArcGISRunner in GitHub settings.

Files: `/docs/TASKS.md`

Steps:

1. In GitHub settings for kschultzBGOH/ArcGISRunner, set the default branch to main.
2. Check the 'Create main and make it the default branch' box in docs/TASKS.md (by whom: see issue 'Creating main: actor, box check, and later doc pushes').

Acceptance:

- GitHub shows main as the default branch of kschultzBGOH/ArcGISRunner.
- The Phase 0 'Create main' box in docs/TASKS.md is checked (by whom: see issue).

Issues: ISS-18, ISS-40, ISS-41, ISS-42

Sources: R443, R028, decision-log:O2, /home/user/ArcGISRunner/docs/TASKS.md:9

### W0.5 Cloud test harness: root package.json, TypeScript, Vitest, @arcgis/core 4.33

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.3

Add a root package.json, for cloud dev only, with TypeScript, Vitest and @arcgis/core 4.33, so that npm test runs the lib/ tests in cloud sessions where Developer Edition is not available.

Files: `/package.json`, `TBD in task`

Steps:

1. Branch from main.
2. Add /package.json (cloud dev only) with TypeScript, Vitest and @arcgis/core 4.33 as dependencies.
3. Make npm test run Vitest over the lib/ folders: widgets/arcgis-runner/src/runtime/lib/ and the crud lib/ folder (exact crud path is open; see issues).
4. Set TypeScript strict mode in the harness TypeScript config.
5. Limit the TypeScript and Vitest scope to lib/ folders so no file that imports jimu-* is compiled or tested in the cloud.
6. Run npm test in the cloud session and report the output in the pull request.
7. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist.
8. The brain reviews the pull request; the user runs the local checklist, reports back, and merges.

Acceptance:

- npm test runs Vitest over lib/ tests in a cloud session.
- /package.json lists TypeScript, Vitest and @arcgis/core 4.33.
- The harness TypeScript config sets strict mode.
- No `any` unless an SDK typing gap forces it, with a comment saying why.
- No file that imports jimu-* is compiled or tested by the harness.
- node_modules/ stays untracked (existing .gitignore).
- The user's local checklist reports whether the harness files change the Developer Edition 1.18 build result for the widget.

Local checklist (user runs):

- Check out the pull request branch locally.
- With widgets/arcgis-runner linked into the Developer Edition 1.18 checkout (mklink /J), run that checkout's npm start.
- Confirm the harness files add no new Developer Edition errors for arcgis-runner.
- Report the result in the pull request before merging.

Issues: ISS-01, ISS-03, ISS-12, ISS-43, ISS-44, ISS-45, ISS-46, ISS-47, ISS-53, ISS-143

Sources: R398, R399, R406, R408, R410, R411, R021, R047, R016, R017, R019, R012, R013, R014, R446, R447, CLAUDE.md:148, CLAUDE.md:159, CLAUDE.md:163, CLAUDE.md:185, CLAUDE.md:190-200, CLAUDE.md:206, /home/user/ArcGISRunner/docs/TASKS.md:10, /home/user/ArcGISRunner/.gitignore:1-2

### W0.6 Manifest at exbVersion 1.18.0; minimal widget builds in Developer Edition 1.18

**Repo:** widget · **Verification:** local checklist · **Depends on:** W0.3

Confirm manifest.json declares exbVersion 1.18.0 and that a minimal widget package, including its manifest.json, config.json and icon.svg, builds in the user's Developer Edition 1.18.

Files: `/widgets/arcgis-runner/manifest.json`, `/widgets/arcgis-runner/config.json`, `/widgets/arcgis-runner/icon.svg`, `TBD in task`

Steps:

1. Branch from main.
2. Confirm manifest.json declares exbVersion 1.18.0 (already present at manifest.json:4).
3. Confirm the package at widgets/arcgis-runner/ contains manifest.json, config.json and icon.svg, as the repo layout lists.
4. Make the package at widgets/arcgis-runner/ build in Developer Edition 1.18 as a minimal widget. How much of the stale source to change is open (see issues).
5. If config.json or src/config.ts changes, update docs/CONFIG_SCHEMA.md.
6. Write the local test checklist into the pull request: build in Developer Edition 1.18, what to click, what should happen.
7. Check the box in docs/TASKS.md and open a pull request into main.
8. The brain reviews the pull request; the user runs the local checklist, reports back, and merges.

Acceptance:

- manifest.json declares exbVersion 1.18.0.
- widgets/arcgis-runner/ contains manifest.json, config.json and icon.svg.
- The user's local checklist reports that arcgis-runner builds in Developer Edition 1.18 with no errors.

Local checklist (user runs):

- Link the widget into the Developer Edition 1.18 checkout: mklink /J <exb>\client\your-extensions\widgets\arcgis-runner <repo>\widgets\arcgis-runner
- Run that checkout's npm start.
- Confirm arcgis-runner compiles with no errors.
- Record which command produced client/dist/widgets/arcgis-runner/ and whether it exists (see issue 'Developer Edition build command not named').
- If client/dist/widgets/arcgis-runner/ exists, record whether it contains manifest.json and icon.svg.
- Report the results in the pull request before merging.

Issues: ISS-03, ISS-46, ISS-47, ISS-48, ISS-49, ISS-50, ISS-51, ISS-52, ISS-53, ISS-60, ISS-69

Sources: R401, R402, R403, R413, R421, R422, R426, R326, R327, R328, R444, R445, R111, R019, R024, R014, R446, R447, R438, CLAUDE.md:68-71, CLAUDE.md:75-76, CLAUDE.md:150-152, CLAUDE.md:167-169, CLAUDE.md:185, CLAUDE.md:209, /home/user/ArcGISRunner/README.md:11-16, /home/user/ArcGISRunner/docs/TASKS.md:11, /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:1-19

### W0.7 Spike — deployment: host, register in Portal 12.0, add to an experience, read props.context.folderUrl

**Repo:** widget · **Verification:** local checklist · **Depends on:** W0.6

Prove the widget build can be hosted on the builder app server (or any HTTPS server with CORS for the Portal origin), registered in Portal 12.0, added to an experience, and loaded there, and that it reads props.context.folderUrl, which the builder base URL is derived from.

Files: TBD in task

Steps:

1. Branch from main.
2. Add spike code that reads props.context.folderUrl in the runtime widget and shows the value so the checklist can record it.
3. Write the local test checklist into the pull request.
4. Check the box in docs/TASKS.md and open a pull request into main.
5. The brain reviews the pull request; the user runs the local checklist, reports back, and merges.
6. Record the result. If it shows the plan is wrong, the brain updates CLAUDE.md.

Acceptance:

- Portal 12.0 accepts the hosted manifest.json as an Experience Builder widget item.
- ArcGIS Runner can be added to an experience in Portal 12.0 and loads there.
- The widget reads props.context.folderUrl, and the checklist records the exact value so the derivation in CLAUDE.md:80-82 (folderUrl minus /widgets/arcgis-runner/) can be checked against it.
- The spike result is recorded.

Local checklist (user runs):

- Build the pull request branch in Developer Edition 1.18 (junction + npm start). Record which command produced client/dist/widgets/arcgis-runner/ and whether it exists (see issue 'Developer Edition build command not named').
- Copy client/dist/widgets/arcgis-runner/ to the HTTPS host: the builder app server's public/widgets/arcgis-runner/, or another HTTPS server.
- Add CORS headers for the Portal origin on that folder (on IIS, a web.config in the widget folder) (to verify; see issue 'CORS headers and Portal registration details unverified').
- Open the hosted manifest.json URL in a browser and confirm it is served.
- As a Portal admin in Portal 12.0, use Add Item -> Experience Builder widget with the hosted manifest.json URL (to verify; see issue 'CORS headers and Portal registration details unverified').
- In Portal's Experience Builder, create an experience and add ArcGIS Runner.
- Confirm ArcGIS Runner loads. Check the browser console for CORS or load errors.
- Record the folderUrl value the widget shows.
- Report the results in the pull request before merging.

Issues: ISS-03, ISS-48, ISS-52, ISS-53, ISS-54, ISS-55, ISS-56, ISS-57, ISS-58, ISS-59, ISS-60, ISS-73, ISS-208

Sources: R201, R202, R414, R345, R439, R325, R112, R113, R089, R111, R448, R074, R015, R014, R446, R447, decision-log:D12, /home/user/ArcGISRunner/docs/TASKS.md:12, CLAUDE.md:75-83, CLAUDE.md:185-186, CLAUDE.md:201-202, /home/user/arcgisbuilderwebapplication/CLAUDE.md:148-153

### W0.8 Spike — auth: read the Portal token from the Experience Builder session; load in a public experience

**Repo:** widget · **Verification:** local checklist · **Depends on:** W0.7

Prove the widget can read the signed-in user's Portal token from Experience Builder's session (to send later as Authorization: Bearer, per CLAUDE.md:84-86), and that it loads in a publicly shared experience without asking for sign-in.

Files: TBD in task

Steps:

1. Branch from main.
2. Add spike code that reads the signed-in user's Portal token from the Experience Builder session inside the widget (the API is not named in the sources; see issues).
3. Show whether a token was found. The widget must not ask for sign-in itself.
4. Write the local test checklist into the pull request.
5. Check the box in docs/TASKS.md and open a pull request into main.
6. The brain reviews the pull request; the user runs the local checklist, reports back, and merges.
7. Record the result, including which API returned the token. If it shows the plan is wrong, the brain updates CLAUDE.md.

Acceptance:

- Signed in to Portal 12.0, the widget reads the user's Portal token from the Experience Builder session.
- In an experience shared publicly and opened signed out, the widget loads, finds no token, and does not ask for sign-in.
- The result records which API returned the token.

Local checklist (user runs):

- Build and deploy the pull request branch as in W0.7 (build, copy to the HTTPS host, CORS).
- Sign in to Portal 12.0 and open an experience that contains ArcGIS Runner. Confirm the widget reports that it found a token.
- Share an experience that contains ArcGIS Runner publicly.
- Record the sharing level of each Portal item used (experience, widget item, and any webmap and layers) (see issue 'Public sharing needed for the anonymous check').
- Open it in a private browser window while signed out. Confirm the widget loads, reports no token, and shows no sign-in prompt of its own.
- Report the results in the pull request before merging.

Issues: ISS-03, ISS-53, ISS-58, ISS-59, ISS-70, ISS-71, ISS-122

Sources: R415, R416, R092, R348, R090, R091, R448, R015, R014, R446, R447, decision-log:D18, /home/user/ArcGISRunner/docs/TASKS.md:13, CLAUDE.md:84-88, CLAUDE.md:185, /home/user/arcgisbuilderwebapplication/CLAUDE.md:130-133

### W0.9 Spike — URL hash: read/write #runner= alongside Experience Builder's hash parameters

**Repo:** widget · **Verification:** local checklist · **Depends on:** W0.6

Find out whether the widget can read and write its own #runner= parameter alongside Experience Builder's hash parameters without either side clobbering the other or the page reloading, including on page switches and browser back/forward.

Files: TBD in task

Steps:

1. Branch from main.
2. If the spike runs in Portal, it also depends on W0.7's hosting and registration (where it runs is open; see issue 'URL hash spike: wanted behaviour and test location not stated').
3. Add spike code that writes a sample #runner= value in the documented format (#runner={profileId}:{layerId}:{view|edit}:{featureKey}) and keeps Experience Builder's own hash parameters.
4. Add spike code that reads #runner= from the URL and shows the value.
5. Write the local test checklist into the pull request.
6. Check the box in docs/TASKS.md and open a pull request into main.
7. The brain reviews the pull request; the user runs the local checklist, reports back, and merges.
8. Record the answers. If they show the plan is wrong, the brain updates CLAUDE.md.

Acceptance:

- The result says whether writing #runner= leaves Experience Builder's hash parameters intact.
- The result says whether Experience Builder's own hash updates leave #runner= intact.
- The result says whether writing #runner= reloads the page.
- The result records what happens to #runner= on Experience Builder page switches.
- The result records what happens on browser back and forward.
- The result says whether the widget reads #runner= from a URL it is opened with.

Local checklist (user runs):

- Build the pull request branch in Developer Edition 1.18. Deploy it as in W0.7 if the spike runs in Portal (where it runs is open; see issues).
- Open an experience that has ArcGIS Runner and at least two pages. Note the Experience Builder hash parameters in the URL.
- Trigger the widget's hash write. Confirm whether the Experience Builder parameters are still there and whether the page reloaded.
- Switch Experience Builder pages. Record whether #runner= is still in the URL.
- Use browser back and forward. Record what happens to #runner= and to the page.
- Open the URL that contains #runner= in a new tab. Confirm whether the widget reads the value.
- Report the results in the pull request before merging.

Issues: ISS-03, ISS-53, ISS-58, ISS-59, ISS-72, ISS-73, ISS-74

Sources: R417, R418, R376, R377, R333, R448, R015, R014, R446, R447, decision-log:D21, /home/user/ArcGISRunner/docs/TASKS.md:14, CLAUDE.md:124-126, CLAUDE.md:185

### W0.10 Spike — CSP: can handler code compiled with new Function run in a Portal-hosted experience?

**Repo:** widget · **Verification:** local checklist · **Depends on:** W0.7

Find out whether handler code compiled from text with new Function runs in a Portal-hosted experience under Experience Builder's Content-Security-Policy. The result decides the widget JS handler runner and feeds builder app Phase 4.

Files: TBD in task

Steps:

1. Branch from main.
2. Add spike code that compiles a handler function body from text with new Function and calls it with a ctx argument, the way handlers are called as (ctx) => { ... }.
3. Show whether the handler ran, or the error if it failed.
4. Write the local test checklist into the pull request.
5. Check the box in docs/TASKS.md and open a pull request into main.
6. The brain reviews the pull request; the user runs the local checklist, reports back, and merges.
7. The brain records the result in both backlogs (widget Phase 1 JS handler runner, builder Phase 4 JS handler editor).
8. If the handler is blocked, the brain puts the documented fallbacks to the user, who decides: (a) a fixed set of built-in no-code actions, or (b) self-hosted experiences downloaded from Developer Edition.

Acceptance:

- In a Portal-hosted experience, the result shows whether a handler compiled with new Function ran or was blocked by the Content-Security-Policy.
- The result records which Portal context or contexts were tested.
- The result is recorded where widget Phase 1 (JS handler runner) and builder Phase 4 (JS handler editor) can use it.

Local checklist (user runs):

- Build and deploy the pull request branch as in W0.7 (build, copy to the HTTPS host, CORS).
- Open a Portal-hosted experience that contains ArcGIS Runner. Record which context it is (Experience Builder preview, published view, or anonymous public view; see issue 'CSP may differ by Portal context').
- Trigger the spike handler. Record whether it ran.
- Check the browser console for a Content-Security-Policy violation and copy its text if there is one.
- Report the results in the pull request before merging.

Issues: ISS-03, ISS-20, ISS-53, ISS-58, ISS-75, ISS-76, ISS-77

Sources: R140, R141, R056, R142, R143, R359, R397, R137, R088, R448, R014, R446, R447, decision-log:O3, /home/user/ArcGISRunner/docs/TASKS.md:15, /home/user/ArcGISRunner/docs/TASKS.md:24, CLAUDE.md:105-107, CLAUDE.md:185, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:114

## W1 — Widget Phase 1 — Shell

W1 replaces the stale per-layer scaffold with the shell that every kind shares. It sets the widget config to { profileId, builderBaseUrl? }, adds a settings panel that lists profiles built for the connected map's webmap, and makes the runtime fetch and check that profile, lazy-load its kind, and inject its custom CSS. The JS handler runner is built only if the W0.10 CSP spike passes.

### W1.1 Widget config shape and builder base URL

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.3, W0.4, W0.5, W0.6

Replace the stale per-layer config with { profileId, builderBaseUrl? }. Derive the builder app address from the widget's hosting folder (props.context.folderUrl minus /widgets/arcgis-runner/), with builderBaseUrl as an override for local Developer Edition work only.

Files: `widgets/arcgis-runner/src/config.ts`, `widgets/arcgis-runner/config.json`, `widgets/arcgis-runner/src/runtime/lib/`, `docs/CONFIG_SCHEMA.md`, `docs/TASKS.md`

Steps:

1. Branch from main and read CLAUDE.md.
2. In src/config.ts, replace the stale RunnerLayerConfig and Config (useMapWidgetIds, layers, defaultPageSize) with Config { profileId: string; builderBaseUrl?: string }.
3. Replace the stale contents of config.json (useMapWidgetIds, layers, defaultPageSize) with the new shape.
4. Add a function under src/runtime/lib/ with no jimu-* imports that returns builderBaseUrl when it is set, and otherwise returns folderUrl minus /widgets/arcgis-runner/.
5. Add Vitest tests for that function covering override set and override absent.
6. Confirm docs/CONFIG_SCHEMA.md matches the new src/config.ts; update it in the same change if it does not.
7. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- src/config.ts defines only profileId and the optional builderBaseUrl.
- config.json no longer contains useMapWidgetIds, layers or defaultPageSize.
- With builderBaseUrl set, the base URL equals builderBaseUrl.
- Without builderBaseUrl, the base URL equals props.context.folderUrl minus /widgets/arcgis-runner/.
- App authors are never asked to type a URL.
- The base URL function has no jimu-* imports, and its tests pass with npm test in the cloud.
- docs/CONFIG_SCHEMA.md matches src/config.ts.
- TypeScript strict; no any unless an SDK typing gap forces it, with a comment saying why.
- The widget builds in Developer Edition 1.18.

Local checklist (user runs):

- If not already linked, link the widget into the Developer Edition 1.18 checkout with mklink /J <exb>\client\your-extensions\widgets\arcgis-runner <repo>\widgets\arcgis-runner, then run npm start in that checkout.
- Confirm the widget builds in Developer Edition 1.18 with no TypeScript errors.
- Add Runner to an experience in Developer Edition and confirm it loads.
- Report the result in the pull request before merging.

Issues: ISS-01, ISS-02, ISS-06, ISS-12, ISS-48, ISS-49, ISS-57, ISS-144, ISS-145, ISS-146, ISS-149

Sources: R343, R344, R345, R346, R341, R342, R347, R024, R021, R013, R410, R414, R427, R428, R429, /home/user/ArcGISRunner/docs/TASKS.md:19, CLAUDE.md:80-83, CLAUDE.md:154, CLAUDE.md:184, CLAUDE.md:206, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:7-12

### W1.2 Settings panel: map selector and profile dropdown

**Repo:** widget · **Verification:** local checklist · **Depends on:** W1.1, W0.7, W0.8, B3.2, B3.5

Let the app author connect a Map widget and pick a profile from a dropdown loaded from GET {base}/api/runtime/profiles?webmapId=, which lists only profiles built for the connected map's webmap.

Files: `widgets/arcgis-runner/src/setting/setting.tsx`, `docs/TASKS.md`

Steps:

1. Branch from main and read CLAUDE.md.
2. Keep a Map widget selector in src/setting/setting.tsx for one connected Map widget (multiple Map widgets are out of scope).
3. Read the webmap item id of the connected Map widget.
4. Call GET {base}/api/runtime/profiles?webmapId={webmapId}, using the base URL from W1.1.
5. Send the author's Portal token from the Experience Builder session as Authorization: Bearer when signed in, as established by the W0.8 auth spike.
6. Show the returned profiles in a dropdown built from Calcite Components (via jimu-ui where wrapped).
7. When the author picks a profile, store its id as profileId in the widget config.
8. Remove the stale per-layer placeholder text and comment from setting.tsx. Add no field, layer or URL inputs.
9. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- The settings panel shows a Map widget selector and a profile dropdown, and no field or layer configuration.
- The dropdown request goes to {base}/api/runtime/profiles?webmapId={connected webmap id}.
- The dropdown lists only profiles built for the connected map's webmap.
- Choosing a profile stores its id as profileId in the widget config.
- The request carries Authorization: Bearer with the Portal token when the author is signed in.
- The settings panel has no URL input.
- The widget never asks for sign-in itself.
- jimu-* components only wire lib/ logic to Experience Builder.
- TypeScript strict; no any unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Build in Developer Edition 1.18 (junction from the README, then npm start).
- Set builderBaseUrl in config.json to the builder app, add Runner to an experience, and open its settings.
- Connect a Map widget whose webmap has at least one published profile.
- Confirm the dropdown lists only profiles built for that webmap.
- Pick a profile, close the settings panel, reopen it, and confirm the same profile is still selected.
- In the browser network tab, confirm the request goes to {base}/api/runtime/profiles?webmapId={webmap id} and carries Authorization: Bearer while signed in.
- Deferred until W0.7 (deployment spike) and B3.7 (builder widget deploy script) land: with the build deployed to the builder app and registered in Portal, repeat in Portal's Experience Builder without builderBaseUrl and confirm the request goes to the derived base URL.
- Report the results in the pull request before merging.

Issues: ISS-09, ISS-12, ISS-48, ISS-49, ISS-57, ISS-70, ISS-126, ISS-127, ISS-131, ISS-147, ISS-148, ISS-149, ISS-150, ISS-151, ISS-152, ISS-153, ISS-154, ISS-155

Sources: R336, R337, R338, R339, R340, R123, R238, R092, R348, R331, R335, R090, R094, R237, R329, R412, R415, R021, R013, R434, R435, R436, /home/user/ArcGISRunner/docs/TASKS.md:12-13, /home/user/ArcGISRunner/docs/TASKS.md:20, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, CLAUDE.md:13-14, CLAUDE.md:84-91, CLAUDE.md:155, CLAUDE.md:197, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:14-15

### W1.3 Runtime shell: fetch and check the profile

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W1.1, W1.2, W0.4, W0.7, W0.8, B3.5

At runtime, connect to the Map widget, fetch the chosen profile from GET {base}/api/runtime/profiles/{profileId}, and check schemaVersion, kind and webmap. Show clear errors for an unknown version or kind, an unreachable server, or a webmap mismatch.

Files: `widgets/arcgis-runner/src/runtime/widget.tsx`, `widgets/arcgis-runner/src/runtime/shell/`, `widgets/arcgis-runner/src/runtime/lib/`, `docs/TASKS.md`

Steps:

1. Branch from main and read CLAUDE.md.
2. Make src/runtime/widget.tsx mount the shell in src/runtime/shell/. Remove the placeholder text and the comment that points at the old phase numbers.
3. In the shell, connect to the Map widget with JimuMapViewComponent (shell step 1).
4. Fetch GET {base}/api/runtime/profiles/{profileId}, using the base URL from W1.1 and profileId from the widget config.
5. Send the user's Portal token from the Experience Builder session as Authorization: Bearer when signed in, as established by the W0.8 auth spike, and no Authorization header when anonymous.
6. Add profile checks under src/runtime/lib/ with no jimu-* imports: schemaVersion is the version the widget supports (v1), kind is one the widget knows, and the profile's webmapId equals the connected map's webmap item id.
7. Show a clear error for an unknown schemaVersion, an unknown kind ('update the Runner widget'), an unreachable server, and a webmap mismatch.
8. If an action needs sign-in, show the service's message; never prompt for sign-in.
9. Keep shell state (profile, errors, map view) in local React state; no Redux.
10. Add Vitest tests for the profile checks.
11. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- src/runtime/widget.tsx mounts the shell, and the placeholder text ('Connect a Map widget to get started.', 'Map connected. Layer detection not implemented yet.') is gone.
- The shell connects to the Map widget through JimuMapViewComponent.
- The shell requests {base}/api/runtime/profiles/{profileId}, with Authorization: Bearer when signed in and no Authorization header when anonymous.
- A profile with an unknown schemaVersion shows a clear error.
- A profile with an unknown kind shows 'update the Runner widget' instead of guessing.
- An unreachable builder app shows a clear error.
- A profile whose webmapId differs from the connected map's webmap shows a clear error.
- The profile checks live in src/runtime/lib/ with no jimu-* imports and pass Vitest in the cloud.
- The widget renders only the profile set in its config, and end users have no way to switch profiles.
- The widget never asks for sign-in itself.
- When a request needs sign-in, the widget shows the service's message.
- Shell state is local React state; no Redux.
- jimu-* components only wire lib/ logic to Experience Builder.
- TypeScript strict; no any unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Build in Developer Edition 1.18 (junction from the README, then npm start).
- Open an experience where Runner's Map widget shows the profile's webmap, and confirm no error shows.
- Point the Map widget at a different webmap and confirm the webmap mismatch error.
- Stop the builder app, or set builderBaseUrl to an address where nothing is listening, and confirm the unreachable-server error.
- While signed in, confirm in the network tab that the profile request carries Authorization: Bearer.
- Deferred until W0.7 (deployment spike) and B3.7 (builder widget deploy script) land: open a publicly shared, Portal-hosted experience anonymously and confirm the profile request has no Authorization header and the profile loads when its webmap is public.
- Confirm the widget never shows a sign-in prompt of its own.
- Report the results in the pull request before merging.

Issues: ISS-09, ISS-12, ISS-48, ISS-49, ISS-57, ISS-68, ISS-70, ISS-127, ISS-129, ISS-131, ISS-148, ISS-149, ISS-150, ISS-154, ISS-155, ISS-156, ISS-157, ISS-158, ISS-159

Sources: R350, R351, R352, R353, R354, R355, R356, R124, R146, R148, R149, R092, R334, R348, R349, R330, R404, R405, R406, R410, R412, R415, R416, R021, R013, R144, R430, R431, R433, /home/user/ArcGISRunner/docs/TASKS.md:12-13, /home/user/ArcGISRunner/docs/TASKS.md:21, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, CLAUDE.md:73-74, CLAUDE.md:84-91, CLAUDE.md:95-98, CLAUDE.md:157-159, CLAUDE.md:197, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:16, /home/user/arcgisbuilderwebapplication/CLAUDE.md:38-40

### W1.4 Kind registry with lazy-loaded modules

**Repo:** widget · **Verification:** local checklist · **Depends on:** W1.3

Add src/runtime/kinds/registry.ts, which maps each kind key to a lazy module, so the shell downloads only the kind the profile uses.

Files: `widgets/arcgis-runner/src/runtime/kinds/registry.ts`, `widgets/arcgis-runner/src/runtime/kinds/crud/`, `widgets/arcgis-runner/src/runtime/shell/`, `docs/TASKS.md`

Steps:

1. Branch from main and read CLAUDE.md.
2. Create src/runtime/kinds/registry.ts mapping the crud key to a lazy module in src/runtime/kinds/crud/.
3. Make the shell lazy-load the module for the fetched profile's kind (shell step 5).
4. Show 'update the Runner widget' for a kind missing from the registry.
5. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- registry.ts maps the crud key to a lazy module in src/runtime/kinds/crud/.
- The shell loads a kind module only for the loaded profile's kind, so only the kind in use is downloaded.
- A kind missing from the registry shows 'update the Runner widget'.
- TypeScript strict; no any unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Build in Developer Edition 1.18 (junction from the README, then npm start).
- Open an experience whose Runner uses a crud profile.
- In the browser network tab, confirm the crud kind module downloads as its own file, after the profile request.
- Report the results in the pull request before merging.

Issues: ISS-12, ISS-48, ISS-159, ISS-160

Sources: R360, R361, R117, R356, R096, R023, R071, R072, R021, R013, /home/user/ArcGISRunner/docs/TASKS.md:22, CLAUDE.md:108, CLAUDE.md:161-162, CLAUDE.md:184, CLAUDE.md:206, /home/user/arcgisbuilderwebapplication/CLAUDE.md:33-34

### W1.5 Custom CSS injection and widget root attributes

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W1.3, W0.4, B3.1, B3.2

Inject the profile's customCss unscoped into the page <head> as one <style data-runner-profile="{id}"> tag per profile, kept on unmount. Give the widget root class="arcgis-runner" and data-profile="{id}".

Files: `widgets/arcgis-runner/src/runtime/lib/`, `widgets/arcgis-runner/src/runtime/shell/`, `widgets/arcgis-runner/src/runtime/widget.tsx`, `docs/TASKS.md`

Steps:

1. Branch from main and read CLAUDE.md.
2. Add a function under src/runtime/lib/ with no jimu-* imports that, given a profile id and its customCss, looks for <style data-runner-profile="{id}"> in document.head, adds it when missing, and never adds a second tag for the same id.
3. Call it from the shell after the profile loads (shell step 3). Do not remove the tag on unmount.
4. Give the widget root class="arcgis-runner" and data-profile="{id}" in place of the stale widget-arcgis-runner p-2 class.
5. Add Vitest tests: one tag per profile, no duplicate on a repeat call for the same id, separate tags for two profiles.
6. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- After the profile loads, <head> holds one <style data-runner-profile="{id}"> tag with the profile's customCss, unscoped.
- Loading the same profile again reuses the tag and adds no duplicate.
- The tag stays after the widget unmounts and remains until the page reloads.
- customCss rules style the whole experience; rules starting with .arcgis-runner or [data-profile="{id}"] style only the widget.
- The widget root has class="arcgis-runner" and data-profile="{id}".
- The injection logic lives in src/runtime/lib/ with no jimu-* imports and passes Vitest in the cloud.
- jimu-* components only wire lib/ logic to Experience Builder.
- TypeScript strict; no any unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Build in Developer Edition 1.18 (junction from the README, then npm start).
- Use a profile whose customCss has one rule starting with .arcgis-runner and one rule for another part of the experience.
- Open the experience and confirm in the Elements panel that <head> holds one <style data-runner-profile="{id}"> tag with the profile's CSS.
- Confirm the widget root has class="arcgis-runner" and data-profile="{id}".
- Confirm the second rule styles the other part of the experience.
- Switch to an experience page without Runner and back, and confirm the style tag is still there and not duplicated.
- Report the results in the pull request before merging.

Issues: ISS-12, ISS-48, ISS-49, ISS-148, ISS-161, ISS-162, ISS-163, ISS-164

Sources: R357, R358, R120, R121, R122, R118, R410, R412, R021, R013, R432, /home/user/ArcGISRunner/docs/TASKS.md:23, CLAUDE.md:99-104, CLAUDE.md:184, CLAUDE.md:197, CLAUDE.md:206, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:23, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:28-30, /home/user/arcgisbuilderwebapplication/CLAUDE.md:52-54

### W1.6 JS handler runner (only if the CSP spike passed)

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.10, W0.4, W1.3

If the CSP spike shows that handler code compiled from text runs in a Portal-hosted experience, add the shell's JS handler runner. It compiles handler function bodies from profile text and calls them with a kind-defined ctx.

Files: `widgets/arcgis-runner/src/runtime/shell/`, `widgets/arcgis-runner/src/runtime/lib/`, `docs/TASKS.md`

Steps:

1. Check the W0.10 CSP spike result first. If handlers compiled from text are blocked, do not build this task; return it to the brain, where the user chooses between the documented fallbacks.
2. Branch from main and read CLAUDE.md.
3. Add the runner: compile a handler function body from profile text (new Function, as tested in the spike) into a function called as (ctx) => { ... }.
4. Call the compiled handler with the ctx the calling kind supplies. The runner defines no events; each kind defines its own event set.
5. Add Vitest tests that compile a body and call it with a stub ctx.
6. Check the box in docs/TASKS.md and open a pull request into main that ends with the local test checklist. Never push to main directly.

Acceptance:

- The task is built only after the W0.10 CSP spike reports that handlers compiled from text run in a Portal-hosted experience.
- The runner turns a function-body string into a handler called as (ctx) => { ... }.
- The handler receives the ctx the kind supplies.
- The runner lives in the shell and is kind-agnostic; crud events are fired later, in W3.6.
- Vitest tests that compile a body and call it with a stub ctx pass in the cloud.
- TypeScript strict; no any unless an SDK typing gap forces it, with a comment saying why.

Local checklist (user runs):

- Build in Developer Edition 1.18 (junction from the README, then npm start) and confirm the widget builds with no TypeScript errors.
- Confirm an experience with Runner still loads as before.
- Report the results in the pull request before merging.

Issues: ISS-11, ISS-20, ISS-48, ISS-75, ISS-76, ISS-77, ISS-165, ISS-166

Sources: R359, R140, R141, R142, R143, R056, R088, R137, R138, R139, R166, R185, R186, R397, R019, R021, R013, /home/user/ArcGISRunner/docs/TASKS.md:15, /home/user/ArcGISRunner/docs/TASKS.md:24, /home/user/ArcGISRunner/docs/TASKS.md:42, CLAUDE.md:105-107, CLAUDE.md:198-199, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:114, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47

## W2 — Widget Phase 2 — crud: List & View

W2 builds the read side of the `crud` kind. It matches profile layers to the connected map through the Map widget's data sources, then adds a layer picker and List and View screens that live inside the widget. It also adds two-way map selection (List rows highlight and zoom, map clicks open View) and `#runner=` record links with a Copy link button. The backlog puts the map-click Add/Edit rule, the unsaved-changes prompt and record links (including `edit` links) in Phase 2, so W2.5 and W2.6 own that logic outright, and W3.2's Add and Edit screens plug into it. Logic goes in jimu-free `lib/` modules tested with Vitest in the cloud. Each task branches from `main`, never pushes to `main`, and opens a pull request that ends with a local checklist. The user runs the checklist in Developer Edition 1.18 and reports back before merging. Checks that need real Portal sharing are marked Portal-hosted and depend on the W0 deployment and auth spikes and the builder widget deploy script. Record links also depend on the W0 URL hash spike.

### W2.1 Layer matching against the Map widget's layer data sources

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.4, W1.2, W1.3, W1.4, B3.2, B3.5

Match each profile layer in `settings.layers` to a layer or table in the connected map by `layerId`, falling back to `url`. Reuse the data sources the Map widget already created for each layer, so selection syncs with the map and other widgets and the author never picks data sources.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/ (path ambiguous, CLAUDE.md:163)`, `docs/TASKS.md`

Steps:

1. Branch from `main`. Never push to `main`.
2. In the crud `lib/` folder, write a jimu-free matcher. Its inputs are the profile's `LayerConfig[]` and the connected map's layers and tables. It matches each `LayerConfig` by `layerId` (the operational layer or table id in the webmap) and tries `url` only when `layerId` finds nothing.
3. Write Vitest tests for three cases: a match by `layerId`, a fallback match by `url` when `layerId` finds nothing, and a layer that matches neither, which the matcher reports as unmatched. What Runner then does with an unmatched layer is an open question (see issues).
4. In the crud kind module, read the connected map's layers and tables and the data sources the Map widget created for them (its layer views), and pass them to the matcher. Add no data source picker for the author.
5. Keep each matched layer's data source with it so the List, map-click and record-link tasks can select through it.
6. Keep the `jimu-*` code thin. The matching rules stay in `lib/`.
7. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. The user runs it and reports back before merging.

Acceptance:

- `npm test` runs the matcher tests and they pass.
- A profile layer whose `layerId` is in the connected map matches that layer.
- A profile layer whose `layerId` isn't found but whose `url` is found matches by `url`.
- A profile layer that matches neither is reported as unmatched.
- Runner uses the Map widget's existing data sources. The app author doesn't pick any.
- The widget builds in Developer Edition 1.18 with no TypeScript errors.

Local checklist (user runs):

- Link `widgets/arcgis-runner` into the Developer Edition 1.18 checkout with `mklink /J` if it isn't linked yet, then run that checkout's `npm start`.
- Confirm the build finishes with no TypeScript errors.
- Set `builderBaseUrl` in the widget's `config.json` to the builder app address (local Developer Edition work only).
- Add a Map widget and Runner to an experience. Connect the map and pick a published `crud` profile for that map's webmap.
- Preview the experience and confirm Runner loads the profile with no errors in the browser console. The matched layers first appear on screen in W2.2's layer picker.

Issues: ISS-01, ISS-02, ISS-06, ISS-13, ISS-45, ISS-100, ISS-131, ISS-167, ISS-168, ISS-169, ISS-170

Sources: R362, R363, R154, R156, R157, R406, R408, R410, R412, R017, R012, R013, R019, R447, R002, R336, R123, R124, CLAUDE.md:21, CLAUDE.md:113-116, CLAUDE.md:182-184, CLAUDE.md:193-200, /home/user/ArcGISRunner/docs/TASKS.md:20, /home/user/ArcGISRunner/docs/TASKS.md:28, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:39, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:45-46

### W2.2 Layer picker and in-widget screen navigation

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W2.1

Show a LayerPicker that lists the matched profile layers in `settings.layers` order. Move between the `crud` screens inside the widget, not through Experience Builder pages.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/LayerPicker`, `widgets/arcgis-runner/src/runtime/kinds/crud/`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/ (path ambiguous, CLAUDE.md:163)`, `docs/TASKS.md`

Steps:

1. Branch from `main`. Never push to `main`.
2. In crud `lib/`, order the matched layers by their position in `settings.layers`. Add a Vitest test for the order.
3. Build LayerPicker with Calcite Components (through `jimu-ui` where it wraps them). What each entry shows is an open question (see issues).
4. Hold the current layer and the current screen in local React state. Don't use Redux.
5. When the user chooses a layer, show that layer's screens inside the widget.
6. In this phase, navigation covers List and View. The Add and Edit screens join the same navigation in W3.
7. Don't use Experience Builder pages for screens.
8. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. The user runs it and reports back before merging.

Acceptance:

- The picker lists the profile's layers in `settings.layers` order.
- Choosing a layer changes what Runner shows, and the Experience Builder page doesn't change.
- Screen state lives in local React state.
- `npm test` passes.

Local checklist (user runs):

- Build the widget in Developer Edition 1.18 (junction plus `npm start`) and confirm there are no TypeScript errors.
- Open an experience with a Map widget and Runner, set to a `crud` profile that has several layers.
- Confirm the picker lists the layers in the same order as the profile.
- Pick each layer in turn. Confirm Runner's content changes and the Experience Builder page stays the same.

Issues: ISS-101, ISS-103, ISS-154, ISS-157, ISS-168, ISS-171, ISS-172, ISS-173

Sources: R364, R365, R154, R329, R330, R407, R012, R013, R447, CLAUDE.md:72-74, CLAUDE.md:117-118, CLAUDE.md:182-184, CLAUDE.md:199-200, /home/user/ArcGISRunner/docs/TASKS.md:29, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:39-40

### W2.3 List screen: columns, labels, sort, page size, pagination, row selection

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W2.2, W0.6, W0.7, B3.7

Show a List screen for each layer, using the profile's columns, labels, sort and page size, with pagination, and read the rows straight from the feature service. Selecting a row highlights the feature on the map and zooms to it.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/ListScreen`, `widgets/arcgis-runner/src/runtime/kinds/crud/mapSync.ts`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/ (path ambiguous, CLAUDE.md:163)`, `docs/TASKS.md`

Steps:

1. Branch from `main`. Never push to `main`.
2. In crud `lib/`, turn `layouts.list` into query options. Fields come from `columns`. Order comes from `sortField` and `sortOrder` when they're set. Page size comes from `pageSize`, and the query also takes the current page. Add Vitest tests.
3. Have ListScreen query the matched layer with `queryFeatures` straight to the feature service. List data never goes through the builder app.
4. Show one column per entry in `columns`, in that order. Head each column with that field's `label` from `fields`.
5. Add pagination controls (Calcite via `jimu-ui` where wrapped; see issues) that move through the rows `pageSize` at a time.
6. In mapSync.ts, when the user selects a row, select the feature through the Map widget's data source for that layer so the map highlights it, then zoom the map to it.
7. If a query fails because it needs sign-in, show the service's message. Don't prompt for sign-in.
8. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. The user runs it and reports back before merging.

Acceptance:

- The List shows exactly the `columns` fields, in order, with `label` headers.
- When `sortField` is set, rows are ordered on that field in `sortOrder` order.
- A page shows at most `pageSize` rows, and pagination moves between pages.
- Selecting a row highlights the feature on the map and zooms to it.
- Selection syncs with the map and other widgets through the Map widget's data source.
- The List works for point, multipoint, polyline and polygon layers and for tables.
- List reads go to the feature service directly, not to the builder app.
- A read that needs sign-in shows the service's message, and Runner doesn't ask for sign-in itself.
- `npm test` passes.

Local checklist (user runs):

- Build the widget in Developer Edition 1.18 and confirm there are no TypeScript errors.
- Open the experience and pick a layer.
- Check that the columns and headers match the profile's `layouts.list.columns` and the field labels.
- On a layer with `sortField` set, confirm the row order. Page forward and back, and confirm each page shows at most `pageSize` rows.
- Select a row and confirm the map highlights the feature and zooms to it.
- Repeat for each geometry type in the webmap and for a table.
- In the browser dev tools Network tab, confirm List queries go to the feature service URL, not the builder app.
- Portal-hosted check (needs the widget deployed from the builder app and registered in Portal 12.0): open a publicly shared experience while signed out. Confirm the List loads for a publicly shared layer, and that a layer that isn't public shows the service's message.

Issues: ISS-09, ISS-154, ISS-155, ISS-167, ISS-170, ISS-171, ISS-173, ISS-174, ISS-175, ISS-176, ISS-177, ISS-178, ISS-179

Sources: R366, R367, R332, R363, R085, R178, R179, R180, R181, R170, R090, R348, R349, R108, R329, R407, R012, R013, R447, R448, R201, R202, R416, R111, CLAUDE.md:45, CLAUDE.md:72-73, CLAUDE.md:84-88, CLAUDE.md:114-116, CLAUDE.md:135, CLAUDE.md:182-184, CLAUDE.md:199-202, /home/user/ArcGISRunner/docs/TASKS.md:12-13, /home/user/ArcGISRunner/docs/TASKS.md:30, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:85-90, /home/user/arcgisbuilderwebapplication/CLAUDE.md:160

### W2.4 View screen: read-only sections

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W2.2, W0.6, W0.7, B3.7

Show a View screen that displays one feature read-only, in the titled sections and field order of `layouts.view`.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/ViewScreen`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/ (path ambiguous, CLAUDE.md:163)`, `docs/TASKS.md`

Steps:

1. Branch from `main`. Never push to `main`.
2. In crud `lib/`, build the View content from `layouts.view.sections` and `fields`. Each section gets its `title` and its fields in order, and each field gets its `label`. Add Vitest tests.
3. Have ViewScreen read the feature with `queryFeatures` straight to the feature service.
4. Render the sections read-only, with no inputs.
5. If the read needs sign-in, show the service's message. Don't prompt for sign-in.
6. Leave out the Delete action and the Copy link button. Delete comes in W3, and Copy link comes in W2.6.
7. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. The user runs it and reports back before merging.

Acceptance:

- View shows each `layouts.view` section title, followed by that section's fields in order, with their labels and values.
- No field on View can be edited.
- View works for features of every geometry type and for table records.
- A read that needs sign-in shows the service's message, and Runner doesn't ask for sign-in itself.
- `npm test` passes.

Local checklist (user runs):

- Build the widget in Developer Edition 1.18 and confirm there are no TypeScript errors.
- Open a feature's View screen. This needs a path into View (see issues).
- Check the section titles, field order and labels against the profile's `layouts.view`.
- Confirm no field can be edited.
- Repeat for each geometry type and for a table record.
- Portal-hosted check (needs the widget deployed from the builder app and registered in Portal 12.0): open View signed out on a feature of a public layer and on a feature of a non-public layer. Confirm the feature loads on the public layer and the service's message appears on the non-public one.

Issues: ISS-155, ISS-171, ISS-174, ISS-175, ISS-177, ISS-180

Sources: R368, R182, R170, R076, R085, R090, R348, R349, R108, R407, R012, R013, R447, R448, R416, CLAUDE.md:44, CLAUDE.md:84-88, CLAUDE.md:117-118, CLAUDE.md:135, CLAUDE.md:182-184, CLAUDE.md:199-202, /home/user/ArcGISRunner/docs/TASKS.md:12-13, /home/user/ArcGISRunner/docs/TASKS.md:31, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:55-60, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:92-94, decision-log:D18

### W2.5 Map click opens View

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W2.1, W2.3, W2.4

Clicking a feature of a profile layer on the map opens it in Runner's View screen. Several hits show a short pick list. This task owns two navigation rules from the backlog: map clicks don't navigate on Add/Edit, and leaving a form with unsaved changes asks first. W3.2's Add and Edit screens plug into both rules rather than building them.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/mapSync.ts`, `widgets/arcgis-runner/src/runtime/kinds/crud/`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/ (path ambiguous, CLAUDE.md:163)`, `docs/TASKS.md`

Steps:

1. Branch from `main`. Never push to `main`.
2. In crud `lib/`, write the click decision. Keep only hits on matched profile layers. No hits means do nothing, one hit opens View, and several hits show a pick list. If the current screen is Add or Edit, don't navigate. Add a Vitest test for each case.
3. In crud `lib/`, write the leave decision. Leaving a screen that has unsaved changes asks first. Leaving a screen without them goes straight through. Add Vitest tests for both cases.
4. Route every screen change in W2.2's in-widget navigation through the leave decision, including changes caused by a map click. A screen tells navigation whether it has unsaved changes. List and View never do; W3.2's Add and Edit forms will.
5. In mapSync.ts, listen for clicks on the connected map view and run `hitTest`, limited to profile layers.
6. Have the click handler read the current screen from navigation and apply the click decision, so the Add/Edit rule works as soon as W3.2's screens join navigation as Add and Edit.
7. For one hit, open that feature's View screen for its layer.
8. For several hits, show a short pick list. Choosing an entry opens that feature's View.
9. Don't change the map's popup.
10. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. The user runs it and reports back before merging.

Acceptance:

- Clicking a feature of a profile layer opens it in View.
- Clicking where several profile-layer features overlap shows a pick list, and picking one opens its View.
- Clicking a feature of a layer that isn't in the profile doesn't change Runner.
- The map's popup behaves as the webmap or Map widget configures it.
- Vitest tests cover the click decision, including no navigation while the current screen is Add or Edit.
- Vitest tests cover the leave decision: unsaved changes ask first, no unsaved changes go straight through.
- Every screen change in navigation goes through the leave decision.
- `npm test` passes.

Local checklist (user runs):

- Build the widget in Developer Edition 1.18 and confirm there are no TypeScript errors.
- Open the experience and click a feature of a profile layer. Confirm Runner opens its View.
- Click where profile-layer features overlap. Confirm a pick list appears, then pick an entry and confirm its View opens.
- Click a feature of a layer that isn't in the profile. Confirm Runner doesn't change.
- Confirm the map popup still appears, or doesn't, as the webmap or Map widget sets it.
- The Add/Edit click rule and the unsaved-changes prompt can't be clicked through until W3.2's forms exist. Confirm their Vitest tests pass here (see issues).

Issues: ISS-13, ISS-170, ISS-171, ISS-179, ISS-181, ISS-182, ISS-183, ISS-184

Sources: R371, R372, R373, R374, R375, R332, R365, R330, R012, R013, R447, CLAUDE.md:45, CLAUDE.md:117-123, CLAUDE.md:182-184, CLAUDE.md:199-200, /home/user/ArcGISRunner/docs/TASKS.md:29, /home/user/ArcGISRunner/docs/TASKS.md:32, /home/user/ArcGISRunner/docs/TASKS.md:38, decision-log:D20, decision-log:P22

### W2.6 Record links: #runner= hash, featureKey, Copy link, open from link

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.6, W0.7, W0.8, W0.9, W2.3, W2.4

Write and read Runner's location in the URL hash as `#runner={profileId}:{layerId}:{view|edit}:{featureKey}`, merged with Experience Builder's own hash parameters. Opening a link goes to that screen and selects and zooms to the feature. This task owns both `view` and `edit` links and the one Copy link control. View gets the control here; W3.2's Edit screen places the same control and joins the same hash and link handling.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/lib/ (path ambiguous, CLAUDE.md:163)`, `widgets/arcgis-runner/src/runtime/kinds/crud/ViewScreen`, `widgets/arcgis-runner/src/runtime/kinds/crud/`, `docs/TASKS.md`

Steps:

1. Branch from `main`. Never push to `main`.
2. Build on the result of the URL hash spike (W0.9). If the spike failed, stop and raise it with the brain (see issue 'No fallback if the URL hash spike fails').
3. In crud `lib/`, format and parse `#runner={profileId}:{layerId}:{view|edit}:{featureKey}`. Add Vitest round-trip tests for `view` and `edit`.
4. In crud `lib/`, merge the `runner` parameter into the existing hash without removing or changing Experience Builder's own parameters. Add tests showing the other parameters survive writes and reads.
5. In crud `lib/`, choose `featureKey`. Use the GlobalID when the layer has a `globalIdField`, otherwise use the ObjectID (`objectIdField`). Add tests for both cases.
6. Write the hash from navigation whenever Runner opens a feature's View or Edit screen, with the matching mode. View writes `view` now. Edit writes `edit` through the same path once W3.2's Edit screen joins navigation. What the hash holds on other screens is open (see issue 'When Runner writes and clears the #runner= hash').
7. Read the hash when the widget loads.
8. Respond only when the link's `profileId` matches this widget's profile.
9. When the link matches, open the screen the link names through navigation. Find the feature by `featureKey` with `queryFeatures`, select it through the Map widget's data source, and zoom to it. Add Vitest tests that a `view` link routes to View and an `edit` link routes to Edit.
10. Build Copy link as one control that copies the page URL with the record link for the current screen. Place it on View. W3.2 places the same control on Edit.
11. Grant nothing through a link. An `edit` link still needs edit permission, and the service decides what the user can do.
12. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. The user runs it and reports back before merging.

Acceptance:

- Opening a feature's View writes `#runner={profileId}:{layerId}:view:{featureKey}` into the hash, and Experience Builder's own hash parameters stay as they were.
- Writing the hash doesn't reload the page.
- `featureKey` is the GlobalID when the layer has one, otherwise the ObjectID.
- Opening a URL with a `#runner=` view link opens View, then selects and zooms to the feature.
- Vitest tests cover formatting, parsing and routing for both `view` and `edit` links.
- With two Runner widgets on one page, only the one whose profile matches the link responds.
- Copy link on View copies a URL that reopens the same record.
- A link grants no access. A user without permission gets the service's message.
- `npm test` passes.

Local checklist (user runs):

- Build the widget in Developer Edition 1.18 and confirm there are no TypeScript errors.
- Open a feature's View. Confirm the URL hash contains `runner=...` and Experience Builder's own hash parameters are still there.
- Click Copy link and paste the URL into a new tab. Confirm Runner opens the same feature's View, with the feature selected and zoomed to on the map.
- Repeat on a layer that has a GlobalID field and on one that has only an ObjectID. Confirm the `featureKey` in each link.
- Switch Experience Builder pages and use browser back and forward. Confirm neither Runner's parameter nor Experience Builder's parameters are lost, and the page doesn't reload.
- Put two Runner widgets with different profiles on one page and open a link. Confirm only the widget with the matching profile responds.
- Clicking through `edit` links and Copy link on Edit waits for W3.2's Edit screen. Confirm their Vitest tests pass here (see issues).
- Portal-hosted check (needs the widget deployed from the builder app and registered in Portal 12.0): open a link to a feature on a non-public layer, signed out or as a user without access. Confirm Runner shows the service's message and grants nothing.

Issues: ISS-13, ISS-45, ISS-59, ISS-72, ISS-74, ISS-155, ISS-170, ISS-171, ISS-174, ISS-176, ISS-181, ISS-184, ISS-185, ISS-186, ISS-187

Sources: R376, R377, R378, R379, R380, R381, R382, R333, R417, R418, R161, R162, R012, R013, R447, R448, R416, CLAUDE.md:46, CLAUDE.md:124-130, CLAUDE.md:182-184, CLAUDE.md:199-202, /home/user/ArcGISRunner/docs/TASKS.md:12-14, /home/user/ArcGISRunner/docs/TASKS.md:33, /home/user/ArcGISRunner/docs/TASKS.md:38, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:50-51, decision-log:D21, decision-log:P15

## W3 — Widget Phase 3 — crud: Add / Edit / Delete

W3 builds the write half of the `crud` kind in the widget. It covers one input renderer per `inputType`, Add and Edit screens built from the designer's section layouts, geometry drawing with `SketchViewModel`, the Delete action, and the `crud` JavaScript events. Every write goes to the builder app's `POST /api/runtime/profiles/{id}/edits/{layerId}` and never to `applyEdits()`, so `EditGate` and the PHP hooks run on every edit. The widget's own checks only shape the UI. The JS events task runs only if the CSP spike (W0.10) passes. The edit client's Bearer token depends on the auth spike (W0.8). All W3 code follows CLAUDE.md:206-207: TypeScript strict, `any` only where an SDK typing gap forces it (with a comment saying why), and comments only for a non-obvious why. Each task session reads CLAUDE.md, branches from `main`, opens a pull request into `main`, and never pushes to `main` directly.

### W3.1 Input renderers per `inputType` (+ `readonly` fallback)

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.4, B2.4

Render each form field with one renderer per `inputType` key, using the same keys as the builder's `InputTypes` registry. An unknown key falls back to `readonly` and logs a warning.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/inputs/`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md, then branch from `main`.
2. Read the `InputType` union and the `FieldConfig` shape from the builder repo's `docs/CONFIG_OUTPUT_SCHEMA.md`. Don't write a second definition of the contract in this repo (see issues).
3. In the crud `lib/` folder, with no `jimu-*` imports, write the input-type mapping from a `FieldConfig` to a renderer key. `text`, `textarea`, `number`, `date`, `datetime`, `dropdown` and `readonly` map to themselves. Any other key maps to `readonly` and logs a warning.
4. In the same lib module, write the editability check. A field is editable on Add/Edit only if `editable` is true and its `inputType` isn't `readonly`.
5. Write Vitest tests for each known key, for an unknown key (returns `readonly` and logs a warning), and for the editability check. Run `npm test`.
6. In `src/runtime/kinds/crud/inputs/`, add one thin renderer per key, using Calcite Components through `jimu-ui` where they are wrapped. `text` and `textarea` handle string fields, `number` handles numeric fields, and `date` and `datetime` handle date fields. `dropdown` lists the entries of the field's coded-value domain (`codedValues` with `name` and `code`). `readonly` shows the value without an input.
7. Keep the logic in lib. Renderers only wire lib output to `jimu-ui`/Calcite.
8. Use TypeScript strict. Use `any` only where an SDK typing gap forces it, with a comment saying why. Write comments only for a non-obvious why (CLAUDE.md:206-207).
9. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. Never push to `main` directly.

Acceptance:

- The widget has exactly one renderer for each of `text`, `textarea`, `number`, `date`, `datetime`, `dropdown` and `readonly`, and the key set matches `app/Runner/Kinds/Crud/InputTypes.php` in the builder repo.
- A field with an unknown `inputType` renders as `readonly`, and a warning is logged.
- A field whose `editable` is false, or whose `inputType` is `readonly`, can't be changed.
- System fields (objectId, globalId, editor-tracking, Shape__Area/Length) can't be changed on Add or Edit.
- `dropdown` offers the entries of the field's coded-value domain.
- In the cloud, `npm test` passes the lib tests, and the lib module has no `jimu-*` imports.
- The widget compiles in Developer Edition 1.18.

Local checklist (user runs):

- Link the branch's `widgets/arcgis-runner` into Developer Edition 1.18 with `mklink /J <exb>\client\your-extensions\widgets\arcgis-runner <repo>\widgets\arcgis-runner`, then run `npm start` in that checkout.
- Confirm Developer Edition compiles the widget with no TypeScript errors.
- The visual check of each input type in a form happens in W3.2.
- Report the result on the pull request before merging.

Issues: ISS-01, ISS-10, ISS-45, ISS-106, ISS-108, ISS-111, ISS-148, ISS-173, ISS-188, ISS-189, ISS-190, ISS-191, ISS-192

Sources: R383, R384, R126, R127, R128, R129, R130, R131, R132, R133, R134, R135, R136, R176, R177, R183, R194, R195, R329, R406, R407, R408, R410, R412, R012, R013, R019, R021, R022, R447, CLAUDE.md:131-132, CLAUDE.md:182-184, CLAUDE.md:206-207, /home/user/arcgisbuilderwebapplication/CLAUDE.md:164-174, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:71-83, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:131-132, decision-log:P16, docs/TASKS.md:37

### W3.2 Add/Edit forms from section layouts

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W3.1, W2.1, W2.2, W2.5, W2.6, B2.6

Build the Add and Edit screens (FormScreen) from the layer's `layouts.add` and `layouts.edit` sections, rendering each field with the W3.1 renderers. A screen is offered only when its profile page is on and the live layer allows that operation. This task also builds the Add/Edit side of the W2.5 and W2.6 behaviours, because the forms first exist here.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/FormScreen`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md, then branch from `main`.
2. In crud `lib/`, build a form model from a `FormLayout`. It keeps sections in their saved order with each section's `title`, keeps fields in their saved order, takes each field's `label` from `fields`, and takes each field's renderer key and editability from W3.1.
3. In crud `lib/`, write the page check for Add and Edit. Add needs `pages.add` and live `supportsAdd`. Edit needs `pages.edit` and live `supportsUpdate`. The profile's `capabilities` snapshot isn't enough, so recheck the live layer.
4. Write Vitest tests for the form model and the page check.
5. Build FormScreen as a screen inside the widget, not an Experience Builder page. Add renders `layouts.add` and Edit renders `layouts.edit`.
6. On Edit, read the feature's current attributes straight from the feature service with `queryFeatures` and fill the form.
7. Keep form values in local React state.
8. While Add or Edit is open, map clicks don't navigate to View, because those clicks are for drawing geometry. Reuse the W2.5 map-click logic.
9. Leaving Add or Edit with unsaved changes asks first.
10. Add a Copy link button to Edit. The link uses the W2.6 format `#runner={profileId}:{layerId}:edit:{featureKey}`, with `featureKey` = GlobalID when present, else ObjectID.
11. Opening an `edit` record link goes to Edit for that feature and selects and zooms to it, using the W2.6 hash logic. Record links never grant access, so an Edit opened from a link uses the same page and capability check as any other Edit.
12. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. Never push to `main` directly.

Acceptance:

- Add shows the `layouts.add` sections and fields in the designer's order, with section titles and field labels.
- Edit shows the `layouts.edit` sections, filled with the feature's current values.
- Fields that aren't editable (by the W3.1 check) can't be changed on either screen.
- Add isn't offered when `pages.add` is false or the live layer doesn't support add. Edit isn't offered when `pages.edit` is false or the live layer doesn't support update.
- Map clicks don't navigate while Add or Edit is open.
- Leaving a form with unsaved changes asks first.
- Edit has a Copy link button whose link follows the documented `#runner=` format and `featureKey` rule.
- Opening an Edit link opens Edit for that feature and selects and zooms to it on the map, gated by the same page and capability check.
- `npm test` passes.

Local checklist (user runs):

- Link the branch's widget into Developer Edition 1.18 with `mklink /J` and run `npm start`.
- Use a published profile with a layer that has Add and Edit turned on and fields set to every input type.
- Open Add. Section titles, field order and labels match the designer, each input type shows its control, and readonly or non-editable fields can't be changed.
- Open an existing feature in Edit. Its current values appear.
- Change a value and try to leave the form. A prompt asks first.
- While on Add or Edit, click a feature on the map. Runner doesn't navigate to View.
- On Edit, click Copy link and open the link in a new tab. The same feature opens in Edit and is selected and zoomed to on the map.
- Turn Edit off for the layer in the builder, republish, and reload the experience. Edit isn't offered, and the Edit link doesn't open Edit.
- Report the result on the pull request before merging.

Issues: ISS-13, ISS-14, ISS-154, ISS-157, ISS-181, ISS-182, ISS-187, ISS-188, ISS-189, ISS-192, ISS-193, ISS-194, ISS-195

Sources: R385, R182, R165, R164, R170, R193, R194, R196, R168, R086, R389, R390, R365, R085, R330, R362, R373, R374, R376, R377, R378, R379, R381, R382, R110, R012, R013, R019, R447, docs/TASKS.md:28, docs/TASKS.md:32-33, docs/TASKS.md:38, CLAUDE.md:113-122, CLAUDE.md:124-130, CLAUDE.md:135, CLAUDE.md:139-140, CLAUDE.md:182-184, decision-log:P15, decision-log:P22, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:92-94, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:127-135

### W3.3 Geometry via `SketchViewModel`; tables skip geometry

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W3.2

On Add and Edit, let the user draw or reshape the feature's geometry with `SketchViewModel` on the connected map, for point, multipoint, polyline and polygon layers. Tables skip geometry.

Files: `TBD in task`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md, then branch from `main`.
2. In crud `lib/`, decide per layer whether Add/Edit need geometry. There is no geometry when `kind` is `table` or `geometryType` is `null`. Otherwise, choose the `SketchViewModel` tool for the `geometryType` (`point`, `multipoint`, `polyline`, `polygon`). Write Vitest tests for this choice.
3. On Add for a layer with geometry, run `SketchViewModel` on the connected map's view so the user draws a new geometry of the layer's type. Keep that geometry with the form values for the save.
4. On Edit for a layer with geometry, load the feature's geometry and let the user reshape it with `SketchViewModel`. Keep the result with the form values for the save.
5. For tables, Add and Edit show attributes only, with no drawing.
6. Drawing relies on the W3.2 rule that map clicks don't navigate while Add or Edit is open.
7. Use TypeScript strict. Use `any` only where an SDK typing gap forces it, with a comment saying why. Write comments only for a non-obvious why (CLAUDE.md:206-207).
8. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. Never push to `main` directly.

Acceptance:

- Add on a point, multipoint, polyline or polygon layer lets the user draw that geometry type on the connected map.
- Edit on those layers lets the user change the existing geometry.
- Add and Edit on a table show no geometry drawing.
- Map clicks while drawing don't open View.
- `npm test` passes.

Local checklist (user runs):

- Link the branch's widget into Developer Edition 1.18 with `mklink /J` and run `npm start`.
- Use a published profile with one layer of each geometry type (point, multipoint, polyline, polygon) plus a table, with Add and Edit turned on.
- For each geometry layer, open Add and draw on the map. The drawn shape is the layer's geometry type.
- For each geometry layer, open an existing feature in Edit and reshape it.
- For the table, open Add and Edit. No drawing appears.
- Saving the drawn geometry is checked in W3.4.
- Report the result on the pull request before merging.

Issues: ISS-167, ISS-181, ISS-196, ISS-215

Sources: R386, R387, R160, R158, R373, R108, R323, R012, R013, R019, R021, R022, R447, CLAUDE.md:133-134, CLAUDE.md:120-121, CLAUDE.md:182-184, CLAUDE.md:206-207, docs/TASKS.md:39, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:47-49

### W3.4 Edit client → `POST /api/runtime/profiles/{id}/edits/{layerId}`; show server/hook messages

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.8, W1.1, W3.2, W3.3, B3.6

Send every `crud` write to the builder app's edit endpoint, never through `applyEdits()`, with the user's Portal token when the user is signed in and no token when anonymous. Show the server's and the PHP hooks' rejection messages to the user.

Files: `TBD in task`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md, then branch from `main`.
2. Take the request and response shape from the builder's edit endpoint (B3.6). Neither repo documents that shape yet (see issues).
3. In a jimu-free `lib/` module, build the URL `{base}/api/runtime/profiles/{profileId}/edits/{layerId}`. `base` comes from W1.1 (derived from `folderUrl`, or `builderBaseUrl` for local Developer Edition work). `profileId` comes from the widget config and `layerId` from the layer's `LayerConfig`.
4. When the user is signed in, add `Authorization: Bearer {token}` with the Portal token from Experience Builder's session, read the way the auth spike (W0.8) found. When the user is anonymous, send no Authorization header.
5. Write Vitest tests for URL building and for the header choice (signed in vs anonymous).
6. Wire the Add save to send an add and the Edit save to send an update. Each carries the form attributes and, for layers with geometry, the geometry from W3.3. Expose a delete call for W3.5.
7. The request carries no PHP or hook code. The builder app runs the layer's reviewed hook chosen in the wizard (`phpHook`).
8. Never call `applyEdits()` from the widget.
9. When the server rejects a write (`EditGate`, a PHP hook's `HookRejected` message, the feature service, or the anonymous rate limit), show the returned message to the user. If the action needs sign-in, show the service's message. The widget never asks for sign-in itself.
10. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. Never push to `main` directly.

Acceptance:

- Saving on Add or Edit sends a POST to `{base}/api/runtime/profiles/{profileId}/edits/{layerId}`.
- The widget code contains no `applyEdits()` call.
- The request carries no PHP or hook code.
- Signed-in requests carry `Authorization: Bearer`. Anonymous requests carry no Authorization header.
- A rejection from `EditGate`, a PHP hook or the feature service appears as a message in the widget.
- A saved add or update appears in the feature service, and editor tracking records the signed-in user the same way it does in Portal.
- A save from an Edit opened by a record link succeeds only when the profile and the service allow it.
- The widget shows no sign-in prompt of its own.
- `npm test` passes.

Local checklist (user runs):

- Run the builder app with the edit endpoint (B3.6) on IIS. Build the widget in Developer Edition 1.18 with `builderBaseUrl` in `config.json` set to the builder app.
- Signed in, add a feature to a layer and a row to a table. The browser dev tools show a POST to the edits endpoint with an `Authorization: Bearer` header, and the new record appears in the service.
- Save an add and an edit on a layer of each geometry type (point, multipoint, polyline, polygon) and on a table. Each appears in the service, including changed geometry. On a layer with editor tracking, the editor field shows your username.
- Anonymous, on a public editable layer, add a feature. The POST has no Authorization header. See the issue on where the anonymous check can run.
- Once builder hooks exist (see issues), on a layer whose PHP hook rejects the edit, save. The hook's message appears in the widget.
- On a layer the current identity can't edit, save. The service's message appears in the widget.
- Report the result on the pull request before merging.

Issues: ISS-02, ISS-09, ISS-68, ISS-70, ISS-130, ISS-131, ISS-133, ISS-139, ISS-149, ISS-167, ISS-188, ISS-190, ISS-195, ISS-197, ISS-200, ISS-208, ISS-210

Sources: R084, R125, R388, R092, R345, R346, R348, R349, R083, R108, R109, R095, R216, R231, R232, R233, R239, R243, R244, R382, R415, R144, R012, R013, R019, R447, docs/TASKS.md:13, docs/TASKS.md:40, CLAUDE.md:55, CLAUDE.md:62, CLAUDE.md:84-88, CLAUDE.md:130, CLAUDE.md:135-138, CLAUDE.md:182-184, docs/CONFIG_SCHEMA.md:14-17, /home/user/arcgisbuilderwebapplication/CLAUDE.md:137-142, /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, /home/user/arcgisbuilderwebapplication/CLAUDE.md:186-190

### W3.5 Delete action (List + View) with confirmation; gate on profile pages + live capabilities

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W2.3, W2.4, W3.4

Offer Delete as an action on List and View, behind a confirmation dialog, not as a screen. Show it only when `pages.delete` is true and the live layer supports delete.

Files: `widgets/arcgis-runner/src/runtime/kinds/crud/ListScreen`, `widgets/arcgis-runner/src/runtime/kinds/crud/ViewScreen`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/`, `docs/TASKS.md`

Steps:

1. Read CLAUDE.md, then branch from `main`.
2. In crud `lib/`, extend the page check from W3.2 to cover Delete. Delete needs `pages.delete` and live `supportsDelete`, not just the published `capabilities` snapshot. Write Vitest tests.
3. Add a Delete action to the List screen (W2.3) and the View screen (W2.4) when the check passes. Delete is an action, not a screen.
4. Clicking Delete opens a confirmation dialog. Cancelling sends nothing.
5. On confirm, send the delete through the edit client (W3.4).
6. Show server and hook rejection messages through W3.4.
7. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. Never push to `main` directly.

Acceptance:

- Delete appears on List and View only when `pages.delete` is true and the live layer supports delete.
- Delete asks for confirmation, and cancelling sends no request.
- Confirming sends a POST to the edits endpoint, and the feature is gone from the service.
- Delete works for a table row and for a feature of each geometry type.
- A rejection message appears in the widget.
- There is no Delete screen.
- `npm test` passes.

Local checklist (user runs):

- Link the branch's widget into Developer Edition 1.18 with `mklink /J` and run `npm start`, with the builder edit endpoint running and `builderBaseUrl` set.
- On a layer with Delete turned on, Delete appears on List and on View.
- Click Delete and cancel. No POST appears in the dev tools, and the feature remains.
- Click Delete and confirm. A POST is sent, and the feature is gone from the service.
- Delete a row from a table, and delete a feature from a layer of each geometry type (point, multipoint, polyline, polygon). Each is gone from the service.
- On a layer with Delete turned off in the profile, or on a service that doesn't support delete, no Delete action appears.
- Once builder hooks exist (see issues), on a layer whose PHP hook rejects deletes, confirm a delete. The hook's message appears.
- Report the result on the pull request before merging.

Issues: ISS-10, ISS-15, ISS-133, ISS-139, ISS-154, ISS-157, ISS-167, ISS-173, ISS-193, ISS-195, ISS-198, ISS-210

Sources: R369, R370, R196, R168, R389, R390, R084, R388, R108, R012, R013, R019, R447, docs/TASKS.md:41, CLAUDE.md:62, CLAUDE.md:117-118, CLAUDE.md:139-140, CLAUDE.md:182-184, decision-log:P8

### W3.6 Fire `crud` JS events

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.6, W0.10, W1.6, W2.2, W3.2, W3.4, W3.5, B3.7, B4.1

Call the layer's `customJs` handlers at the five `crud` events with a `CrudJsContext`. `beforeSave` and `beforeDelete` can cancel the write.

Files: `TBD in task`, `widgets/arcgis-runner/src/runtime/kinds/crud/lib/`, `docs/TASKS.md`

Steps:

1. Start only after the CSP spike (W0.10) passes and the shell's JS handler runner (W1.6) exists. If the spike shows handlers are blocked, stop. The user picks a fallback after the spike.
2. Read CLAUDE.md, then branch from `main`.
3. In crud `lib/`, build a `CrudJsContext` for each event, with `layerId`, `page`, `attributes`, `changedField` (onFieldChange only), `setValue(field, value)` and `cancel(message)` (before* events only).
4. Look up the handler body in the layer's `customJs[event]`. If there is none, do nothing. Otherwise call it through the shell's runner as `(ctx) => { ... }`.
5. Write Vitest tests. `changedField` is set only for `onFieldChange`, `cancel` stops the action only for `beforeSave` and `beforeDelete`, `setValue` changes the attribute, and a missing handler is skipped.
6. Fire `onPageLoad` when a page for this layer opens.
7. Fire `onFieldChange` when a form value changes on Add or Edit, with `changedField` set.
8. Fire `beforeSave` before the edit client sends an add or update. If the handler calls `cancel(message)`, don't send.
9. Fire `afterSave` after a successful add or update.
10. Fire `beforeDelete` before the edit client sends a delete. If the handler calls `cancel(message)`, don't send.
11. Apply `setValue` calls to the form values.
12. Check the box in `docs/TASKS.md` and open a pull request into `main` that ends with the local test checklist. Never push to `main` directly.

Acceptance:

- Each of the five events calls the layer's handler at the moment given in the schema's event table.
- `cancel` in `beforeSave` stops the add or update request, and `cancel` in `beforeDelete` stops the delete request.
- `onPageLoad`, `onFieldChange` and `afterSave` can't cancel.
- `setValue` changes the form value.
- A layer with no handler for an event behaves as if the event didn't exist.
- `npm test` passes.

Local checklist (user runs):

- In the builder's custom code step, publish a profile with a handler for each of the five events on one layer. Each handler writes a line to the browser console.
- Build the widget in Developer Edition 1.18. Run the checks below locally and again in a Portal-hosted experience, because the Content-Security-Policy applies there.
- Open a page for the layer. `onPageLoad` logs.
- Change a field on Add. `onFieldChange` logs with the field name.
- Make `beforeSave` call `ctx.cancel('...')` and save. No POST is sent.
- Remove that cancel and save. `afterSave` logs after the save succeeds.
- Make `beforeDelete` call `ctx.cancel('...')` and confirm a delete. No POST is sent.
- Make `onFieldChange` call `ctx.setValue` on another field. That field's value changes in the form.
- Report the result on the pull request before merging.

Issues: ISS-11, ISS-15, ISS-20, ISS-75, ISS-148, ISS-166, ISS-188, ISS-198, ISS-199

Sources: R391, R088, R137, R138, R166, R185, R186, R187, R188, R189, R190, R191, R192, R392, R393, R394, R395, R396, R397, R359, R140, R141, R142, R143, R056, R111, R201, R012, R013, R019, R447, docs/TASKS.md:12, docs/TASKS.md:15, docs/TASKS.md:24, docs/TASKS.md:42, CLAUDE.md:105-107, CLAUDE.md:182-184, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:100-124

## W4 — Widget Phase 4 — Polish (and deferred list)

W4 finishes the widget with i18n of the text it renders itself and tests that use the Developer Edition 1.18 jest setup. The test scope is still open. W4 also keeps the four explicitly deferred features (attachments, related records, offline editing, multiple Map widgets) and the other out-of-scope items (ArcGIS Online, Portal versions other than 12.0, Web AppBuilder, apps outside Experience Builder) out of v1. Many details are still open and are listed as issues. They include the locales, how the jest setup is confirmed, the jest command, the test scope, whether server messages are translated, and the builder endpoints the i18n checklist needs.

### W4.1 i18n of the widget's own UI text

**Repo:** widget · **Verification:** local checklist · **Depends on:** W1.2, W1.3, W2.2, W2.3, W2.4, W2.5, W2.6, W3.1, W3.2, W3.4, W3.5

Read every text string the widget renders itself from `src/translations/default.ts`, as the Phase 4 i18n item requires. The file today holds keys from the earlier per-layer-settings design, which Phase 1 replaces (O9). W4.1 adds the keys that the current screens need.

Files: `widgets/arcgis-runner/src/translations/default.ts`, `widgets/arcgis-runner/src/setting/setting.tsx`, `widgets/arcgis-runner/src/runtime/shell/`, `widgets/arcgis-runner/src/runtime/kinds/crud/`

Steps:

1. Branch from `main`, open a pull request into `main`, and never push to `main` directly.
2. List every text string the widget renders itself. Cover the settings panel (map selector, profile dropdown) and the shell error states (unknown `schemaVersion`, unknown kind with "update the Runner widget", unreachable builder app, webmap mismatch). Cover the `crud` screens (layer picker, List with pagination, View, Add, Edit) and the input renderers for each `inputType` (text, textarea, number, date, datetime, dropdown, readonly). Cover the Add/Edit geometry controls, the Delete confirmation dialog, the map-click pick list, the unsaved-changes prompt and the Copy link button.
3. Put those strings in `widgets/arcgis-runner/src/translations/default.ts`. Read them in the `jimu-*` components through Experience Builder's translation mechanism. Verify the exact API in Developer Edition 1.18.
4. Keep `lib/` modules free of `jimu-*` imports. Only the thin components look up text.
5. Show server, hook and service messages to the user (CLAUDE.md:88, :138). Whether i18n applies to them is an open question.
6. Leave profile-supplied text (profile name, layer titles, field labels, section titles) as stored until the open question on profile text is decided.
7. Update `docs/TASKS.md` as the per-task workflow requires. The checkbox is shared with the tests item (see open question).
8. End the pull request with a local test checklist.

Acceptance:

- No UI text the widget renders is hardcoded in a `.tsx` file. Each string comes from `src/translations/default.ts`.
- Server, hook and service messages reach the user.
- In Developer Edition 1.18, every screen, input, dialog and error state shows readable text, never a translation key.

Local checklist (user runs):

- Link the widget into the Developer Edition 1.18 checkout with `mklink /J`, run `npm start`, and build.
- Open Runner's settings panel. Confirm that the map selector and profile dropdown show readable text and no translation keys.
- Connect a Map widget whose webmap doesn't match the chosen profile. Confirm that the shell's mismatch error reads correctly.
- Set `builderBaseUrl` to an address with no server behind it. Confirm that the unreachable-server error reads correctly.
- Open List, View, Add and Edit for a layer. Confirm that all labels, buttons, inputs and pagination text read correctly.
- On Add and Edit for a feature layer, confirm that the geometry controls show readable text.
- Open Add and Edit for a table. Confirm that the form reads correctly without geometry controls.
- Open the Delete confirmation, the map-click pick list (click where features overlap), the unsaved-changes prompt and Copy link. Confirm that each one shows readable text.
- Trigger a save that the server rejects (an `EditGate` rejection works if no hook exists yet). Confirm that the server's message appears.
- Report the results in the pull request before merging.

Issues: ISS-01, ISS-02, ISS-157, ISS-215, ISS-216, ISS-217, ISS-218

Sources: R419, R042, R409, R437, R438, R336, R338, R354, R355, R356, R364, R365, R366, R369, R372, R374, R381, R383, R129, R386, R387, R349, R388, R410, R412, R012, R013, R019, R447, docs/TASKS.md:46, CLAUDE.md:88, CLAUDE.md:131-138, CLAUDE.md:164, CLAUDE.md:182-184

### W4.3 Developer Edition jest tests for the widget

**Repo:** widget · **Verification:** cloud tests + local checklist · **Depends on:** W0.5, W0.6

Add widget tests that use the Developer Edition 1.18 jest setup, as the Phase 4 backlog line 'tests (Developer Edition jest setup)' requires. The test scope is open (see issue). The cloud Vitest tests of `lib/` modules stay as they are.

Files: TBD in task

Steps:

1. Branch from `main`, open a pull request into `main`, and never push to `main` directly.
2. Add jest tests for what the brain names in the task prompt (the test scope is an open question). Use the command, folder and file naming of the Developer Edition 1.18 jest setup (unverified; see issue).
3. Keep logic in `lib/` modules with their Vitest tests, as CLAUDE.md:193-197 requires.
4. Keep root `npm test` running the `lib/` Vitest tests. No jest test file may break it.
5. Put the jest run in the pull request's local test checklist, because the cloud session can't run it.
6. Update `docs/TASKS.md` as the per-task workflow requires. The checkbox is shared with the i18n item (see open question).

Acceptance:

- The jest tests run and pass in the user's Developer Edition 1.18 checkout.
- Root `npm test` still runs the `lib/` Vitest tests and passes in the cloud.
- The pull request ends with a local test checklist that includes the jest run.

Local checklist (user runs):

- Check out the branch in the repo that the Developer Edition 1.18 checkout links with `mklink /J`.
- Run the jest command given in the pull request. Confirm that it finds the Runner tests and that they pass.
- Build the widget in Developer Edition 1.18. Confirm that it still loads in an experience.
- Report the results in the pull request before merging.

Issues: ISS-48, ISS-218, ISS-219

Sources: R420, R042, R398, R399, R410, R412, R012, R013, R017, R019, R444, R447, docs/TASKS.md:10, docs/TASKS.md:46, CLAUDE.md:182-184, CLAUDE.md:190-200

### W4.4 Keep explicitly deferred and out-of-scope features out of v1

**Repo:** both · **Verification:** review only

Keep attachments, related records, offline editing and multiple Map widgets out of v1, as `docs/TASKS.md` and the Project scope section of both CLAUDE.md files state. Also keep v1 off ArcGIS Online, Portal versions other than 12.0, Web AppBuilder and apps outside Experience Builder, as the same out-of-scope list states.

Files: `docs/TASKS.md`, `CLAUDE.md`, `kschultzBGOH/ArcGISBuilderWebApplication:CLAUDE.md`

Steps:

1. When reviewing each pull request in either repo, the brain checks that no code, UI, config key or profile field adds attachments, related records or offline editing.
2. The brain applies the same check to multiple Map widgets once the open question on its meaning is decided.
3. In the same review, the brain checks that no change adds support for ArcGIS Online, Portal versions other than 12.0, Web AppBuilder or apps outside Experience Builder (CLAUDE.md:56).
4. In the same review, the brain checks that no change lets end users switch profiles at runtime (the profile is fixed per widget instance) or lets Runner edit services that aren't in the profile's webmap.
5. Keep the out-of-scope list identical in the Project scope section of both CLAUDE.md files, as CLAUDE.md:25 requires.

Acceptance:

- Neither repo's v1 supports attachments, related records or offline editing.
- Multiple Map widgets are not supported in v1, in the sense the open question settles.
- Neither repo's v1 adds support for ArcGIS Online, Portal versions other than 12.0, Web AppBuilder or apps outside Experience Builder.
- Neither repo's v1 lets end users switch profiles at runtime, and Runner can't edit services that aren't in the profile's webmap.
- The out-of-scope list reads the same in the Project scope section of both CLAUDE.md files.

Issues: ISS-14, ISS-153, ISS-220, ISS-221

Sources: R099, R100, R101, R335, R102, R103, R104, R105, R003, R008, R014, R023, docs/TASKS.md:48-50, CLAUDE.md:25, CLAUDE.md:54, CLAUDE.md:56, /home/user/arcgisbuilderwebapplication/CLAUDE.md:101, /home/user/arcgisbuilderwebapplication/CLAUDE.md:103, R097, R098, R334, decision-log:O8

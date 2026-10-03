# ArcGIS Runner — Issue Register

Generated 2026-10-03 with the plan in [`PLAN.md`](./PLAN.md). Issues are documented, not resolved. Each one names the task (or "before coding") where it must be settled.

This file is identical in both repos (`kschultzBGOH/ArcGISBuilderWebApplication` and `kschultzBGOH/ArcGISRunner`). The brain session keeps them in sync.

## Summary

| Type | Count |
|---|---|
| pending-user-action | 7 |
| conflict | 18 |
| stale-code | 7 |
| plan-gap | 13 |
| risk | 14 |
| unverified-assumption | 46 |
| open-question | 116 |
| **total** | **221** |

## Index

| Id | Type | Resolve by | Title |
|---|---|---|---|
| ISS-01 | pending-user-action | before coding | CLAUDE.md line-by-line review not finished |
| ISS-02 | conflict | before coding | Session-loaded CLAUDE.md is an older design |
| ISS-03 | conflict | before coding | Build order versus widget Phase 0 'do first' |
| ISS-04 | conflict | before coding | Builder phasing differs between CLAUDE.md and TASKS.md |
| ISS-05 | conflict | before coding | Brain pushes docs directly while tasks use pull requests |
| ISS-06 | conflict | before coding | Original goal moved from widget to builder |
| ISS-07 | unverified-assumption | before coding | Proposed builder stack, sign-in, tests and IIS setup not confirmed |
| ISS-08 | unverified-assumption | before coding | Proposed profile storage model not confirmed |
| ISS-09 | unverified-assumption | before coding | Proposed runtime API, write path and anonymous rate limit not confirmed |
| ISS-10 | unverified-assumption | before coding | Proposed crud wizard rules not confirmed |
| ISS-11 | unverified-assumption | before coding | Proposed crud JS event set not confirmed |
| ISS-12 | unverified-assumption | before coding | Proposed widget shell, hosting and dev workflow not confirmed |
| ISS-13 | unverified-assumption | before coding | Proposed map sync, record links and drift warning not confirmed |
| ISS-14 | open-question | before coding | Validation deferral dropped without a recorded decision |
| ISS-15 | conflict | B2.3 | Delete: page or action |
| ISS-16 | pending-user-action | B0.4 | Register the OAuth app and provide the group id |
| ISS-17 | open-question | B0.4 | APP_URL is not recorded anywhere |
| ISS-18 | open-question | B0.4 | Where OAuth values are kept and who checks user-only boxes |
| ISS-19 | unverified-assumption | B0.4 | Portal 12.0 OAuth registration and endpoints unverified |
| ISS-20 | risk | B0.4 | Custom JavaScript runs with each user's Portal session |
| ISS-21 | unverified-assumption | B0.5 | Laravel major version and PHP patch level not pinned |
| ISS-22 | unverified-assumption | B0.5 | Composer plugins and scripts may not run in cloud sessions |
| ISS-23 | unverified-assumption | B0.5 | How routes/api.php is created |
| ISS-24 | open-question | B0.5 | Calcite Components package and version not named |
| ISS-25 | open-question | B0.5 | config/runner.php contents and disk definition |
| ISS-26 | open-question | B0.5 | Env var values and formats not specified |
| ISS-27 | unverified-assumption | B0.5 | Session and cache storage not specified |
| ISS-28 | open-question | B0.5 | No test runner named for the builder SPA |
| ISS-29 | open-question | B0.5 | Which task commits public/web.config |
| ISS-30 | open-question | B0.5 | Pest setup comes after the first task that needs it |
| ISS-31 | open-question | B0.6 | ProfileStore operations and draft file shape not defined |
| ISS-32 | plan-gap | B0.6 | Slug id generation rule, and the plan disagrees on whether it is settled |
| ISS-33 | risk | B0.6 | Atomic replace on the UNC share unverified |
| ISS-34 | open-question | B0.6 | How ProfileStore and share writes are exercised in Phase 0 |
| ISS-35 | risk | B0.6 | Concurrent edits to one draft not addressed |
| ISS-36 | open-question | B0.7 | Portal fixture list and fixture content source |
| ISS-37 | open-question | B0.7 | Feature service calls, PortalClient and the applyEdits target |
| ISS-38 | open-question | B0.7 | What B0.7 can verify before PortalClient exists |
| ISS-39 | open-question | B0.7 | 'CI' mentioned but no CI system defined |
| ISS-40 | pending-user-action | W0.3 | Create main in the widget repo and make it the default |
| ISS-41 | conflict | W0.3 | Who creates main in the widget repo |
| ISS-42 | plan-gap | W0.3 | W0.3 is an action marked review-only |
| ISS-43 | open-question | W0.5 | Cloud test harness details not stated |
| ISS-44 | unverified-assumption | W0.5 | Vitest type-checking and empty-suite behaviour unverified |
| ISS-45 | open-question | W0.5 | crud lib/ folder path is ambiguous |
| ISS-46 | unverified-assumption | W0.5 | lib/ test files inside the Developer Edition junction |
| ISS-47 | unverified-assumption | W0.5 | Version alignment facts unchecked |
| ISS-48 | risk | W0.6 | jimu code is first compiled on the user's machine |
| ISS-49 | stale-code | W0.6 | Stale widget source in the minimal build |
| ISS-50 | conflict | W0.6 | Manifest task already partly done |
| ISS-51 | open-question | W0.6 | manifest.json author and version values |
| ISS-52 | unverified-assumption | W0.6 | Developer Edition 1.18 build command and output folder |
| ISS-53 | unverified-assumption | W0.6 | Which Developer Edition folders need npm start |
| ISS-54 | open-question | W0.7 | Deployment spike host versus 'registered once' |
| ISS-55 | unverified-assumption | W0.7 | Widget CORS headers and Portal registration unverified |
| ISS-56 | pending-user-action | W0.7 | Register the widget item in Portal |
| ISS-57 | unverified-assumption | W0.7 | Experience Builder 1.18 API names unverified |
| ISS-58 | open-question | W0.7 | Where spike results go, and whether spike code stays |
| ISS-59 | risk | W0.7 | No fallback if the deployment, auth or hash spike fails |
| ISS-60 | plan-gap | B0.8 | B0.8 depends on a widget build it doesn't list |
| ISS-61 | conflict | B0.8 | DEPLOYMENT.md contents differ between CLAUDE.md and TASKS.md |
| ISS-62 | open-question | B0.8 | Does DEPLOYMENT.md cover the remaining server env vars |
| ISS-63 | open-question | B0.8 | HTTPS binding not listed for the IIS site |
| ISS-64 | open-question | B0.8 | Network reach for public, anonymous users |
| ISS-65 | open-question | B0.8 | How the builder app itself is deployed, and server prerequisites |
| ISS-66 | open-question | B0.8 | How unmerged builder branches reach the real servers |
| ISS-67 | risk | B0.8 | Two CORS settings for the same origins |
| ISS-68 | risk | B0.8 | Laravel server is a single point of failure |
| ISS-69 | risk | B0.8 | Widget rebuild after a Portal upgrade |
| ISS-70 | unverified-assumption | W0.8 | Widget Portal token: API and server-side use unverified |
| ISS-71 | unverified-assumption | W0.8 | Public sharing needed for the anonymous check |
| ISS-72 | open-question | W0.9 | Hash spike: wanted behaviour and test location |
| ISS-73 | plan-gap | W0.9 | W0.9 dependency on W0.7 missing |
| ISS-74 | conflict | W0.9 | Hash merge stated as settled but still a spike question |
| ISS-75 | pending-user-action | W0.10 | CSP spike result and fallback choice |
| ISS-76 | conflict | W0.10 | Custom JavaScript in v1 scope but gated by a spike |
| ISS-77 | unverified-assumption | W0.10 | CSP may differ by Portal context |
| ISS-78 | open-question | B1.1 | B1.1 checklist can't run the full sign-in round trip |
| ISS-79 | unverified-assumption | B1.1 | community/self response shape unverified |
| ISS-80 | open-question | B1.1 | Token refresh trigger and expiry handling |
| ISS-81 | open-question | B1.2 | /auth/* route list not named |
| ISS-82 | open-question | B1.2 | Route files, session middleware and CSRF for builder and runtime routes |
| ISS-83 | open-question | B1.3 | Group check between login and save, and whether autosave counts |
| ISS-84 | open-question | B1.3 | Not-authorized page and no-session response |
| ISS-85 | open-question | B1.4 | KindRegistry contents, CrudSettingsValidator owner and runtime handler shape |
| ISS-86 | open-question | B1.4 | SPA kind registry file and entry shape not named |
| ISS-87 | open-question | B1.5 | Profile list behaviour details |
| ISS-88 | open-question | B1.5 | Task order between list Open, wizard shell and common steps |
| ISS-89 | open-question | B1.6 | Autosave timing and failure handling |
| ISS-90 | open-question | B1.6 | Later common steps in the Phase 1 shell |
| ISS-91 | conflict | B1.6 | Custom JavaScript listed as shared but stored per crud layer |
| ISS-92 | open-question | B1.7 | Changing kind or webmap after later steps are filled |
| ISS-93 | unverified-assumption | B1.7 | Webmap search call and behaviour unverified |
| ISS-94 | open-question | B1.7 | Source of RunnerProfile.portalUrl |
| ISS-95 | open-question | B2.1 | Which webmap layers count as feature layers |
| ISS-96 | unverified-assumption | B2.1 | Portal 12.0 webmap and layer JSON shapes unverified |
| ISS-97 | unverified-assumption | B2.1 | Identity used for detection requests |
| ISS-98 | open-question | B2.1 | Behaviour when a layer schema can't be fetched |
| ISS-99 | open-question | B2.1 | Detection endpoint path and code location |
| ISS-100 | unverified-assumption | B2.1 | Stored layerId must match the id the widget sees |
| ISS-101 | open-question | B2.1 | No step sets field labels or layer titles |
| ISS-102 | open-question | B2.2 | Field-level selection in Layers & fields |
| ISS-103 | open-question | B2.2 | No step sets layer order |
| ISS-104 | open-question | B2.3 | Capabilities snapshot timing |
| ISS-105 | open-question | B2.3 | List and View boxes and default checkbox state |
| ISS-106 | unverified-assumption | B2.4 | Field type names behind the input types table |
| ISS-107 | open-question | B2.4 | Default input type per field |
| ISS-108 | pending-user-action | B2.4 | More input options promised by the user |
| ISS-109 | unverified-assumption | B2.4 | How system fields are recognised |
| ISS-110 | open-question | B2.4 | Fields the service reports as not editable |
| ISS-111 | open-question | B2.4 | Where the SPA gets the valid-for mapping |
| ISS-112 | unverified-assumption | B2.5 | Drag reorder component not named |
| ISS-113 | open-question | B2.5 | Layouts for pages that are turned off |
| ISS-114 | open-question | B2.5 | Starting layouts and form layout rules |
| ISS-115 | open-question | B2.5 | List page size limits and sort field rule |
| ISS-116 | open-question | B3.1 | Custom CSS preview: what it renders and how it is isolated |
| ISS-117 | plan-gap | B3.1 | B3.1 cites a source that doesn't support it |
| ISS-118 | open-question | B3.2 | How validation failures reach the author |
| ISS-119 | open-question | B3.2 | customJs and phpHook before their Phase 4 UI |
| ISS-120 | open-question | B3.2 | Draft lifecycle after publish |
| ISS-121 | open-question | B3.3 | How ResolvePortalIdentity checks a token and handles a bad one |
| ISS-122 | plan-gap | B3.3 | B3.3 missing dependency on the auth spike |
| ISS-123 | plan-gap | B3.4 | No task holds the real-Portal WebmapAccess check |
| ISS-124 | unverified-assumption | B3.4 | Portal item request and how Portal 12.0 reports no access |
| ISS-125 | open-question | B3.4 | WebmapAccess cache duration, key and store |
| ISS-126 | conflict | B3.5 | Listing endpoint fields differ |
| ISS-127 | open-question | B3.5 | Runtime error responses not defined |
| ISS-128 | open-question | B3.5 | How the listing finds profiles for a webmap |
| ISS-129 | open-question | B3.5 | When a republished profile reaches the widget |
| ISS-130 | unverified-assumption | B3.5 | CORS mechanism and preflight on IIS |
| ISS-131 | open-question | B3.5 | Developer Edition origin and RUNNER_ALLOWED_ORIGINS |
| ISS-132 | open-question | B3.5 | Runtime routes stay ungated is only testable from Phase 3 |
| ISS-133 | open-question | B3.6 | Edit endpoint request and response contract |
| ISS-134 | open-question | B3.6 | EditGate handling of disallowed fields |
| ISS-135 | open-question | B3.6 | Server-side recheck of live capabilities |
| ISS-136 | open-question | B3.6 | Whether the edit endpoint checks WebmapAccess |
| ISS-137 | unverified-assumption | B3.6 | Client IP seen by Laravel under IIS |
| ISS-138 | risk | B3.6 | Edit endpoint open to anonymous callers |
| ISS-139 | risk | B3.6 | Edits run without PHP hooks until Phase 4 |
| ISS-140 | open-question | B3.7 | Deploy script form, location and where it runs |
| ISS-141 | open-question | B3.7 | Where the widget folder's web.config comes from |
| ISS-142 | stale-code | B3.7 | .gitignore lacks public/widgets/arcgis-runner/ |
| ISS-143 | plan-gap | B3.7 | B3.7 depends on the wrong widget task |
| ISS-144 | stale-code | W1.1 | Stale widget config.json and config.ts |
| ISS-145 | conflict | W1.1 | CONFIG_SCHEMA.md described as 'dev override only' |
| ISS-146 | open-question | W1.1 | builderBaseUrl override and config.json defaults |
| ISS-147 | stale-code | W1.2 | Stale settings panel placeholder |
| ISS-148 | conflict | W1.2 | Where the widget's profile types live |
| ISS-149 | open-question | W1.2 | Where shared request and auth code lives |
| ISS-150 | unverified-assumption | W1.2 | Reading the connected map's webmap id |
| ISS-151 | open-question | W1.2 | Can the settings panel run anonymously |
| ISS-152 | open-question | W1.2 | Settings panel states unspecified |
| ISS-153 | open-question | W1.2 | Meaning of 'multiple Map widgets' |
| ISS-154 | open-question | W1.2 | Where widget UI strings go before Phase 4 i18n |
| ISS-155 | risk | W1.2 | Portal-hosted checks wait on deployment |
| ISS-156 | stale-code | W1.3 | Stale widget.tsx comments and placeholder |
| ISS-157 | stale-code | W1.3 | Stale translation strings |
| ISS-158 | unverified-assumption | W1.3 | What Experience Builder shows when Runner's files can't load |
| ISS-159 | plan-gap | W1.3 | Map connection and unknown-kind message split between W1.3 and W1.4 |
| ISS-160 | unverified-assumption | W1.4 | Lazy-loaded files in the Developer Edition build |
| ISS-161 | stale-code | W1.5 | Widget root class doesn't match the docs |
| ISS-162 | open-question | W1.5 | Custom CSS tag details |
| ISS-163 | risk | W1.5 | Experience-wide CSS waits for a Runner to load |
| ISS-164 | open-question | W1.5 | No DOM test environment named for Vitest |
| ISS-165 | open-question | W1.6 | JS runner behaviour undefined |
| ISS-166 | risk | W1.6 | W1.6 has no real caller to test against |
| ISS-167 | pending-user-action | W2.1 | Test webmap and profile for widget checklists |
| ISS-168 | open-question | W2.1 | Profile layers that match no map layer |
| ISS-169 | open-question | W2.1 | How the url fallback compares URLs |
| ISS-170 | unverified-assumption | W2.1 | Map widget data source API, and tables without layer views |
| ISS-171 | open-question | W2.2 | What pages.list and pages.view = false mean in the widget |
| ISS-172 | open-question | W2.2 | Layer picker label and starting screen |
| ISS-173 | unverified-assumption | W2.2 | Which Calcite components jimu-ui 1.18 wraps |
| ISS-174 | open-question | W2.3 | Read path for List and View |
| ISS-175 | open-question | W2.3 | How the user opens View from List |
| ISS-176 | open-question | W2.3 | Row selection and links for tables |
| ISS-177 | open-question | W2.3 | List order and value display |
| ISS-178 | unverified-assumption | W2.3 | Paged and sorted queries unverified |
| ISS-179 | open-question | W2.3 | Selections made outside Runner |
| ISS-180 | plan-gap | W2.4 | W2.4's checklist has no way into View |
| ISS-181 | conflict | W2.5 | Phase 2 backlog items act on Phase 3 screens |
| ISS-182 | open-question | W2.5 | Unsaved-changes prompt and pick list content |
| ISS-183 | unverified-assumption | W2.5 | Limiting hitTest without touching the popup |
| ISS-184 | open-question | W2.5 | Two Runners on one page: clicks and shared profiles |
| ISS-185 | open-question | W2.6 | When Runner writes and clears the hash |
| ISS-186 | open-question | W2.6 | Escaping in the record link format |
| ISS-187 | open-question | W2.6 | Record links that can't be followed |
| ISS-188 | open-question | W3.1 | When schema v1 freezes |
| ISS-189 | open-question | W3.1 | Range domain, length and nullable handling in forms |
| ISS-190 | unverified-assumption | W3.1 | Date value format unverified |
| ISS-191 | open-question | W3.1 | Widget re-check of system fields |
| ISS-192 | risk | W3.1 | Renderers can't be seen until forms exist |
| ISS-193 | unverified-assumption | W3.2 | Live capability check unverified |
| ISS-194 | open-question | W3.2 | Layout fields missing from fields or the live service |
| ISS-195 | open-question | W3.2 | Add starting values, entry points and post-action navigation |
| ISS-196 | unverified-assumption | W3.3 | SketchViewModel details and drawing steps |
| ISS-197 | unverified-assumption | W3.4 | Anonymous edit check in a Developer Edition preview |
| ISS-198 | open-question | W3.5 | beforeDelete timing and ctx.page for Delete |
| ISS-199 | open-question | W3.6 | Handler behaviour gaps |
| ISS-200 | open-question | B4.1 | Who builds the Custom code step if the JS editor is blocked |
| ISS-201 | open-question | B4.1 | JS handler editor details |
| ISS-202 | open-question | B4.2 | LayerHook method signatures |
| ISS-203 | open-question | B4.2 | HookRegistry key format and discovery |
| ISS-204 | conflict | B4.3 | One backlog line covers B4.3 and B4.4 |
| ISS-205 | conflict | B4.3 | 'Every edit runs the layer's PHP hook' versus nullable phpHook |
| ISS-206 | open-question | B4.3 | EditGate rules and hook-changed attributes |
| ISS-207 | open-question | B4.3 | after* hook behaviour on success and failure |
| ISS-208 | plan-gap | B4.3 | B4.3 and B4.5 checks need the widget edit path |
| ISS-209 | open-question | B4.4 | No route named for the hook list |
| ISS-210 | open-question | B4.5 | Example hook behaviour and production presence |
| ISS-211 | open-question | B4.6 | Drift warning scope |
| ISS-212 | open-question | B4.7 | Error handling and session timeout UX scope |
| ISS-213 | open-question | B4.8 | Second kind unnamed and timing unclear |
| ISS-214 | open-question | B4.8 | schemaVersion handling when a kind is added |
| ISS-215 | plan-gap | W4.1 | W4.1 missing dependencies |
| ISS-216 | open-question | W4.1 | i18n scope: locales, profile text and server messages |
| ISS-217 | unverified-assumption | W4.1 | Experience Builder translation API and file layout |
| ISS-218 | open-question | W4.1 | One backlog checkbox covers i18n and tests |
| ISS-219 | unverified-assumption | W4.3 | Developer Edition jest setup, scope and coexistence with Vitest |
| ISS-220 | plan-gap | W4.4 | Out-of-scope items missing from the W4.4 scope guard |
| ISS-221 | open-question | W4.4 | Deferred list has no checkbox, review point or owner |

## Details

### ISS-01 CLAUDE.md line-by-line review not finished

**Type:** pending-user-action · **Resolve by:** before coding · **Affects:** B0.1, B0.2, B0.3, B1.1, B2.1, B3.1, B4.1, W0.3, W0.5, W1.1, W2.1, W3.1, W4.1

The review of the widget CLAUDE.md stopped at 'How work gets done'. Its Conventions and Response Style sections are unreviewed, and the builder CLAUDE.md has had no line-by-line review. Every task session reads these files first, and most plan tasks rest on them. Both backlogs and the widget CLAUDE.md heading also call coding sessions 'builder sessions', which a reader can confuse with the Builder Web Application.

Sources: decision-log:O4, CLAUDE.md:171, CLAUDE.md:204-210, /home/user/ArcGISRunner/docs/TASKS.md:3

### ISS-02 Session-loaded CLAUDE.md is an older design

**Type:** conflict · **Resolve by:** before coding · **Affects:** W0.3, W1.1, W2.1, W3.4, W4.1

The CLAUDE.md text loaded into this planning session matches commit 5391b60. It describes one JSON per webmap from GET /api/configs/{webmapId}, writes through FeatureLayer.applyEdits(), a config URL setting, geometryType 'point | polyline | polygon | table', and a src/components/ layout. The CLAUDE.md on disk and the builder docs use profiles, /api/runtime/profiles/... endpoints, writes only through the builder app, multipoint/null geometry, and a shell/kinds layout. A task session given the older text, or a main created from the wrong commit, would build the replaced design.

Sources: session-loaded CLAUDE.md context (matches commit 5391b60), git:5391b60:CLAUDE.md, CLAUDE.md:135-137, CLAUDE.md:149-164, /home/user/arcgisbuilderwebapplication/CLAUDE.md:155-159, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:49

### ISS-03 Build order versus widget Phase 0 'do first'

**Type:** conflict · **Resolve by:** before coding · **Affects:** W0.5, W0.6, W0.7, W0.8, W0.9, W0.10, B0.8, B4.1

CLAUDE.md and D6 say the builder app is built first and the widget consumes its output. The widget backlog labels Phase 0 'do first', and builder Phase 4 depends on the widget CSP spike. The deployment spike's first hosting option assumes a builder IIS site exists. The sources don't say whether W0 runs before, alongside or after the builder phases.

Sources: CLAUDE.md:21, decision-log:D6, /home/user/ArcGISRunner/docs/TASKS.md:5, /home/user/ArcGISRunner/docs/TASKS.md:12, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47

### ISS-04 Builder phasing differs between CLAUDE.md and TASKS.md

**Type:** conflict · **Resolve by:** before coding · **Affects:** B0.5, B0.6, B1.1, B1.4, B1.5, B1.6

The builder CLAUDE.md puts the scaffold and the profile store in Phase 1 'Foundation' and has no Phase 0. The builder backlog puts the scaffold and ProfileStore in Phase 0 'Decisions & scaffolding'. The plan follows TASKS.md placement and B1 dependencies on B0.5 and B0.6 assume it.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:246-247, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11-12

### ISS-05 Brain pushes docs directly while tasks use pull requests

**Type:** conflict · **Resolve by:** before coding · **Affects:** B0.1, B0.2, B0.3, B4.8, W0.3

D24 says each task gets its own branch and a pull request into main, and task sessions never push to main. So far the brain pushed planning docs straight to main in the builder repo and to claude/arcgis-runner-setup-uwxnyf in the widget repo. B0.1-B0.3 are backlog tasks that landed without pull requests. The sources don't say whether the brain's own doc edits, including the B4.8 design changes, follow the pull request rule.

Sources: decision-log:O10, decision-log:D24, /home/user/arcgisbuilderwebapplication/CLAUDE.md:260-265, CLAUDE.md:182-184, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:7-9

### ISS-06 Original goal moved from widget to builder

**Type:** conflict · **Resolve by:** before coding · **Affects:** B2.1, B2.2, W1.1, W2.1

D2 described a widget that auto-detects map layers and tables and is configurable, including which fields show. The current design moves detection and field configuration into the builder wizard. The widget config holds only the profile. The user should confirm that this shift matches the original goal.

Sources: decision-log:D2, /home/user/arcgisbuilderwebapplication/CLAUDE.md:61-63, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:3-4, CLAUDE.md:113-114

### ISS-07 Proposed builder stack, sign-in, tests and IIS setup not confirmed

**Type:** unverified-assumption · **Resolve by:** before coding · **Affects:** B0.5, B0.7, B0.8, B1.1, B1.2, B1.3

The user did not object to these proposals but never confirmed them. P1: Laravel serves a React + TypeScript SPA from resources/js via Vite (laravel-vite-plugin), with Calcite Components and same-origin session cookies; the SPA never calls Portal. P2: Portal OAuth2 authorization-code flow with redirect {APP_URL}/auth/callback, tokens in the server session, group check via /sharing/rest/community/self at login and on every save/publish. P18: Pest with Http::fake(), nothing hits a real Portal. P20: PHP 8.4 NTS x64 FastCGI, URL Rewrite via committed public/web.config, app pool as a domain service account, CONFIG_ROOT as a UNC path, widget folder web.config for CORS.

Sources: decision-log:P1, decision-log:P2, decision-log:P18, decision-log:P20

### ISS-08 Proposed profile storage model not confirmed

**Type:** unverified-assumption · **Resolve by:** before coding · **Affects:** B0.6, B1.5, B1.6, B3.2

P3 is unconfirmed: immutable slug ids, autosaved drafts, Publish makes a profile live, storage at profiles/{id}.json and drafts/{id}.json under CONFIG_ROOT, atomic writes by temp file + rename.

Sources: decision-log:P3

### ISS-09 Proposed runtime API, write path and anonymous rate limit not confirmed

**Type:** unverified-assumption · **Resolve by:** before coding · **Affects:** B3.5, B3.6, W1.2, W1.3, W2.3, W3.4

These are unconfirmed. P4: GET /api/runtime/profiles?webmapId=, GET /api/runtime/profiles/{id}, POST /api/runtime/profiles/{id}/edits/{layerId}, with CORS limited to RUNNER_ALLOWED_ORIGINS. P13: reads go straight to the feature service and writes go only through the builder app. P14: anonymous edits rate-limited per IP by RUNNER_ANON_EDITS_PER_MINUTE.

Sources: decision-log:P4, decision-log:P13, decision-log:P14

### ISS-10 Proposed crud wizard rules not confirmed

**Type:** unverified-assumption · **Resolve by:** before coding · **Affects:** B2.3, B2.4, B2.5, B3.1, W3.1, W3.5

These are unconfirmed. P5: starting input types text, textarea, number, date, datetime, dropdown, readonly, filtered by field type. P7: the List page is columns only, and there is one stylesheet per profile. P8: Delete is an action with a confirmation dialog on List and View, not a screen. P16: an unknown inputType falls back to readonly with a warning, and system fields are always readonly.

Sources: decision-log:P5, decision-log:P7, decision-log:P8, decision-log:P16

### ISS-11 Proposed crud JS event set not confirmed

**Type:** unverified-assumption · **Resolve by:** before coding · **Affects:** B4.1, W1.6, W3.6

P6 is unconfirmed: crud events onPageLoad, onFieldChange, beforeSave, afterSave and beforeDelete, with a ctx object that has setValue and cancel.

Sources: decision-log:P6

### ISS-12 Proposed widget shell, hosting and dev workflow not confirmed

**Type:** unverified-assumption · **Resolve by:** before coding · **Affects:** W0.5, W1.1, W1.2, W1.3, W1.4, W1.5

These are unconfirmed. P9: the builder base URL comes from props.context.folderUrl, with an optional builderBaseUrl override for local Developer Edition work. P10: the settings dropdown lists profiles for the connected webmap, and a mismatch shows an error. P12: kind modules are lazy-loaded. P17: widget logic sits in jimu-free lib/ modules tested with Vitest, and every widget PR ends with a local checklist. P21: Windows local dev uses mklink /J. P23: customCss goes into <head> once per profile and stays on unmount, and the root has class arcgis-runner and data-profile.

Sources: decision-log:P9, decision-log:P10, decision-log:P12, decision-log:P17, decision-log:P21, decision-log:P23

### ISS-13 Proposed map sync, record links and drift warning not confirmed

**Type:** unverified-assumption · **Resolve by:** before coding · **Affects:** W2.1, W2.5, W2.6, W3.2, B4.6

These are unconfirmed. P11: reuse the Map widget's layer data sources for selection sync. P15: link format #runner={profileId}:{layerId}:{view|edit}:{featureKey}, featureKey = GlobalID else ObjectID, a Copy link button, links never grant access. P22: the map popup is left alone, several hits show a pick list, map clicks don't navigate on Add/Edit, leaving a form with unsaved changes asks first. P19: polish includes a config drift warning.

Sources: decision-log:P11, decision-log:P15, decision-log:P19, decision-log:P22

### ISS-14 Validation deferral dropped without a recorded decision

**Type:** open-question · **Resolve by:** before coding · **Affects:** W4.4, B2.4, W3.2

Earlier CLAUDE.md versions deferred 'validation beyond domains / JS beforeSave / PHP hooks'. Commit e582c1d removed that line when it added the Project scope section. Neither the current out-of-scope list nor the TASKS.md deferred list mentions it. The sources don't say whether the removal was intended.

Sources: git:e582c1d CLAUDE.md, CLAUDE.md:48-56, /home/user/ArcGISRunner/docs/TASKS.md:48-50

### ISS-15 Delete: page or action

**Type:** conflict · **Resolve by:** B2.3 · **Affects:** B2.3, W3.5, W3.6

Several sources call List / Add / Edit / View / Delete 'pages', and PageKey includes 'delete'. The widget CLAUDE.md and P8 say Delete is an action with a confirmation dialog on List and View, not a screen. This affects the Pages step checkboxes, pages.delete, and what ctx.page holds for delete.

Sources: CLAUDE.md:15-17, CLAUDE.md:118, /home/user/arcgisbuilderwebapplication/CLAUDE.md:64, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:42, decision-log:P8

### ISS-16 Register the OAuth app and provide the group id

**Type:** pending-user-action · **Resolve by:** B0.4 · **Affects:** B0.4, B0.8, B1.1, B1.2, B1.3, B1.5, B1.6, B1.7, B2.1, B2.2, B3.1, B3.2

The user must register the builder's OAuth app in Portal 12.0 with redirect {APP_URL}/auth/callback and provide the allowed group id. Every builder local checklist that signs in (B1, B2, B3.1, B3.2, B4) waits on this.

Sources: decision-log:O1, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:10

### ISS-17 APP_URL is not recorded anywhere

**Type:** open-question · **Resolve by:** B0.4 · **Affects:** B0.4, B0.5, B0.8

No source gives the builder site's address. The OAuth redirect, the widget item URL ({APP_URL}/widgets/arcgis-runner/manifest.json) and the deployment doc depend on it. The sources use {APP_URL} only as a placeholder, and it is not in the env table. Backing it with Laravel's own APP_URL variable is an assumption.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:10, /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-127, /home/user/arcgisbuilderwebapplication/CLAUDE.md:150-151, /home/user/arcgisbuilderwebapplication/CLAUDE.md:192-201

### ISS-18 Where OAuth values are kept and who checks user-only boxes

**Type:** open-question · **Resolve by:** B0.4 · **Affects:** B0.4, B0.8, W0.4

The backlog says the user will 'provide group id' but not to whom or where. No source says who writes the OAuth values into the server's .env, or when. The client secret must stay out of git. No source says who checks the TASKS.md box for user-only tasks such as B0.4 and W0.4.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:10, /home/user/arcgisbuilderwebapplication/CLAUDE.md:197-198, /home/user/arcgisbuilderwebapplication/.gitignore:4, /home/user/ArcGISRunner/docs/TASKS.md:9, decision-log:O1

### ISS-19 Portal 12.0 OAuth registration and endpoints unverified

**Type:** unverified-assumption · **Resolve by:** B0.4 · **Affects:** B0.4, B0.8, B1.1, B1.2

The sources don't describe how Portal 12.0 registers an OAuth app (where the redirect URI goes, where client id and secret appear). They don't give the authorize and token endpoints, parameters or refresh grant. Whether one OAuth app accepts more than one redirect URI (for a test site) is also unverified. Http::fake() fixtures are only as accurate as this check.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-127, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18

### ISS-20 Custom JavaScript runs with each user's Portal session

**Type:** risk · **Resolve by:** B0.4 · **Affects:** B0.4, B1.3, B4.1, W0.10, W1.6, W3.6

Handler code from a profile runs in every widget user's browser with their Portal session. The only stated mitigation is keeping the builder group small, which is a user decision when choosing the allowed group. A passing CSP spike makes the risk live.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:180-182

### ISS-21 Laravel major version and PHP patch level not pinned

**Type:** unverified-assumption · **Resolve by:** B0.5 · **Affects:** B0.5, B0.7

CLAUDE.md says 'current major supporting PHP 8.4' with PHP 8.4.25 in production but names no Laravel version. The planning container had PHP 8.4.19 CLI, so task sessions may not match production.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:115, decision-log:D4

### ISS-22 Composer plugins and scripts may not run in cloud sessions

**Type:** unverified-assumption · **Resolve by:** B0.5 · **Affects:** B0.5, B0.7

In the cloud container Composer runs as root non-interactively and disables plugins. Laravel's post-install scripts and Pest's Composer plugin may not run, so a cloud install may not prove what B0.5 and B0.7 claim.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-268

### ISS-23 How routes/api.php is created

**Type:** unverified-assumption · **Resolve by:** B0.5 · **Affects:** B0.5

The builder layout lists /routes/api.php. Recent Laravel skeletons may not create it by default, and the usual way to add it may install an unnamed auth package. Which task creates it is not stated.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:229

### ISS-24 Calcite Components package and version not named

**Type:** open-question · **Resolve by:** B0.5 · **Affects:** B0.5

The sources say only 'Calcite Components' for the builder SPA. They name no npm package, React wrapper or version.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:121-122, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11

### ISS-25 config/runner.php contents and disk definition

**Type:** open-question · **Resolve by:** B0.5 · **Affects:** B0.5, B0.6, B3.5, B3.6

config/runner.php is named but its keys and the env vars it reads are not. The sources don't say which config file defines the runner_configs disk, or that RUNNER_ALLOWED_ORIGINS and RUNNER_ANON_EDITS_PER_MINUTE are read through config/runner.php.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:143, /home/user/arcgisbuilderwebapplication/CLAUDE.md:192-201, /home/user/arcgisbuilderwebapplication/CLAUDE.md:228, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11

### ISS-26 Env var values and formats not specified

**Type:** open-question · **Resolve by:** B0.5 · **Affects:** B0.5, B0.8, B3.5, B3.6

No source gives a default for RUNNER_ANON_EDITS_PER_MINUTE or says how RUNNER_ALLOWED_ORIGINS lists more than one origin. Only PORTAL_URL and CONFIG_ROOT have examples. The response to an over-limit request and whether refused edits count toward the limit are also unstated.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:141-142, /home/user/arcgisbuilderwebapplication/CLAUDE.md:196-201

### ISS-27 Session and cache storage not specified

**Type:** unverified-assumption · **Resolve by:** B0.5 · **Affects:** B0.5, B0.8, B1.2, B3.4

Portal tokens live in the server session and WebmapAccess caches briefly. No source names a session driver, cache driver, lifetime or database, so scaffold defaults would decide silently. No source says whether one or several IIS servers run the site, which matters for shared sessions.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:127, /home/user/arcgisbuilderwebapplication/CLAUDE.md:135-136, decision-log:D22

### ISS-28 No test runner named for the builder SPA

**Type:** open-question · **Resolve by:** B0.5 · **Affects:** B0.5, B1.4, B1.6, B2.2, B2.5, B3.1, B3.2, B4.1, B4.4, B4.7

Builder tests are Pest with Http::fake(), which covers PHP only. No test tool is named for resources/js. SPA behaviour in the kind registry, wizard shell, crud steps, Custom CSS, Review, Custom code and error UX can be checked only by brain review and local checklists.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:235, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-270, decision-log:P18

### ISS-29 Which task commits public/web.config

**Type:** open-question · **Resolve by:** B0.5 · **Affects:** B0.5, B0.8

CLAUDE.md says public/web.config is committed and DEPLOYMENT.md must cover it. The scaffold item doesn't list it, so no task is named to create it. Whichever task does touches IIS and needs a local checklist.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:117-118, /home/user/arcgisbuilderwebapplication/CLAUDE.md:268-270, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14

### ISS-30 Pest setup comes after the first task that needs it

**Type:** open-question · **Resolve by:** B0.5 · **Affects:** B0.5, B0.6, B0.7

B0.6 (ProfileStore) is checked with Pest, but the backlog lists Pest setup after ProfileStore. The plan makes B0.6 depend on B0.7, reversing backlog order. No source says whether the scaffold installs Pest, so B0.5 can only be checked by a build.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11-13, /home/user/arcgisbuilderwebapplication/CLAUDE.md:235, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-268

### ISS-31 ProfileStore operations and draft file shape not defined

**Type:** open-question · **Resolve by:** B0.6 · **Affects:** B0.6, B1.5, B1.6, B1.7, B2.2, B2.3, B2.4, B2.5, B2.6, B3.2, B3.5

The backlog names drafts, published files, atomic writes and slug ids but not the operations later tasks need: create/open/duplicate/delete draft, autosave and load, write published file, read by webmapId and by id. CONFIG_OUTPUT_SCHEMA.md defines only the published RunnerProfile. No source says whether a draft is a partial RunnerProfile (with partial CrudSettings such as a FieldConfig with no inputType) or also holds wizard state.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:12, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:22-23, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:8-25

### ISS-32 Slug id generation rule, and the plan disagrees on whether it is settled

**Type:** plan-gap · **Resolve by:** B0.6 · **Affects:** B0.6, B1.5, B1.7

profileId is a generated, immutable slug (example 'hydrant-inspections'). No source says what text it comes from, when it is generated, how a clash is handled, or what id a duplicate gets. A profile created from the list page exists before Name & kind sets a name. B0.6 states the slug is generated at creation, while the B1 issue treats timing and source as open, so the plan contradicts itself.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:146-147, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:16, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:12, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:22, traceability pass

### ISS-33 Atomic replace on the UNC share unverified

**Type:** risk · **Resolve by:** B0.6 · **Affects:** B0.6, B0.8, B3.2

Publish and autosave rely on temp file + rename. It is unverified that this replaces an existing file atomically on an SMB share from PHP on Windows, whether the temp file must be in the same folder, whether replace fails while another request has the file open, and whether Laravel's disk API accepts a UNC root. Cloud tests on Linux can't prove any of this.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:44-46, /home/user/arcgisbuilderwebapplication/CLAUDE.md:143-147, /home/user/arcgisbuilderwebapplication/CLAUDE.md:199, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-268

### ISS-34 How ProfileStore and share writes are exercised in Phase 0

**Type:** open-question · **Resolve by:** B0.6 · **Affects:** B0.6, B0.8

B0.6's local checklist writes to the real share, but no command or UI drives ProfileStore in Phase 0. Share write access belongs to the IIS app pool account, so a run under the user's account may give a different result. B0.8 has the same gap: no route or command writes to CONFIG_ROOT on IIS in Phase 0. Whether B0.6's check waits for the B0.8 IIS site is open.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:119-120, /home/user/arcgisbuilderwebapplication/CLAUDE.md:268-270, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:12-14

### ISS-35 Concurrent edits to one draft not addressed

**Type:** risk · **Resolve by:** B0.6 · **Affects:** B0.6, B1.6

Any group member can open any draft, and drafts autosave. No source gives a locking or conflict rule for two members saving the same drafts/{profileId}.json, or for an autosave landing during a publish. Atomic writes prevent torn files but not lost changes.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:44, /home/user/arcgisbuilderwebapplication/CLAUDE.md:146

### ISS-36 Portal fixture list and fixture content source

**Type:** open-question · **Resolve by:** B0.7 · **Affects:** B0.7, B1.1, B2.1

B0.7 calls for Http::fake() Portal 12.0 fixtures without listing which responses or where the bodies come from. Cloud sessions can't reach Portal to capture them, and fixtures that differ from real responses let tests pass wrongly. The Laravel mechanism that blocks unfaked HTTP calls also needs verifying.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:13, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267, /home/user/arcgisbuilderwebapplication/CLAUDE.md:276-277

### ISS-37 Feature service calls, PortalClient and the applyEdits target

**Type:** open-question · **Resolve by:** B0.7 · **Affects:** B0.7, B1.1, B2.1, B3.6

'All Portal calls go through PortalClient.' Layer schemas and applyEdits are feature service endpoints, possibly on a federated server. The sources don't say whether those calls go through PortalClient or the B0.7 fixtures, or which URL applyEdits is sent to; the profile's url snapshot is the only layer URL named.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:125, /home/user/arcgisbuilderwebapplication/CLAUDE.md:137-138, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:13, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:46

### ISS-38 What B0.7 can verify before PortalClient exists

**Type:** open-question · **Resolve by:** B0.7 · **Affects:** B0.7

No builder code calls Portal until PortalClient in Phase 1, so 'Portal calls in tests are answered by fixtures' is empty in Phase 0 or needs throwaway code. 'No test reaches a real Portal' passes trivially because cloud sessions can't reach Portal.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:13, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267

### ISS-39 'CI' mentioned but no CI system defined

**Type:** open-question · **Resolve by:** B0.7 · **Affects:** B0.7

P18 says nothing hits a real Portal 'in CI'. No source defines a CI pipeline; tests only run in cloud sessions.

Sources: decision-log:P18, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-268

### ISS-40 Create main in the widget repo and make it the default

**Type:** pending-user-action · **Resolve by:** W0.3 · **Affects:** W0.3, W0.4

main is to be created from claude/arcgis-runner-setup-uwxnyf with the user's approval, then the user sets it as the default branch in GitHub settings. origin has no main today. Task sessions branch from main, so no widget task can start until this is done.

Sources: decision-log:O2, /home/user/ArcGISRunner/docs/TASKS.md:9

### ISS-41 Who creates main in the widget repo

**Type:** conflict · **Resolve by:** W0.3 · **Affects:** W0.3, W0.4

The widget backlog says the user creates main and makes it default. The decision log says main is created from claude/arcgis-runner-setup-uwxnyf with approval and names no actor. CLAUDE.md says task sessions never push to main. The sources also don't say whether this plan is committed before main is created.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:9, decision-log:O2, CLAUDE.md:182-184

### ISS-42 W0.3 is an action marked review-only

**Type:** plan-gap · **Resolve by:** W0.3 · **Affects:** W0.3, W0.4

W0.3 creates main on origin, which is an action, but its verification is 'review-only'. W0.4, the matching action, is 'user-action'. W0.3 has no stated way to confirm main was created at the right commit.

Sources: traceability pass

### ISS-43 Cloud test harness details not stated

**Type:** open-question · **Resolve by:** W0.5 · **Affects:** W0.5

The sources name /package.json, TypeScript, Vitest and @arcgis/core 4.33. They don't name the config files, TypeScript and Vitest versions, the exact 4.33 patch, whether a lockfile is committed, whether npm test type-checks, or what npm test does with no test files yet.

Sources: CLAUDE.md:148, /home/user/ArcGISRunner/docs/TASKS.md:10

### ISS-44 Vitest type-checking and empty-suite behaviour unverified

**Type:** unverified-assumption · **Resolve by:** W0.5 · **Affects:** W0.5

Vitest may transpile TypeScript without type-checking, so strict mode may not be enforced by npm test alone. Its behaviour with zero test files is also unverified.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:10, CLAUDE.md:206

### ISS-45 crud lib/ folder path is ambiguous

**Type:** open-question · **Resolve by:** W0.5 · **Affects:** W0.5, W2.1, W2.6, W3.1

The repo layout shows crud/lib/ indented under kinds/crud/. It can be read as src/runtime/kinds/crud/lib/ or src/runtime/kinds/crud/crud/lib/. The harness scope and every crud task depend on the answer.

Sources: CLAUDE.md:160-163

### ISS-46 lib/ test files inside the Developer Edition junction

**Type:** unverified-assumption · **Resolve by:** W0.5 · **Affects:** W0.5, W0.6

The junction links all of widgets/arcgis-runner into Developer Edition. If lib/ test files that import vitest sit inside it, the Developer Edition 1.18 build may try to compile them.

Sources: CLAUDE.md:159, CLAUDE.md:163, CLAUDE.md:167-169

### ISS-47 Version alignment facts unchecked

**Type:** unverified-assumption · **Resolve by:** W0.5 · **Affects:** W0.5, W0.6

CLAUDE.md states that Experience Builder 1.18 uses Maps SDK 4.33, matches ArcGIS Enterprise 12.0, and that @arcgis/core 4.33 is on npm. The harness and build rely on these facts.

Sources: CLAUDE.md:68-71, CLAUDE.md:196

### ISS-48 jimu code is first compiled on the user's machine

**Type:** risk · **Resolve by:** W0.6 · **Affects:** W0.6, W0.7, W1.1, W1.2, W1.3, W1.4, W1.5, W1.6, W4.3

Developer Edition isn't available in the cloud, and all coding sessions run there, so code importing jimu-* is first compiled and type-checked during the user's local checklist. Errors surface after the pull request is open. W4.3's jest tests also can't run in the cloud.

Sources: decision-log:O7, decision-log:D23, CLAUDE.md:190-192

### ISS-49 Stale widget source in the minimal build

**Type:** stale-code · **Resolve by:** W0.6 · **Affects:** W0.6, W1.1, W1.2, W1.3, W1.5

The widget source predates the profile design (O9). The sources don't say whether the W0.6 minimal widget builds this source as it is or strips it first. widget.tsx:10 calls React.useState<JimuMapView>(null), which fails under strictNullChecks; whether the Developer Edition build enforces strict mode is unverified.

Sources: decision-log:O9, /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:5-17

### ISS-50 Manifest task already partly done

**Type:** conflict · **Resolve by:** W0.6 · **Affects:** W0.6

The widget backlog lists 'Manifest at exbVersion 1.18.0' as open. manifest.json already declares exbVersion 1.18.0 and version 1.18.0. Only the Developer Edition build check is outstanding.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:11, /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:3-4

### ISS-51 manifest.json author and version values

**Type:** open-question · **Resolve by:** W0.6 · **Affects:** W0.6

manifest.json has an empty author and a version equal to exbVersion (1.18.0). The sources don't say what either should hold.

Sources: /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:3-8

### ISS-52 Developer Edition 1.18 build command and output folder

**Type:** unverified-assumption · **Resolve by:** W0.6 · **Affects:** W0.6, W0.7, B0.8, B3.7

CLAUDE.md names client/dist/widgets/arcgis-runner/ as the build output, and the README says local dev runs npm start. No source names the production build command, or confirms that folder holds everything needed to host the widget. Whether icon.svg is copied into the output, and whether Portal needs it, is also unverified. DEPLOYMENT.md and the deploy script rely on this.

Sources: CLAUDE.md:75-77, CLAUDE.md:152, /home/user/ArcGISRunner/README.md:13-14, /home/user/arcgisbuilderwebapplication/CLAUDE.md:148-150, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43

### ISS-53 Which Developer Edition folders need npm start

**Type:** unverified-assumption · **Resolve by:** W0.6 · **Affects:** W0.5, W0.6, W0.7, W0.8, W0.9, W0.10

The checklists say to run 'that checkout's npm start'. Developer Edition may run separate server and client processes, each with its own npm start. The sources don't say which.

Sources: /home/user/ArcGISRunner/README.md:13-14

### ISS-54 Deployment spike host versus 'registered once'

**Type:** open-question · **Resolve by:** W0.7 · **Affects:** W0.7, B3.7

The spike may host on the builder app server or any HTTPS server with CORS. If another server is used, the Portal item points there, and a second registration or URL change would be needed later, though CLAUDE.md says the widget is registered once. The sources don't say which host, who provides it, or whether the spike's item is permanent.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:12, CLAUDE.md:10-12, CLAUDE.md:78-79, /home/user/arcgisbuilderwebapplication/CLAUDE.md:150-151

### ISS-55 Widget CORS headers and Portal registration unverified

**Type:** unverified-assumption · **Resolve by:** W0.7 · **Affects:** W0.7, B0.8, B3.7

The sources say a web.config in the widget folder adds CORS headers for the Portal origin, but don't list the headers. Unverified: which headers Experience Builder in Portal 12.0 needs to load a widget from another origin, whether Portal needs other settings, whether IIS serves every build file type (including .json) without extra MIME settings, and how Add Item -> Experience Builder widget behaves in 12.0.

Sources: CLAUDE.md:77-79, /home/user/arcgisbuilderwebapplication/CLAUDE.md:151-153, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14

### ISS-56 Register the widget item in Portal

**Type:** pending-user-action · **Resolve by:** W0.7 · **Affects:** W0.7, B0.8, B3.7

A Portal admin (the user) must register {APP_URL}/widgets/arcgis-runner/manifest.json once (Add Item -> Experience Builder widget). The builder backlog has no task of its own for this; the deployment spike includes it. B3.7's end-to-end check and every Portal-hosted widget check need it.

Sources: CLAUDE.md:78-79, /home/user/arcgisbuilderwebapplication/CLAUDE.md:150-151, /home/user/ArcGISRunner/docs/TASKS.md:12, decision-log:D12

### ISS-57 Experience Builder 1.18 API names unverified

**Type:** unverified-assumption · **Resolve by:** W0.7 · **Affects:** W0.7, W1.1, W1.2, W1.3

W0 and W1 rely on names from CLAUDE.md or the stale scaffold, not a 1.18 build: props.context.folderUrl, JimuMapViewComponent, useMapWidgetIds, MapWidgetSelector (jimu-ui/advanced/setting-components), AllWidgetSettingProps (jimu-for-builder) and props.onSettingChange. For folderUrl, it is unverified whether it is absolute, ends in a slash, or is exposed to the settings panel. The sources don't say what happens when folderUrl doesn't end in /widgets/arcgis-runner/ and no override is set.

Sources: CLAUDE.md:80-83, CLAUDE.md:96, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:19, /home/user/ArcGISRunner/widgets/arcgis-runner/src/setting/setting.tsx:1-14, /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:1-2

### ISS-58 Where spike results go, and whether spike code stays

**Type:** open-question · **Resolve by:** W0.7 · **Affects:** W0.7, W0.8, W0.9, W0.10

The sources say the brain updates CLAUDE.md when work shows the plan was wrong, and the CSP result feeds builder Phase 4. They don't say where spike results are recorded (pull request, CLAUDE.md or TASKS.md) or whether spike code stays in main.

Sources: CLAUDE.md:182-186, /home/user/ArcGISRunner/docs/TASKS.md:15

### ISS-59 No fallback if the deployment, auth or hash spike fails

**Type:** risk · **Resolve by:** W0.7 · **Affects:** W0.7, W0.8, W0.9, W2.6

Fallbacks exist only for a failed CSP spike. Record links are needed in v1 and public experiences must work, so a failed deployment, auth or hash spike has no planned path. W2.6 stops if the hash spike fails.

Sources: decision-log:O3, decision-log:D18, decision-log:D21, /home/user/ArcGISRunner/docs/TASKS.md:12-14

### ISS-60 B0.8 depends on a widget build it doesn't list

**Type:** plan-gap · **Resolve by:** B0.8 · **Affects:** B0.8, W0.6, W0.7, B3.7

Part of B0.8's acceptance (registering {APP_URL}/widgets/arcgis-runner/manifest.json, static files served with CORS) needs a widget build in public/widgets/arcgis-runner/. B0.8 depends only on B0.4 and B0.5, not on W0.6, W0.7 or B3.7. The sources also don't say whether W0.7 comes first and feeds DEPLOYMENT.md, since both cover the same hosting and registration steps.

Sources: traceability pass, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14, /home/user/ArcGISRunner/docs/TASKS.md:12

### ISS-61 DEPLOYMENT.md contents differ between CLAUDE.md and TASKS.md

**Type:** conflict · **Resolve by:** B0.8 · **Affects:** B0.8

The builder CLAUDE.md lists the OAuth app but not URL Rewrite / public/web.config. The builder backlog lists URL Rewrite + public/web.config but not the OAuth app. B0.8 covers the union.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:210, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14

### ISS-62 Does DEPLOYMENT.md cover the remaining server env vars

**Type:** open-question · **Resolve by:** B0.8 · **Affects:** B0.8

B0.8 covers CONFIG_ROOT and the OAuth values. The sources don't say whether DEPLOYMENT.md covers setting PORTAL_URL, RUNNER_ALLOWED_ORIGINS and RUNNER_ANON_EDITS_PER_MINUTE on the IIS server.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:196-201, /home/user/arcgisbuilderwebapplication/CLAUDE.md:210, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14

### ISS-63 HTTPS binding not listed for the IIS site

**Type:** open-question · **Resolve by:** B0.8 · **Affects:** B0.8

The deployment spike expects an HTTPS server. The DEPLOYMENT.md item doesn't list a TLS certificate or HTTPS binding.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:12, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14

### ISS-64 Network reach for public, anonymous users

**Type:** open-question · **Resolve by:** B0.8 · **Affects:** B0.8

D17 says the server is reachable wherever Portal users work, and D18 includes public sharing. If public experiences are used outside the org network, the IIS site must be reachable there too, which also exposes the group-gated builder UI. No source says whether Portal or the builder site faces the internet.

Sources: decision-log:D17, decision-log:D18, /home/user/arcgisbuilderwebapplication/CLAUDE.md:130-131

### ISS-65 How the builder app itself is deployed, and server prerequisites

**Type:** open-question · **Resolve by:** B0.8 · **Affects:** B0.8

No source says how the builder app gets onto IIS: who runs Composer install and the Vite build, and where. Server prerequisites beyond PHP FastCGI and URL Rewrite are not listed, including PHP extensions, app pool write access to Laravel's writable folders, and Node for the build.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:188-189, /home/user/arcgisbuilderwebapplication/CLAUDE.md:148-150, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14

### ISS-66 How unmerged builder branches reach the real servers

**Type:** open-question · **Resolve by:** B0.8 · **Affects:** B0.8, B1.1, B1.2, B1.3, B1.5, B2.1, B3.3, B4.1, B4.3, B4.6, B4.7

Builder pull requests that touch sign-in, the share or IIS end with a local checklist against the real servers before merge. No source says how a PR branch gets onto an IIS site for that, or whether it goes on production. Every B1-B4 local checklist starts with this step.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:266-270, /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-127

### ISS-67 Two CORS settings for the same origins

**Type:** risk · **Resolve by:** B0.8 · **Affects:** B0.8, B3.5

Static widget files get CORS from the folder web.config; runtime endpoints get it from RUNNER_ALLOWED_ORIGINS. The same origins live in two places and can drift. If experiences run somewhere other than the Portal host (for example under CSP fallback (b)), both lists need that origin.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:151-154, /home/user/arcgisbuilderwebapplication/CLAUDE.md:200, decision-log:O3

### ISS-68 Laravel server is a single point of failure

**Type:** risk · **Resolve by:** B0.8 · **Affects:** B0.8, B3.5, B3.6, W1.3, W3.4

If the Laravel server is down, every Runner widget stops working, because it serves widget files, profiles and edits. The DEPLOYMENT.md item doesn't cover availability. The widget's 'unreachable server' error only shows when the widget files still load.

Sources: decision-log:O5, /home/user/arcgisbuilderwebapplication/CLAUDE.md:20-26

### ISS-69 Widget rebuild after a Portal upgrade

**Type:** risk · **Resolve by:** B0.8 · **Affects:** B0.8, B3.7, W0.6

After a Portal upgrade the widget must be rebuilt with the matching Developer Edition and redeployed. No source says whether DEPLOYMENT.md covers this step.

Sources: decision-log:O6, CLAUDE.md:68-71

### ISS-70 Widget Portal token: API and server-side use unverified

**Type:** unverified-assumption · **Resolve by:** W0.8 · **Affects:** W0.8, W1.2, W1.3, W3.4, B3.3, B3.4, B3.6

No source names the API that reads the signed-in user's Portal token from the Experience Builder session, or its form. The runtime design assumes Laravel can use that token server-side against Portal 12.0 (identity, webmap item) and feature services (applyEdits). Whether Portal binds it to a referer, client or IP is unverified. How the user gets a test token for B3.3's checklist is unstated.

Sources: CLAUDE.md:84-86, /home/user/ArcGISRunner/docs/TASKS.md:13, /home/user/arcgisbuilderwebapplication/CLAUDE.md:130-138

### ISS-71 Public sharing needed for the anonymous check

**Type:** unverified-assumption · **Resolve by:** W0.8 · **Affects:** W0.8

The auth spike shares only the experience publicly. The sources don't say whether its webmap, layers or the registered widget item must also be public for an anonymous user to load a custom widget in Portal 12.0.

Sources: CLAUDE.md:44, /home/user/ArcGISRunner/docs/TASKS.md:13

### ISS-72 Hash spike: wanted behaviour and test location

**Type:** open-question · **Resolve by:** W0.9 · **Affects:** W0.9, W2.6

The sources don't say which hash parameters Experience Builder 1.18 writes, how Runner should write the hash (which affects reloads and history), or the wanted behaviour on back/forward and page switches. They don't say whether the spike runs in Developer Edition or Portal.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:14, CLAUDE.md:124-126

### ISS-73 W0.9 dependency on W0.7 missing

**Type:** plan-gap · **Resolve by:** W0.9 · **Affects:** W0.9, W0.7

W0.9's steps say that if the hash spike runs in Portal it depends on W0.7's hosting and registration, and its acceptance checks page switches. Its dependsOn lists only W0.6.

Sources: traceability pass

### ISS-74 Hash merge stated as settled but still a spike question

**Type:** conflict · **Resolve by:** W0.9 · **Affects:** W0.9, W2.6

CLAUDE.md states that #runner= merges with Experience Builder's hash parameters and never replaces them. The widget backlog still asks whether the widget can read and write it without either side clobbering it or reloading the page.

Sources: CLAUDE.md:125-126, /home/user/ArcGISRunner/docs/TASKS.md:14

### ISS-75 CSP spike result and fallback choice

**Type:** pending-user-action · **Resolve by:** W0.10 · **Affects:** W0.10, W1.6, W3.6, B4.1

The CSP spike decides whether handlers compiled from text run in Portal-hosted experiences. If blocked, the user picks fallback (a) built-in no-code actions or (b) self-hosted experiences from Developer Edition, a choice deferred until after the spike. Neither fallback is designed. Under (a), B4.1 would be replaced by a task in neither backlog; under (b), the sources don't say whether the editor is still built. W1.6, W3.6 and B4.1 wait on this.

Sources: decision-log:O3, /home/user/ArcGISRunner/docs/TASKS.md:15, /home/user/ArcGISRunner/docs/TASKS.md:24, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47

### ISS-76 Custom JavaScript in v1 scope but gated by a spike

**Type:** conflict · **Resolve by:** W0.10 · **Affects:** W0.10, W1.6, B4.1

Both Project scope sections list custom JavaScript handlers run by the widget as in scope for v1. The widget backlog builds the runner only if the CSP spike passes, and the fallbacks would change v1 scope.

Sources: CLAUDE.md:40, /home/user/arcgisbuilderwebapplication/CLAUDE.md:87, /home/user/ArcGISRunner/docs/TASKS.md:24, decision-log:O3

### ISS-77 CSP may differ by Portal context

**Type:** unverified-assumption · **Resolve by:** W0.10 · **Affects:** W0.10, W1.6, B4.1

The spike tests 'a Portal-hosted experience'. Whether the Content-Security-Policy is the same in the Experience Builder preview, the published experience and anonymous public access in Portal 12.0 is unverified.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:15, CLAUDE.md:106-107

### ISS-78 B1.1 checklist can't run the full sign-in round trip

**Type:** open-question · **Resolve by:** B1.1 · **Affects:** B1.1, B1.2

B1.1 builds the sign-in client but no route, so its checklist only compares PortalClient and its fixtures with the real Portal. The first round trip runs in B1.2. The brain should confirm this split meets the local-checklist rule.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:268-270

### ISS-79 community/self response shape unverified

**Type:** unverified-assumption · **Resolve by:** B1.1 · **Affects:** B1.1, B1.3

The group check relies on /sharing/rest/community/self returning the user's group memberships with ids. Whether it does on Portal 12.0, and which parameters it needs, is unverified.

Sources: decision-log:P2, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18

### ISS-80 Token refresh trigger and expiry handling

**Type:** open-question · **Resolve by:** B1.1 · **Affects:** B1.1, B1.2, B4.7

PortalClient refreshes tokens, but the sources don't say when (before each call, on a 401, on a timer) or what happens when the refresh token expires. Portal 12.0 token lifetimes and Laravel session behaviour under IIS FastCGI are also unverified.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18-19, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:55, /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-127

### ISS-81 /auth/* route list not named

**Type:** open-question · **Resolve by:** B1.2 · **Affects:** B1.2

Only {APP_URL}/auth/callback is named. The route that starts sign-in and whether a sign-out route exists are not stated.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-127, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:19

### ISS-82 Route files, session middleware and CSRF for builder and runtime routes

**Type:** open-question · **Resolve by:** B1.2 · **Affects:** B1.2, B1.3, B1.5, B1.6, B1.7, B2.1, B3.2, B3.5, B3.6

The layout lists routes/web.php and routes/api.php but not which routes go where. The SPA uses same-origin session cookies. Unverified: whether a session-based, group-gated route works in routes/api.php, whether api.php ships with the /api prefix, and how SPA write requests pass CSRF. Runtime routes may need stateless middleware.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:115, /home/user/arcgisbuilderwebapplication/CLAUDE.md:122, /home/user/arcgisbuilderwebapplication/CLAUDE.md:127, /home/user/arcgisbuilderwebapplication/CLAUDE.md:229

### ISS-83 Group check between login and save, and whether autosave counts

**Type:** open-question · **Resolve by:** B1.3 · **Affects:** B1.3, B1.6

Membership is checked at login and on every save/publish. The sources don't say what the middleware does on other builder requests, or whether draft autosave counts as a save, which would add a community/self call per autosave.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:128-129, decision-log:P2

### ISS-84 Not-authorized page and no-session response

**Type:** open-question · **Resolve by:** B1.3 · **Affects:** B1.3

The sources don't say whether the not-authorized page is a Laravel view or SPA route, what it says, how group-gated API calls answer non-members, or what a visitor with no session sees (redirect or error).

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:20, /home/user/arcgisbuilderwebapplication/CLAUDE.md:18

### ISS-85 KindRegistry contents, CrudSettingsValidator owner and runtime handler shape

**Type:** open-question · **Resolve by:** B1.4 · **Affects:** B1.4, B2.4, B3.2, B3.6

KindRegistry maps a kind to a settings validator and runtime handlers. CrudSettingsValidator is in the layout but no backlog item builds it, and Review & Publish uses it. The sources don't say what crud registers in Phase 1, which rules the validator checks (layout fields in fields, readonly rules, pages vs capabilities, input types vs field types), when the server first checks them, or what a runtime handler is and how the edit route dispatches to crud. No-speculative-generality argues against placeholders.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:31-32, /home/user/arcgisbuilderwebapplication/CLAUDE.md:219-221, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:21, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:127-135, CLAUDE.md:208

### ISS-86 SPA kind registry file and entry shape not named

**Type:** open-question · **Resolve by:** B1.4 · **Affects:** B1.4, B1.6

The SPA kind registry has no named file. The crud steps arrive in Phase 2, so the Phase 1 crud entry has no steps to list.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:231-234, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:21, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:26-33

### ISS-87 Profile list behaviour details

**Type:** open-question · **Resolve by:** B1.5 · **Affects:** B1.5

The sources don't say what the list shows (name, kind, webmap, draft or published), what duplicating a published profile produces or names, or what Open does when only a published file exists. No documented action deletes or unpublishes a published profile.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:22

### ISS-88 Task order between list Open, wizard shell and common steps

**Type:** open-question · **Resolve by:** B1.5 · **Affects:** B1.5, B1.6, B1.7

Open on the list page (B1.5) needs a wizard, but the shell is B1.6, which depends on B1.5. B1.6 has no steps to change until B1.7, so autosave and reload checks move to B1.7. The brain should confirm this ordering.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:22-24

### ISS-89 Autosave timing and failure handling

**Type:** open-question · **Resolve by:** B1.6 · **Affects:** B1.6, B4.7

The autosave trigger (each change, a delay, step change) is not stated, nor what the user sees when an autosave fails, for example because the session ended or the group check failed.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:44, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:55

### ISS-90 Later common steps in the Phase 1 shell

**Type:** open-question · **Resolve by:** B1.6 · **Affects:** B1.6

Custom CSS and Review & Publish come in Phase 3 and Custom code in Phase 4. The sources don't say whether the Phase 1 shell shows them before they exist.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:48-58, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:37-47

### ISS-91 Custom JavaScript listed as shared but stored per crud layer

**Type:** conflict · **Resolve by:** B1.6 · **Affects:** B1.6, B4.1

The builder CLAUDE.md lists custom JavaScript among fields shared by every kind and in RunnerProfile's shared fields. CONFIG_OUTPUT_SCHEMA.md's RunnerProfile has no JavaScript field, and customJs sits in the crud LayerConfig.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:36-37, /home/user/arcgisbuilderwebapplication/CLAUDE.md:240, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:14-25, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:61

### ISS-92 Changing kind or webmap after later steps are filled

**Type:** open-question · **Resolve by:** B1.7 · **Affects:** B1.6, B1.7, B2.2

Kind settings depend on the kind and crud layers depend on the webmap. The sources don't say whether either can change after later steps are filled, or what happens to saved layers. The drift warning covers service changes, not a webmap change in the wizard.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:48-51, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:54

### ISS-93 Webmap search call and behaviour unverified

**Type:** unverified-assumption · **Resolve by:** B1.7 · **Affects:** B1.1, B1.7

The PortalClient backlog item lists only authorize URL, code exchange, refresh and community/self, so B1.7 adds webmap search. The Portal 12.0 search API for webmaps the user can access is unverified, and query text, paging and filters are unspecified.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:50, /home/user/arcgisbuilderwebapplication/CLAUDE.md:125, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18

### ISS-94 Source of RunnerProfile.portalUrl

**Type:** open-question · **Resolve by:** B1.7 · **Affects:** B1.7, B3.2

RunnerProfile has portalUrl, but the sources don't say whether it comes from PORTAL_URL or when it is written (webmap step or publish).

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:20, /home/user/arcgisbuilderwebapplication/CLAUDE.md:196

### ISS-95 Which webmap layers count as feature layers

**Type:** open-question · **Resolve by:** B2.1 · **Affects:** B2.1

The sources say 'every feature layer and table' but don't cover other operational layer types or a layer with no service URL, although LayerConfig.url is required.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:61-62, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:28, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:46

### ISS-96 Portal 12.0 webmap and layer JSON shapes unverified

**Type:** unverified-assumption · **Resolve by:** B2.1 · **Affects:** B2.1, B2.3, B2.4

Detection assumes the webmap data lists operational layers (with nested group layers) and tables, and each layer's JSON gives fields, types, geometry type, objectId and globalId fields, domains and edit capabilities. None is verified for Portal 12.0, including how supportsAdd/Update/Delete are derived, how geometry types map to the schema's four values, and what to do with other values.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:28, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:44-81

### ISS-97 Identity used for detection requests

**Type:** unverified-assumption · **Resolve by:** B2.1 · **Affects:** B2.1, B2.3

Detection presumably uses the signed-in user's session token, but no source says so. Unverified: whether every service in a real webmap accepts that token, and whether reported edit capabilities vary by identity, which would make the Pages result differ from what end users get.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:125-127, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:64, decision-log:D5

### ISS-98 Behaviour when a layer schema can't be fetched

**Type:** open-question · **Resolve by:** B2.1 · **Affects:** B2.1, B2.2

The sources don't say what detection or the Layers & fields step does when a schema request fails or the user can't open the service.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:28, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:55

### ISS-99 Detection endpoint path and code location

**Type:** open-question · **Resolve by:** B2.1 · **Affects:** B2.1

The sources place a layers controller under app/Http/Controllers/Builder/ but don't name the route path or method, or say whether detection is crud-specific code under app/Runner/Kinds/Crud/.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:214, /home/user/arcgisbuilderwebapplication/CLAUDE.md:219-229

### ISS-100 Stored layerId must match the id the widget sees

**Type:** unverified-assumption · **Resolve by:** B2.1 · **Affects:** B2.1, W2.1

The widget matches by layerId, then url. This works only if the operational layer id the builder reads from webmap JSON equals the id the widget sees in Experience Builder 1.18, including layers nested in group layers.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:45-46, CLAUDE.md:113-114

### ISS-101 No step sets field labels or layer titles

**Type:** open-question · **Resolve by:** B2.1 · **Affects:** B2.1, B2.2, B2.5, W2.2

FieldConfig.label and LayerConfig.title are required and the widget shows them, but no wizard step edits them and no source names their default.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:48, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:73, /home/user/ArcGISRunner/docs/TASKS.md:30

### ISS-102 Field-level selection in Layers & fields

**Type:** open-question · **Resolve by:** B2.2 · **Affects:** B2.2, B2.4, B2.5, B2.6

'Select which to include' and the user's 'fields and field types, selectable' don't say whether individual fields can be dropped or only whole layers. This decides whether fields holds the full schema and what later steps offer.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:61-63, decision-log:D8, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:53

### ISS-103 No step sets layer order

**Type:** open-question · **Resolve by:** B2.2 · **Affects:** B2.2, W2.2

settings.layers order is the widget's picker order. No step reorders layers and no default order is stated.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:39

### ISS-104 Capabilities snapshot timing

**Type:** open-question · **Resolve by:** B2.3 · **Affects:** B2.1, B2.3, B3.2

capabilities is a 'snapshot at publish', but the Pages step needs capabilities while drafting. The sources don't say whether publish re-fetches them, or what happens to a checked page if capabilities changed.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:64, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:133, CLAUDE.md:139-140

### ISS-105 List and View boxes and default checkbox state

**Type:** open-question · **Resolve by:** B2.3 · **Affects:** B2.3

Only Add, Edit and Delete tie to capabilities. The sources don't say whether List or View can be disabled or which boxes start checked.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:133, /home/user/arcgisbuilderwebapplication/CLAUDE.md:64-65

### ISS-106 Field type names behind the input types table

**Type:** unverified-assumption · **Resolve by:** B2.4 · **Affects:** B2.4, W3.1

The input types table uses plain words (string, integer, small integer, double, single, date, date-only); FieldConfig.type holds esriFieldType* strings. The exact Portal 12.0 names are unverified. An unnamed field type gets only readonly (plus dropdown with a coded domain); confirm this is intended.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:167-174, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:74

### ISS-107 Default input type per field

**Type:** open-question · **Resolve by:** B2.4 · **Affects:** B2.2, B2.4

No source says which input type a field gets before the author picks one.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:66, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:79

### ISS-108 More input options promised by the user

**Type:** pending-user-action · **Resolve by:** B2.4 · **Affects:** B2.4, W3.1

The user said more input options will come later. inputOptions exists in the schema but defines no keys, so renderers have nothing to read. Adding an input type key is a schema change.

Sources: decision-log:D8, /home/user/arcgisbuilderwebapplication/CLAUDE.md:164-165, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:80

### ISS-109 How system fields are recognised

**Type:** unverified-assumption · **Resolve by:** B2.4 · **Affects:** B2.4, B3.6

System fields are objectId, globalId, editor-tracking and Shape__Area/Length. The sources don't say how to recognise editor-tracking fields, and name matching on Shape__Area/Shape__Length may miss fields. They also don't say whether EditGate identifies them at edit time from the profile or live metadata, or shares code with the input types step.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:131-132, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:31

### ISS-110 Fields the service reports as not editable

**Type:** open-question · **Resolve by:** B2.4 · **Affects:** B2.4

A field is editable only if editable is true and inputType isn't readonly. The sources don't say whether the input types step forces readonly for editable: false fields or still offers other keys.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:76, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:130-131

### ISS-111 Where the SPA gets the valid-for mapping

**Type:** open-question · **Resolve by:** B2.4 · **Affects:** B2.4, W3.1

Input type keys live in InputTypes.php and must match the widget's renderers. The sources don't say whether the SPA reads the valid-for mapping from the server or keeps a TypeScript copy, which would add a third list that can drift.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:164-165

### ISS-112 Drag reorder component not named

**Type:** unverified-assumption · **Resolve by:** B2.5 · **Affects:** B2.5, B2.6

The Designer needs drag reordering. The sources name Calcite but no drag-and-drop library; whether the Calcite version in use supports drag reordering is unverified.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:67-68, /home/user/arcgisbuilderwebapplication/CLAUDE.md:121-122

### ISS-113 Layouts for pages that are turned off

**Type:** open-question · **Resolve by:** B2.5 · **Affects:** B2.5, B2.6

layouts requires list, add, edit and view. The sources don't say whether the Designer shows or stores a layout for a page turned off.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:55-60

### ISS-114 Starting layouts and form layout rules

**Type:** open-question · **Resolve by:** B2.5 · **Affects:** B2.5, B2.6

No source gives starting columns, sort or sections for a new layer. Open: whether a field can sit in more than one section, can be left off a form, or whether Add must include non-nullable fields.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:67-68, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:75, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:92-94

### ISS-115 List page size limits and sort field rule

**Type:** open-question · **Resolve by:** B2.5 · **Affects:** B2.5

The only stated page size value is the default 25. No minimum or maximum, and no rule on whether sortField must be a column.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:87-89

### ISS-116 Custom CSS preview: what it renders and how it is isolated

**Type:** open-question · **Resolve by:** B3.1 · **Affects:** B3.1

The preview shows the widget only. The sources don't say what it renders, with what sample content, how unscoped rules are kept off the builder's own page, or whether the note explains .arcgis-runner and [data-profile] targeting. Whether the SPA can render the jimu-based widget or needs a stand-in is unverified.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:52-55, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:37, CLAUDE.md:190-192

### ISS-117 B3.1 cites a source that doesn't support it

**Type:** plan-gap · **Resolve by:** B3.1 · **Affects:** B3.1

B3.1 cites builder docs/TASKS.md:29 (the Layers & fields step), which doesn't support the Custom CSS step and only seems to justify the dependency on B2.2.

Sources: traceability pass

### ISS-118 How validation failures reach the author

**Type:** open-question · **Resolve by:** B3.2 · **Affects:** B3.2

Review & Publish validates via the kind validator, but the sources don't say how a failure is shown or whether the validator's errors are returned to the step.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:58, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38

### ISS-119 customJs and phpHook before their Phase 4 UI

**Type:** open-question · **Resolve by:** B3.2 · **Affects:** B3.2, B4.1, B4.2, B4.4

LayerConfig requires customJs and phpHook, and phpHook must be a HookRegistry key or null. Review & Publish lands in Phase 3, before the Custom code step and HookRegistry in Phase 4. The sources don't say what Phase 3 writes for these fields or how phpHook is validated before HookRegistry exists.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:61-62, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47-49

### ISS-120 Draft lifecycle after publish

**Type:** open-question · **Resolve by:** B3.2 · **Affects:** B3.2

The sources don't say whether drafts/{profileId}.json is kept, cleared or replaced after publish, how an author edits a published profile before republishing, or whether unpublishing exists.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:44-46, /home/user/arcgisbuilderwebapplication/CLAUDE.md:143-147

### ISS-121 How ResolvePortalIdentity checks a token and handles a bad one

**Type:** open-question · **Resolve by:** B3.3 · **Affects:** B3.3

The sources don't say how the token is checked (for example via community/self) or what happens with an invalid or expired token. Laravel must never add its own sign-in gate for widget users.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:130-133, /home/user/arcgisbuilderwebapplication/CLAUDE.md:218

### ISS-122 B3.3 missing dependency on the auth spike

**Type:** plan-gap · **Resolve by:** B3.3 · **Affects:** B3.3, W0.8

B3.3 consumes the token the widget sends as Authorization: Bearer, which W0.8 establishes, and its local checklist needs a real widget-sent token. B3.3 doesn't depend on W0.8.

Sources: traceability pass

### ISS-123 No task holds the real-Portal WebmapAccess check

**Type:** plan-gap · **Resolve by:** B3.4 · **Affects:** B3.3, B3.4, B3.5

B3.4 is cloud tests only and defers its real-Portal check to B3.5's local checklist, but B3.5's steps and acceptance are all Pest/Http::fake() and never name a WebmapAccess check (one checklist line mentions it). The sources don't say whether runtime token handling or WebmapAccess count as 'touching sign-in' for the local-checklist rule. A wrong assumption about Portal item responses would surface after B3.4 merges.

Sources: traceability pass, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-270

### ISS-124 Portal item request and how Portal 12.0 reports no access

**Type:** unverified-assumption · **Resolve by:** B3.4 · **Affects:** B3.4

The exact REST endpoint for the webmap item, how the token is passed, and how Portal signals no access (HTTP status or error body in a 200) are unverified.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:134-136

### ISS-125 WebmapAccess cache duration, key and store

**Type:** open-question · **Resolve by:** B3.4 · **Affects:** B3.4

The answer is cached 'briefly', with no duration. The sources don't say the key includes the identity, or which store is used. A key without the identity would serve one identity's answer to another.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:135-136, /home/user/arcgisbuilderwebapplication/CLAUDE.md:226

### ISS-126 Listing endpoint fields differ

**Type:** conflict · **Resolve by:** B3.5 · **Affects:** B3.5, W1.2

The builder CLAUDE.md says the listing returns id, name, kind, webmapId. CONFIG_OUTPUT_SCHEMA.md says Pick<RunnerProfile, 'id' | 'name' | 'kind' | 'webmapId' | 'publishedAt'>. CLAUDE.md calls the schema doc the only definition of RunnerProfile but doesn't say which listing shape wins.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:155-156, /home/user/arcgisbuilderwebapplication/CLAUDE.md:241-242, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:32-33

### ISS-127 Runtime error responses not defined

**Type:** open-question · **Resolve by:** B3.5 · **Affects:** B3.5, B3.6, W1.2, W1.3

The sources don't give status codes or bodies for a caller who can't open the webmap, an unknown profileId, a missing webmapId, or a server error. The widget must show 'the service's message' when sign-in is needed, but doesn't know where it comes from or how to tell an unreachable server from an HTTP error, or what to show when profileId is empty or no map is connected.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:154-159, CLAUDE.md:87-88, CLAUDE.md:97-98

### ISS-128 How the listing finds profiles for a webmap

**Type:** open-question · **Resolve by:** B3.5 · **Affects:** B3.5

The sources don't say how the listing endpoint finds published profiles for a webmapId. ProfileStore is described only as drafts + published with atomic writes and slug ids.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:143-147, /home/user/arcgisbuilderwebapplication/CLAUDE.md:227

### ISS-129 When a republished profile reaches the widget

**Type:** open-question · **Resolve by:** B3.5 · **Affects:** B3.5, W1.3

Republishing must change experiences with no rebuild. The sources don't say what cache headers the profile endpoints send, whether the widget fetches the profile once per page load or again in a session, or whether browser or IIS caching may delay it.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:45-46, /home/user/arcgisbuilderwebapplication/CLAUDE.md:111, CLAUDE.md:64

### ISS-130 CORS mechanism and preflight on IIS

**Type:** unverified-assumption · **Resolve by:** B3.5 · **Affects:** B3.5, B3.6, W3.4

The sources don't name how CORS is applied to runtime routes. The Authorization header triggers an OPTIONS preflight; whether IIS passes it to Laravel and the edit endpoint allows the header is unverified. The backlog puts '+ CORS' on the GET endpoints only, not the edit endpoint.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:116-118, /home/user/arcgisbuilderwebapplication/CLAUDE.md:154, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41-42

### ISS-131 Developer Edition origin and RUNNER_ALLOWED_ORIGINS

**Type:** open-question · **Resolve by:** B3.5 · **Affects:** B3.5, B3.6, W1.2, W1.3, W2.1, W3.4

Runtime endpoints allow only RUNNER_ALLOWED_ORIGINS, normally the Portal host. Local Developer Edition work calls the builder app from its own origin via builderBaseUrl. The sources don't say whether that origin is added, in which environment, or how a Developer Edition session gets a Portal token. Every W1-W3 local checklist depends on this.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:154, /home/user/arcgisbuilderwebapplication/CLAUDE.md:200, CLAUDE.md:82-83

### ISS-132 Runtime routes stay ungated is only testable from Phase 3

**Type:** open-question · **Resolve by:** B3.5 · **Affects:** B1.3, B3.5

B1.3 keeps builder middleware off runtime routes, but none exist until Phase 3. The check belongs in the runtime endpoint tasks.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41-42

### ISS-133 Edit endpoint request and response contract

**Type:** open-question · **Resolve by:** B3.6 · **Affects:** B3.6, B4.3, W3.4, W3.5

No source defines the edit endpoint's body (operation, attributes, geometry, feature key, one or several operations per request) or responses (success, EditGate rejection, HookRejected message, feature-service error, sign-in needed, rate limit). It is a cross-repo contract; whether it belongs in CONFIG_OUTPUT_SCHEMA.md is unstated. Hook call granularity depends on it.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, CLAUDE.md:135-138, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:17

### ISS-134 EditGate handling of disallowed fields

**Type:** open-question · **Resolve by:** B3.6 · **Affects:** B3.6

The sources don't say whether EditGate rejects a whole edit or drops a non-editable field, whether a field must appear in the add/edit layout to be editable, or how fields absent from the profile's fields are treated.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:127-135

### ISS-135 Server-side recheck of live capabilities

**Type:** open-question · **Resolve by:** B3.6 · **Affects:** B3.6

EditGate checks pages against the publish-time snapshot. The widget CLAUDE.md says to recheck live capabilities. The sources don't say whether the server rechecks before applyEdits or relies on the service to refuse.

Sources: CLAUDE.md:139-140, /home/user/arcgisbuilderwebapplication/CLAUDE.md:137-140

### ISS-136 Whether the edit endpoint checks WebmapAccess

**Type:** open-question · **Resolve by:** B3.6 · **Affects:** B3.6

The sources don't say whether the edit endpoint also requires the caller to be able to open the profile's webmap.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:134-140

### ISS-137 Client IP seen by Laravel under IIS

**Type:** unverified-assumption · **Resolve by:** B3.6 · **Affects:** B3.6

The per-IP limit depends on the client IP Laravel sees under IIS FastCGI. A proxy or load balancer in front could make all clients share one IP.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:116-118, /home/user/arcgisbuilderwebapplication/CLAUDE.md:141-142

### ISS-138 Edit endpoint open to anonymous callers

**Type:** risk · **Resolve by:** B3.6 · **Affects:** B3.6

The edit endpoint is open whenever a public editable layer is behind it. The per-IP rate limit is the only extra guard named.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:141-142

### ISS-139 Edits run without PHP hooks until Phase 4

**Type:** risk · **Resolve by:** B3.6 · **Affects:** B3.6, B4.3, W3.4, W3.5

The edit endpoint is described with before*/after* hooks, but hooks land in builder Phase 4. Between B3.6 and B4.3 edits run without hooks, so 'every edit runs the layer's PHP hook' isn't met, and W3.4/W3.5 hook-message checks can't run.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:110, /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:42, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:48-49

### ISS-140 Deploy script form, location and where it runs

**Type:** open-question · **Resolve by:** B3.7 · **Affects:** B3.7

The sources don't say what the deploy script is written in, where it lives, which machine runs it (Developer Edition is on the user's machine, the app on IIS), how it reaches the IIS public folder, or where usage is documented.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:148-150, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, decision-log:D13, decision-log:D22

### ISS-141 Where the widget folder's web.config comes from

**Type:** open-question · **Resolve by:** B3.7 · **Affects:** B0.8, B3.7

public/widgets/arcgis-runner/ is gitignored and filled by the copy, yet its CORS web.config must sit there. The sources don't say whether it is committed via a gitignore exception, written by the script or placed by hand, or whether the script clears the folder first (removing a hand-placed file) or not (leaving old files).

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:148-153, /home/user/arcgisbuilderwebapplication/CLAUDE.md:230, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14

### ISS-142 .gitignore lacks public/widgets/arcgis-runner/

**Type:** stale-code · **Resolve by:** B3.7 · **Affects:** B3.7, B0.5

The builder CLAUDE.md says the deployed widget folder is gitignored, but the builder .gitignore lists only node_modules/, dist/, vendor/, .env, *.log and .DS_Store. No task other than B3.7 says who adds it.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:230, /home/user/arcgisbuilderwebapplication/.gitignore:1-6

### ISS-143 B3.7 depends on the wrong widget task

**Type:** plan-gap · **Resolve by:** B3.7 · **Affects:** B3.7, W0.5

B3.7 depends on W0.5 (cloud test harness). Its sources cite widget TASKS.md:11-12, which are W0.6 and W0.7, already listed. No source ties the deploy script to the Vitest harness. This is the one real spike/task id mismatch; the earlier section issues claiming other sections cite wrong spike ids are stale.

Sources: traceability pass, /home/user/ArcGISRunner/docs/TASKS.md:10-12

### ISS-144 Stale widget config.json and config.ts

**Type:** stale-code · **Resolve by:** W1.1 · **Affects:** W1.1

config.json holds useMapWidgetIds, layers and defaultPageSize, and src/config.ts defines per-layer RunnerLayerConfig (visibleFields, editableFields, fieldLabels, sort, pageSize). The current docs define the widget config as { profileId, builderBaseUrl? }, with per-layer settings coming from the profile.

Sources: /home/user/ArcGISRunner/widgets/arcgis-runner/config.json:1-5, /home/user/ArcGISRunner/widgets/arcgis-runner/src/config.ts:3-20, CLAUDE.md:154, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:3-10, decision-log:O9

### ISS-145 CONFIG_SCHEMA.md described as 'dev override only'

**Type:** conflict · **Resolve by:** W1.1 · **Affects:** W1.1

The repo layout says docs/CONFIG_SCHEMA.md covers config.json '(dev override only)'. It also defines profileId, which the settings dropdown sets in normal use.

Sources: CLAUDE.md:147, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:9-10

### ISS-146 builderBaseUrl override and config.json defaults

**Type:** open-question · **Resolve by:** W1.1 · **Affects:** W1.1

config.json is committed and deployed. The sources don't say how a local builderBaseUrl stays out of the deployed build, what profileId's default is, or whether the committed file carries builderBaseUrl. Whether config.json changes reach Runner instances already placed in an experience is unverified.

Sources: CLAUDE.md:82-83, CLAUDE.md:151, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:8-11

### ISS-147 Stale settings panel placeholder

**Type:** stale-code · **Resolve by:** W1.2 · **Affects:** W1.2

setting.tsx shows a per-layer field configuration placeholder and says that config lands in Phase 4. The current design puts only a map selector and profile dropdown in settings, and widget Phase 4 is Polish.

Sources: /home/user/ArcGISRunner/widgets/arcgis-runner/src/setting/setting.tsx:6-7, /home/user/ArcGISRunner/widgets/arcgis-runner/src/setting/setting.tsx:22-25, CLAUDE.md:155, /home/user/ArcGISRunner/docs/TASKS.md:20, /home/user/ArcGISRunner/docs/TASKS.md:44

### ISS-148 Where the widget's profile types live

**Type:** conflict · **Resolve by:** W1.2 · **Affects:** W1.2, W1.3, W1.5, W3.1, W3.6

The widget CLAUDE.md says not to redefine the profile contract here. The schema doc says every schema change must be mirrored in the widget once it ships. Widget code needs TypeScript types for the listing, RunnerProfile, FieldConfig, LayerConfig, FormLayout and CrudJsContext under strict mode. No source says whether they are copied, generated or imported, or how they stay in step.

Sources: CLAUDE.md:19-20, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:4-6, /home/user/arcgisbuilderwebapplication/CLAUDE.md:279-280

### ISS-149 Where shared request and auth code lives

**Type:** open-question · **Resolve by:** W1.2 · **Affects:** W1.1, W1.2, W1.3, W3.4

The settings panel and runtime shell both call builder endpoints with the derived base URL and optional Bearer token. The sources don't say where shared request code lives or which task builds it.

Sources: CLAUDE.md:155, CLAUDE.md:158, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:14-17

### ISS-150 Reading the connected map's webmap id

**Type:** unverified-assumption · **Resolve by:** W1.2 · **Affects:** W1.2, W1.3

Settings needs the connected Map widget's webmap item id for the listing; the runtime needs it for the mismatch check. The Experience Builder 1.18 calls that expose it are not documented or tested.

Sources: CLAUDE.md:89-91, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:19

### ISS-151 Can the settings panel run anonymously

**Type:** open-question · **Resolve by:** W1.2 · **Affects:** W1.2

CONFIG_SCHEMA states the token rule (Bearer when signed in, none when anonymous) for every call. The settings panel runs in builder mode; the sources don't say whether an anonymous author can exist there.

Sources: /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:14, CLAUDE.md:84-86

### ISS-152 Settings panel states unspecified

**Type:** open-question · **Resolve by:** W1.2 · **Affects:** W1.2

The sources don't say what the panel shows with no map connected, an empty listing, or a failed request; which listing field the dropdown displays; or what happens to a stored profileId when a different map is connected.

Sources: CLAUDE.md:13-14, CLAUDE.md:89-91, /home/user/ArcGISRunner/docs/TASKS.md:20

### ISS-153 Meaning of 'multiple Map widgets'

**Type:** open-question · **Resolve by:** W1.2 · **Affects:** W1.2, W4.4

The deferral could mean one Runner connected to several Map widgets, or any experience with more than one. CLAUDE.md allows two Runners on one page but says nothing about their map connections, or what settings does if an author selects more than one. The placeholder widget.tsx renders only when exactly one map id is connected.

Sources: CLAUDE.md:54, CLAUDE.md:128-129, /home/user/ArcGISRunner/docs/TASKS.md:50, /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:18

### ISS-154 Where widget UI strings go before Phase 4 i18n

**Type:** open-question · **Resolve by:** W1.2 · **Affects:** W1.2, W1.3, W2.2, W2.3, W3.2, W3.5

W1-W3 add user-facing text (error states, 'update the Runner widget', screen text). translations/default.ts exists but i18n is a Phase 4 item. The sources don't say whether new strings go into that file now.

Sources: CLAUDE.md:164, /home/user/ArcGISRunner/docs/TASKS.md:46

### ISS-155 Portal-hosted checks wait on deployment

**Type:** risk · **Resolve by:** W1.2 · **Affects:** W1.2, W1.3, W2.3, W2.4, W2.6

The derived-base-URL check (W1.2), anonymous public-experience check (W1.3), and signed-out/no-access checks (W2.3, W2.4, W2.6) need the widget deployed from the builder app, registered in Portal 12.0, and in a public experience. They are deferred until W0.7, W0.8 and B3.7 land, so these tasks may merge before the checks run.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:12-13, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43, CLAUDE.md:201-202

### ISS-156 Stale widget.tsx comments and placeholder

**Type:** stale-code · **Resolve by:** W1.3 · **Affects:** W1.3

widget.tsx describes layer auto-detection in the widget ('Layer detection not implemented yet') and points to old 'Phase 1-3' numbering. Detection now happens in the builder wizard, and the widget matches profile layers by layerId/url. Widget phases are now Shell / List & View / Add, Edit, Delete / Polish.

Sources: /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:5-8, /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:24-25, CLAUDE.md:113-114, /home/user/ArcGISRunner/docs/TASKS.md:17

### ISS-157 Stale translation strings

**Type:** stale-code · **Resolve by:** W1.3 · **Affects:** W1.3, W2.2, W3.2, W3.5, W4.1

translations/default.ts follows the old design: a connect-map prompt, Add/Edit/Delete only, and 'No editable layers or tables were found in the connected map.' It has no strings for View, profile loading, unknown kind, webmap mismatch or unreachable server. No task is assigned to remove the old keys.

Sources: /home/user/ArcGISRunner/widgets/arcgis-runner/src/translations/default.ts:1-9, CLAUDE.md:97-98, /home/user/arcgisbuilderwebapplication/CLAUDE.md:38-40, decision-log:O9

### ISS-158 What Experience Builder shows when Runner's files can't load

**Type:** unverified-assumption · **Resolve by:** W1.3 · **Affects:** W1.3

The sources don't say what Experience Builder 1.18 or Portal 12.0 shows in place of a registered custom widget whose files are unreachable.

Sources: decision-log:O5

### ISS-159 Map connection and unknown-kind message split between W1.3 and W1.4

**Type:** plan-gap · **Resolve by:** W1.3 · **Affects:** W1.3, W1.4

Shell step 1 (JimuMapViewComponent) has no backlog item; the plan puts it in W1.3. W1.3's acceptance requires the 'update the Runner widget' message for unknown kinds, which W1.4 (the registry, depending on W1.3) also owns. The sources don't say whether a known kind means one in the registry, or what the crud module holds before W2.

Sources: traceability pass, CLAUDE.md:96-98, CLAUDE.md:108, /home/user/ArcGISRunner/docs/TASKS.md:21-22

### ISS-160 Lazy-loaded files in the Developer Edition build

**Type:** unverified-assumption · **Resolve by:** W1.4 · **Affects:** W1.4

It is unverified that the Developer Edition 1.18 build splits a custom widget's lazy modules into separate files, and that those load from the IIS-hosted folder under its CORS web.config.

Sources: CLAUDE.md:108, CLAUDE.md:75-78, /home/user/arcgisbuilderwebapplication/CLAUDE.md:151-153

### ISS-161 Widget root class doesn't match the docs

**Type:** stale-code · **Resolve by:** W1.5 · **Affects:** W1.5

widget.tsx renders the root as className="widget-arcgis-runner p-2". The docs require class="arcgis-runner" and data-profile="{id}", which profile CSS relies on to target Runner alone.

Sources: /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:17, CLAUDE.md:102-104, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:29-30

### ISS-162 Custom CSS tag details

**Type:** open-question · **Resolve by:** W1.5 · **Affects:** W1.5

The sources don't say whether a reused style tag's content is replaced when the profile's customCss differs on a later load in the same session, or whether an empty customCss produces a tag.

Sources: CLAUDE.md:99-102, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:28-29

### ISS-163 Experience-wide CSS waits for a Runner to load

**Type:** risk · **Resolve by:** W1.5 · **Affects:** W1.5

Profile CSS applies only once a Runner with that profile loads. If Experience Builder doesn't load widgets on unopened pages, pages viewed earlier lack the CSS.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:52-53, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:28-29

### ISS-164 No DOM test environment named for Vitest

**Type:** open-question · **Resolve by:** W1.5 · **Affects:** W1.5

CSS injection tests need document.head. The harness lists TypeScript, Vitest and @arcgis/core but no DOM environment.

Sources: CLAUDE.md:148, /home/user/ArcGISRunner/docs/TASKS.md:10

### ISS-165 JS runner behaviour undefined

**Type:** open-question · **Resolve by:** W1.6 · **Affects:** W1.6

The sources don't say how the runner handles compile errors or thrown exceptions, whether handlers may be async, or whether a handler compiles once per profile or per call. CLAUDE.md puts the runner in the shell while the cloud rule puts logic in lib/; the split is unstated.

Sources: CLAUDE.md:105-107, CLAUDE.md:193-195, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:114-124

### ISS-166 W1.6 has no real caller to test against

**Type:** risk · **Resolve by:** W1.6 · **Affects:** W1.6, W3.6

Nothing calls the runner until W3.6, and profiles carry handler text only after B4.1. W1.6 is verified only by Vitest with a stub ctx and a build check. W3.6 is the first point a real profile handler can run in Portal.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:42, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47

### ISS-167 Test webmap and profile for widget checklists

**Type:** pending-user-action · **Resolve by:** W2.1 · **Affects:** W2.1, W2.3, W3.3, W3.4, W3.5

W2 and W3 checklists need a published crud profile on a reachable builder app, for a webmap with point, multipoint, polyline and polygon layers and a table, some with a GlobalID field and some without. The sources don't name such a webmap.

Sources: CLAUDE.md:62, CLAUDE.md:198-200, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41

### ISS-168 Profile layers that match no map layer

**Type:** open-question · **Resolve by:** W2.1 · **Affects:** W2.1, W2.2

The sources don't say whether Runner hides an unmatched profile layer, shows it with an error, or fails the profile. The drift warning is builder-only.

Sources: CLAUDE.md:113-114, /home/user/ArcGISRunner/docs/TASKS.md:28, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:54

### ISS-169 How the url fallback compares URLs

**Type:** open-question · **Resolve by:** W2.1 · **Affects:** W2.1

The sources don't say how URLs compare on case, trailing slash, http vs https, query strings or layer index.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:46, CLAUDE.md:113-114

### ISS-170 Map widget data source API, and tables without layer views

**Type:** unverified-assumption · **Resolve by:** W2.1 · **Affects:** W2.1, W2.3, W2.5, W2.6

The jimu-arcgis 1.18 API that reaches the Map widget's data sources from a JimuMapView is not named. It is unverified that selecting through a data source makes the map highlight the feature, and whether data sources exist for webmap tables, which have no layer view.

Sources: CLAUDE.md:114-116, /home/user/ArcGISRunner/docs/TASKS.md:28

### ISS-171 What pages.list and pages.view = false mean in the widget

**Type:** open-question · **Resolve by:** W2.2 · **Affects:** W2.2, W2.3, W2.4, W2.5, W2.6

The sources don't say whether a layer with list or view off appears in the picker, whether map clicks or links can still open View, or what screen shows.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:54, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:133

### ISS-172 Layer picker label and starting screen

**Type:** open-question · **Resolve by:** W2.2 · **Affects:** W2.2

The sources give only picker order. They don't say what label each entry shows (title is likely but unstated), or which layer and screen Runner shows on load without a record link.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:39-40, /home/user/ArcGISRunner/docs/TASKS.md:29, CLAUDE.md:117-118

### ISS-173 Which Calcite components jimu-ui 1.18 wraps

**Type:** unverified-assumption · **Resolve by:** W2.2 · **Affects:** W2.2, W2.3, W3.1, W3.5

It is unverified which Calcite list, table, pagination, input, select, date picker and dialog components jimu-ui 1.18 wraps, and how unwrapped ones load inside the widget.

Sources: CLAUDE.md:72-73

### ISS-174 Read path for List and View

**Type:** open-question · **Resolve by:** W2.3 · **Affects:** W2.3, W2.4, W2.6

Reads use queryFeatures to the feature service, and selection reuses Map widget data sources. The sources don't say whether queries run on the layer or the data source, or whether List honours a webmap layer filter.

Sources: CLAUDE.md:114-116, CLAUDE.md:135

### ISS-175 How the user opens View from List

**Type:** open-question · **Resolve by:** W2.3 · **Affects:** W2.3, W2.4

Selecting a row only 'highlights and zooms'. The sources don't say how View opens from List (row click, button, both).

Sources: /home/user/ArcGISRunner/docs/TASKS.md:30, CLAUDE.md:45, CLAUDE.md:117-118

### ISS-176 Row selection and links for tables

**Type:** open-question · **Resolve by:** W2.3 · **Affects:** W2.3, W2.6

Tables have no geometry. The sources don't say what selecting a table row does instead of highlight and zoom, or what a record link to a table record does.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:30, /home/user/ArcGISRunner/docs/TASKS.md:33, CLAUDE.md:127-128

### ISS-177 List order and value display

**Type:** open-question · **Resolve by:** W2.3 · **Affects:** W2.3, W2.4

The sources don't say how List is ordered when sortField or sortOrder is absent, or how List and View display coded-value domains, dates, date-only values and nulls.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:74, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:87-88

### ISS-178 Paged and sorted queries unverified

**Type:** unverified-assumption · **Resolve by:** W2.3 · **Affects:** W2.3

Maps SDK 4.33 paging and sort parameters, whether every Portal 12.0 service supports paged queries, and how the pager learns the total count are unverified.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:30, CLAUDE.md:135

### ISS-179 Selections made outside Runner

**Type:** open-question · **Resolve by:** W2.3 · **Affects:** W2.3, W2.5

Selection 'syncs with the map and other widgets'. The sources don't say whether a selection made elsewhere changes what Runner shows.

Sources: CLAUDE.md:114-116

### ISS-180 W2.4's checklist has no way into View

**Type:** plan-gap · **Resolve by:** W2.4 · **Affects:** W2.4

W2.4 depends only on W2.2, but its checklist opens a feature's View. The only ways in are List (W2.3, method open), map click (W2.5) or links (W2.6).

Sources: /home/user/ArcGISRunner/docs/TASKS.md:30-33

### ISS-181 Phase 2 backlog items act on Phase 3 screens

**Type:** conflict · **Resolve by:** W2.5 · **Affects:** W2.5, W2.6, W3.2, W3.3

The Phase 2 map-click task includes 'disabled on Add/Edit' and the unsaved-changes prompt; the record-links task covers edit links and a Copy link button on Edit. Add/Edit forms arrive in Phase 3. CLAUDE.md states the prompt more generally. The plan has W2.5/W2.6 own the logic and W3.2 plug in, while W3 text describes W3.2 building the Add/Edit side. The brain confirms the split and updates TASKS.md.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:32-33, /home/user/ArcGISRunner/docs/TASKS.md:38, CLAUDE.md:120-122, CLAUDE.md:124-130

### ISS-182 Unsaved-changes prompt and pick list content

**Type:** open-question · **Resolve by:** W2.5 · **Affects:** W2.5, W3.2

The sources don't say what the prompt shows or offers, or whether it covers leaving the page or closing the tab. They don't say what each pick-list entry shows or how many 'short' allows.

Sources: CLAUDE.md:120-122, /home/user/ArcGISRunner/docs/TASKS.md:32

### ISS-183 Limiting hitTest without touching the popup

**Type:** unverified-assumption · **Resolve by:** W2.5 · **Affects:** W2.5

Which Maps SDK 4.33 hitTest options limit hits to given layers, and how a widget listens to clicks without changing the Map widget popup, are unverified.

Sources: CLAUDE.md:119-123

### ISS-184 Two Runners on one page: clicks and shared profiles

**Type:** open-question · **Resolve by:** W2.5 · **Affects:** W2.5, W2.6

Only link handling has a rule for two Runners. The sources don't say what happens on a map click when both include the layer, or on a link when both use the same profile.

Sources: CLAUDE.md:128-129

### ISS-185 When Runner writes and clears the hash

**Type:** open-question · **Resolve by:** W2.6 · **Affects:** W2.6

The sources don't say what #runner= holds on List or Add, when it is cleared, whether writes add history entries, or whether Runner reacts to hash changes after load.

Sources: CLAUDE.md:124-125, /home/user/ArcGISRunner/docs/TASKS.md:14, /home/user/ArcGISRunner/docs/TASKS.md:33

### ISS-186 Escaping in the record link format

**Type:** open-question · **Resolve by:** W2.6 · **Affects:** W2.6

The format uses ':' as a separator. The sources don't say how ids containing ':' or URL-unsafe characters are encoded. GlobalIDs are often braced; that form is unverified for Portal 12.0.

Sources: CLAUDE.md:124-127

### ISS-187 Record links that can't be followed

**Type:** open-question · **Resolve by:** W2.6 · **Affects:** W2.6, W3.2

The sources don't say what Runner shows when the link's layer isn't in the profile or map, the feature doesn't exist, or an edit link meets pages.edit false or no update support.

Sources: CLAUDE.md:88, CLAUDE.md:130, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:64-68

### ISS-188 When schema v1 freezes

**Type:** open-question · **Resolve by:** W3.1 · **Affects:** W3.1, W3.2, W3.4, W3.6

v1 can change freely until the widget ships, then changes bump schemaVersion. No source says what 'ships' means (a W3 merge, Phase 4 end, first registration). W3 builds against FieldConfig, FormLayout and CrudJsContext, so a v1 change can break merged work.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:4-6, /home/user/arcgisbuilderwebapplication/CLAUDE.md:279-280

### ISS-189 Range domain, length and nullable handling in forms

**Type:** open-question · **Resolve by:** W3.1 · **Affects:** W3.1, W3.2

FieldConfig carries nullable, length and RangeDomain. The sources don't say whether renderers or forms enforce them, such as requiring a non-nullable field before save.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:75-78, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:127-135

### ISS-190 Date value format unverified

**Type:** unverified-assumption · **Resolve by:** W3.1 · **Affects:** W3.1, W3.4

The value types queryFeatures returns for date and date-only fields, the format the edit request should carry, and time zone handling are unverified for Maps SDK 4.33 and Portal 12.0.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:171-172

### ISS-191 Widget re-check of system fields

**Type:** open-question · **Resolve by:** W3.1 · **Affects:** W3.1

System fields are always readonly and the builder enforces it. The sources don't say whether the widget also detects them from the live layer or trusts the profile's inputType.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:131-132, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:31

### ISS-192 Renderers can't be seen until forms exist

**Type:** risk · **Resolve by:** W3.1 · **Affects:** W3.1, W3.2

W3.1 adds renderers before W3.2 adds the forms, so W3.1's checklist only confirms the build and the visual check moves to W3.2.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:37-38

### ISS-193 Live capability check unverified

**Type:** unverified-assumption · **Resolve by:** W3.2 · **Affects:** W3.2, W3.5

Which property exposes live add/update/delete support on the layer or Map widget data source in Maps SDK 4.33 and Experience Builder 1.18 is unverified. The sources don't say how the widget tells the user when snapshot and live layer disagree.

Sources: CLAUDE.md:139-140, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:64-68

### ISS-194 Layout fields missing from fields or the live service

**Type:** open-question · **Resolve by:** W3.2 · **Affects:** W3.2

The sources don't say what a form does when a layout names a field not in fields or no longer in the service.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:129, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:54

### ISS-195 Add starting values, entry points and post-action navigation

**Type:** open-question · **Resolve by:** W3.2 · **Affects:** W3.2, W3.4, W3.5

The sources don't say what values a new feature starts with (blank, defaults, templates), where Add appears, how Edit opens, whether Delete on List is per row or on a selection, or which screen shows after a save or delete.

Sources: CLAUDE.md:117-118, /home/user/ArcGISRunner/docs/TASKS.md:38, /home/user/ArcGISRunner/docs/TASKS.md:41

### ISS-196 SketchViewModel details and drawing steps

**Type:** unverified-assumption · **Resolve by:** W3.3 · **Affects:** W3.3

The SketchViewModel create and update tools for the four geometry types in Maps SDK 4.33, its graphics layer, and how to reach the map view from JimuMapView in 1.18 are unverified. The sources don't say whether Add can save without geometry, or how drawing starts and finishes.

Sources: CLAUDE.md:133-134, /home/user/ArcGISRunner/docs/TASKS.md:39

### ISS-197 Anonymous edit check in a Developer Edition preview

**Type:** unverified-assumption · **Resolve by:** W3.4 · **Affects:** W3.4

W3.4's checklist runs an anonymous edit in Developer Edition. Whether a 1.18 local preview can run unsigned is unverified; otherwise the check needs a Portal-hosted public experience (W0.7, B3.7).

Sources: /home/user/ArcGISRunner/docs/TASKS.md:12-13, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43

### ISS-198 beforeDelete timing and ctx.page for Delete

**Type:** open-question · **Resolve by:** W3.5 · **Affects:** W3.5, W3.6

CrudJsContext.page is a PageKey that includes delete, but Delete is an action. The sources don't say which page value beforeDelete gets, whether it fires before or after confirmation, or whether onPageLoad can carry delete.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:42, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:103-124, CLAUDE.md:117-118

### ISS-199 Handler behaviour gaps

**Type:** open-question · **Resolve by:** W3.6 · **Affects:** W3.6

The sources don't say where cancel(message) text appears, whether handlers can be async, what happens when one throws, what setValue does on List or View, or whether setValue fires onFieldChange again.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:106-124

### ISS-200 Who builds the Custom code step if the JS editor is blocked

**Type:** open-question · **Resolve by:** B4.1 · **Affects:** B4.1, B4.4, W3.4

The Custom code step holds both the JS editor (CSP-gated) and the PHP hook picker. B4.1 creates the step, but B4.4 doesn't depend on it. If the spike blocks handlers, no task creates the step, and it is unstated how a layer's phpHook gets chosen.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:56-57, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47-49

### ISS-201 JS handler editor details

**Type:** open-question · **Resolve by:** B4.1 · **Affects:** B4.1

No editor control or code-editor library is named. The sources don't say whether an empty event is omitted or stored as an empty string, or whether Review & Publish checks handler syntax.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:61

### ISS-202 LayerHook method signatures

**Type:** open-question · **Resolve by:** B4.2 · **Affects:** B4.2, B4.3, B4.5

The six methods are named, and before* can change attributes or throw HookRejected. The sources don't say what each receives (attributes, geometry, identity, profile, layer) or returns, or how a delete hook identifies the feature.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:186-188

### ISS-203 HookRegistry key format and discovery

**Type:** open-question · **Resolve by:** B4.2 · **Affects:** B4.2, B4.4

phpHook is only 'a key from HookRegistry'. The sources don't say whether it is the class name or something else, or how discovery of app/Hooks/* works. A class-name key would break published profiles on rename.

Sources: /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:62, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:48

### ISS-204 One backlog line covers B4.3 and B4.4

**Type:** conflict · **Resolve by:** B4.3 · **Affects:** B4.3, B4.4

The builder backlog has one checkbox, 'Run hooks around applyEdits; hook picker in the custom code step', but both backlogs say one task = one session. The plan splits it into two tasks sharing one box.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:3, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:49

### ISS-205 'Every edit runs the layer's PHP hook' versus nullable phpHook

**Type:** conflict · **Resolve by:** B4.3 · **Affects:** B3.2, B4.3, B4.4

The done-when line says every edit runs the layer's hook. The schema makes phpHook nullable and the picker offers none. The sources don't say whether every editable layer must have a hook, or what happens when a published key no longer exists (reject or skip).

Sources: CLAUDE.md:63, /home/user/arcgisbuilderwebapplication/CLAUDE.md:110, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:62

### ISS-206 EditGate rules and hook-changed attributes

**Type:** open-question · **Resolve by:** B4.3 · **Affects:** B4.3

The order is EditGate, before*, applyEdits. A before* hook can change readonly or non-editable fields. The sources don't say whether EditGate rules apply again to the hook's changes.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, /home/user/arcgisbuilderwebapplication/CLAUDE.md:188, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:134

### ISS-207 after* hook behaviour on success and failure

**Type:** open-question · **Resolve by:** B4.3 · **Affects:** B4.3

The sources don't say whether after* runs when applyEdits fails, or what the endpoint returns when after* throws after the service already changed.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, /home/user/arcgisbuilderwebapplication/CLAUDE.md:186-188

### ISS-208 B4.3 and B4.5 checks need the widget edit path

**Type:** plan-gap · **Resolve by:** B4.3 · **Affects:** B4.3, B4.5, W3.4, B3.7, W0.7

B4.3's checklist needs a real edit through Runner, which needs W3.4, B3.7 and W0.7, but B4.3 depends only on B4.2 and B3.6. B4.5 lists those dependencies, so it can't finish until widget Phase 3 lands. Http::fake() can't show a real service accepts hook-changed attributes.

Sources: traceability pass, /home/user/arcgisbuilderwebapplication/CLAUDE.md:268-270, /home/user/ArcGISRunner/docs/TASKS.md:40, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43

### ISS-209 No route named for the hook list

**Type:** open-question · **Resolve by:** B4.4 · **Affects:** B4.4

The Builder controllers list webmaps, layers, profiles, drafts and publish, but no endpoint for the HookRegistry key list the picker needs.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:122-123, /home/user/arcgisbuilderwebapplication/CLAUDE.md:214

### ISS-210 Example hook behaviour and production presence

**Type:** open-question · **Resolve by:** B4.5 · **Affects:** B4.5, W3.4, W3.5

The backlog says only 'Example hook + tests'. No source defines what it does, though W3.4/W3.5 checklists need a hook that rejects edits. Living in app/Hooks/, it would be discovered in production and offered to builders.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:50, /home/user/arcgisbuilderwebapplication/CLAUDE.md:223

### ISS-211 Drift warning scope

**Type:** open-question · **Resolve by:** B4.6 · **Affects:** B4.6

The sources say only 'saved layers/fields no longer in the service'. They don't say when the check runs, whether it blocks publish, what it names, how layers are matched, whether changed types, domains or capabilities count, or what the widget does at runtime with a missing field.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:54, decision-log:P19, CLAUDE.md:113-114

### ISS-212 Error handling and session timeout UX scope

**Type:** open-question · **Resolve by:** B4.7 · **Affects:** B4.7

The backlog item has no detail: which errors, what the user sees, what happens when the session or Portal token expires mid-wizard, or what becomes of unsaved work. If the agreed scope covers ProfileStore, autosave or Review & Publish, B4.7 also depends on B0.6, B1.6 and B3.2.

Sources: /home/user/arcgisbuilderwebapplication/docs/TASKS.md:55, /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-129

### ISS-213 Second kind unnamed and timing unclear

**Type:** open-question · **Resolve by:** B4.8 · **Affects:** B4.8

More kinds are planned but none is named. Kinds other than crud are out of v1 scope, and Phase 6 follows Phase 5. The sources don't say whether design waits until v1 is done.

Sources: decision-log:D16, /home/user/arcgisbuilderwebapplication/CLAUDE.md:97, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:57-59

### ISS-214 schemaVersion handling when a kind is added

**Type:** open-question · **Resolve by:** B4.8 · **Affects:** B4.8

Adding a kind widens the kind union, and shape changes bump schemaVersion after ship. The sources don't say whether existing crud profiles move to the new version; an older widget would then show an unknown-version error instead of 'update the Runner widget'.

Sources: /home/user/arcgisbuilderwebapplication/CLAUDE.md:38-40, /home/user/arcgisbuilderwebapplication/CLAUDE.md:279-280, CLAUDE.md:97-98

### ISS-215 W4.1 missing dependencies

**Type:** plan-gap · **Resolve by:** W4.1 · **Affects:** W4.1, W3.3

W4.1 moves Add/Edit geometry control strings into translations but doesn't depend on W3.3, which builds those controls. Its checklist also needs the builder listing and edit endpoints and a real Portal (B3.5, B3.6, possibly B4.3), which its dependsOn doesn't list.

Sources: traceability pass, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41-42

### ISS-216 i18n scope: locales, profile text and server messages

**Type:** open-question · **Resolve by:** W4.1 · **Affects:** W4.1

The sources name only default.ts and 'i18n'. They don't say which locales ship, who translates, whether date and number formats are covered, whether profile text (names, titles, labels) is translated (which would change the contract), or whether server, hook and service messages are translated or passed through.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:46, CLAUDE.md:88, CLAUDE.md:138, CLAUDE.md:164, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:48

### ISS-217 Experience Builder translation API and file layout

**Type:** unverified-assumption · **Resolve by:** W4.1 · **Affects:** W4.1

The API a widget uses to read translations, file names for other locales, and whether settings needs its own translations folder under src/setting/ are unverified for 1.18.

Sources: CLAUDE.md:153-164

### ISS-218 One backlog checkbox covers i18n and tests

**Type:** open-question · **Resolve by:** W4.1 · **Affects:** W4.1, W4.3

TASKS.md:46 puts i18n and tests on one line, though one task = one session. Nothing decides whether to split it or check it when both are done.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:3, /home/user/ArcGISRunner/docs/TASKS.md:46

### ISS-219 Developer Edition jest setup, scope and coexistence with Vitest

**Type:** unverified-assumption · **Resolve by:** W4.3 · **Affects:** W4.3

It is unverified that Developer Edition 1.18 ships a jest setup that runs custom widget tests, its command, test folder, and whether it follows the mklink /J junction. No source says who confirms it before W4.3, what the tests cover, or how jest and Vitest files are kept apart in one package. No replacement is named if no usable setup exists.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:46, CLAUDE.md:167-169, CLAUDE.md:188-202

### ISS-220 Out-of-scope items missing from the W4.4 scope guard

**Type:** plan-gap · **Resolve by:** W4.4 · **Affects:** W4.4

Out-of-scope requirement R097 (registering each profile as its own Portal widget, a possible later 'publish as its own widget') has no task step. R334 and R098 (end users can't switch profiles at runtime; Runner can't edit services outside the profile's webmap, decision-log O8) were also missing from W4.4; they are now in its steps and acceptance. W1.3 and B3.6 enforce the behaviour.

Sources: R097, CLAUDE.md:51-52, R334, R098, decision-log:O8

### ISS-221 Deferred list has no checkbox, review point or owner

**Type:** open-question · **Resolve by:** W4.4 · **Affects:** W4.4

The 'Explicitly deferred' entry has no checkbox, and other out-of-scope items sit only in CLAUDE.md. No source says when a deferred item is reconsidered or who approves moving one into scope.

Sources: /home/user/ArcGISRunner/docs/TASKS.md:48-50, CLAUDE.md:56, CLAUDE.md:174-175, decision-log:D25

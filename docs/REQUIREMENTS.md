# ArcGIS Runner — Requirements

Generated 2026-10-03. Every decision, requirement and constraint pulled from the decision log, both CLAUDE.md files, both backlogs and the schema docs, deduplicated. [`PLAN.md`](./PLAN.md) cites these ids. Line references point at the files as they were on the generation date.

This file is identical in both repos (`kschultzBGOH/ArcGISBuilderWebApplication` and `kschultzBGOH/ArcGISRunner`). The brain session keeps them in sync.

Status: `confirmed-by-user`, `proposed-not-objected`, `pending`, or `documented` (stated in the docs).

## process

| Id | Requirement | Status | Sources |
|---|---|---|---|
| R001 | Don't redefine the profile JSON contract in the widget repo. | documented | CLAUDE.md:20 |
| R002 | Build order: design and build the builder app first; the widget consumes its output. | confirmed-by-user | CLAUDE.md:21, decision-log:D6 |
| R003 | A Project scope section was added to both CLAUDE.md files. It is identical in both repos, and the brain session keeps them in sync. | confirmed-by-user | CLAUDE.md:25, /home/user/arcgisbuilderwebapplication/CLAUDE.md:72, decision-log:D25 |
| R004 | When Portal is upgraded, rebuild the widget with the matching Developer Edition and redeploy it. | documented | CLAUDE.md:70-71, decision-log:O6 |
| R005 | The widget repo layout includes `/CLAUDE.md`. | documented | CLAUDE.md:145 |
| R006 | The widget repo layout includes `/docs/TASKS.md`. | documented | CLAUDE.md:146 |
| R007 | The planning chat (the brain) covers both the widget repo and the builder app repo. | documented | CLAUDE.md:173, /home/user/arcgisbuilderwebapplication/CLAUDE.md:257 |
| R008 | The brain holds the plan, makes architecture calls, keeps both CLAUDE.md files and both backlogs current, and reviews pull requests. | documented | CLAUDE.md:174-175, /home/user/arcgisbuilderwebapplication/CLAUDE.md:257-258 |
| R009 | The main planning chat acts as the brain and doesn't write large chunks of code; coding is spawned to separate sessions. | confirmed-by-user | CLAUDE.md:175, decision-log:D1 |
| R010 | Per task, step 1: pick (or ask the user to pick) the next unstarted task in `docs/TASKS.md`. | documented | CLAUDE.md:179 |
| R011 | Per task, step 2: spawn one cloud Claude Code session per task in `docs/TASKS.md`, with a self-contained prompt covering the task, the relevant parts of CLAUDE.md, and the files to touch. | documented | CLAUDE.md:180-181, /home/user/arcgisbuilderwebapplication/CLAUDE.md:261-262 |
| R012 | Per task, step 3: each task gets its own branch from `main`. The session reads CLAUDE.md, does the work, checks the box in `docs/TASKS.md`, and opens a pull request into `main`. | confirmed-by-user | CLAUDE.md:182-183, /home/user/arcgisbuilderwebapplication/CLAUDE.md:263-264, decision-log:D24 |
| R013 | A task session never pushes to `main` directly. | documented | CLAUDE.md:184, /home/user/arcgisbuilderwebapplication/CLAUDE.md:264 |
| R014 | Per task, step 4: the brain reviews each task's pull request. | confirmed-by-user | CLAUDE.md:185, /home/user/arcgisbuilderwebapplication/CLAUDE.md:265, decision-log:D24 |
| R015 | If the work showed the plan was wrong, the brain updates CLAUDE.md. | documented | CLAUDE.md:185-186 |
| R016 | Cloud sessions can't reach Portal, IIS or the network share. | documented | CLAUDE.md:190, /home/user/arcgisbuilderwebapplication/CLAUDE.md:267 |
| R017 | Experience Builder Developer Edition isn't available in cloud sessions, so widget code that imports `jimu-*` can't be compiled or type-checked there. | documented | CLAUDE.md:190-191, CLAUDE.md:191-192, decision-log:O7 |
| R018 | All Claude Code coding sessions run in the cloud. | confirmed-by-user | decision-log:D23 |
| R019 | Every widget pull request ends with a local test checklist: build in Developer Edition 1.18, what to click, what should happen. | proposed-not-objected | CLAUDE.md:198-199, decision-log:P17 |
| R020 | Builder pull requests that touch sign-in, the share or IIS end with a local test checklist. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:268-270 |
| R021 | TypeScript strict; no `any` unless unavoidable (an SDK typing gap forces it), with a comment saying why. | documented | CLAUDE.md:206, /home/user/arcgisbuilderwebapplication/CLAUDE.md:274 |
| R022 | Comments only for a non-obvious why. | documented | CLAUDE.md:207, /home/user/arcgisbuilderwebapplication/CLAUDE.md:278 |
| R023 | No speculative generality: build a kind's features when that kind needs them. | documented | CLAUDE.md:208 |
| R024 | Changes to the widget's own config update `docs/CONFIG_SCHEMA.md`. | documented | CLAUDE.md:209 |
| R025 | The Response Style guide (andrewroxby/claude-style-patch STYLE.md, CC0) is in both CLAUDE.md files and applies to chat replies, docs, and code comments in the project. | confirmed-by-user | CLAUDE.md:212-303, /home/user/arcgisbuilderwebapplication/CLAUDE.md:282-373, decision-log:D25 |
| R026 | Builder convention: Laravel conventions, thin controllers, logic in `app/Services` / `app/Runner`, constructor injection. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:275-276 |
| R027 | Backlog rule in both repos' docs/TASKS.md: one task = one builder session. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:3, /home/user/ArcGISRunner/docs/TASKS.md:3 |
| R028 | Backlog tasks in both repos are checked off as work lands. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:3, /home/user/ArcGISRunner/docs/TASKS.md:3 |
| R029 | Builder Phase 0 is 'Decisions & scaffolding'. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:5 |
| R030 | Builder Phase 0 (done): CLAUDE.md, schema contract, backlog. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:7 |
| R031 | Builder Phase 1 'Foundation': scaffold, Portal sign-in + group check, profile store, wizard shell with common steps (name & kind, webmap). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:246-247, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:16 |
| R032 | Builder Phase 2 '`crud` steps': layers & fields, pages, input types, designer. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:248, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:26 |
| R033 | Builder Phase 3 'Publish & runtime': custom CSS step, review/publish, runtime profile endpoints, edit endpoint with `EditGate`, widget hosting. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:249-250, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:35 |
| R034 | Builder Phase 4 'Custom code': JS handlers (after the CSP spike), PHP hooks. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:251, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:45 |
| R035 | Builder Phase 5 'Polish': drift warnings, error handling, session timeout UX. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:252, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:52 |
| R036 | Builder Phase 6 'Next kinds': decide and design each new kind (starting with the second kind) in the brain session before any build. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:253, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:57, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:59 |
| R037 | Widget Phase 0 is 'Prove deployment' and is done first. | documented | /home/user/ArcGISRunner/docs/TASKS.md:5 |
| R038 | Widget Phase 0 (done): repo skeleton + CLAUDE.md. | documented | /home/user/ArcGISRunner/docs/TASKS.md:7 |
| R039 | Widget Phase 1 is 'Shell'. | documented | /home/user/ArcGISRunner/docs/TASKS.md:17 |
| R040 | Widget Phase 2 is '`crud`: List & View'. | documented | /home/user/ArcGISRunner/docs/TASKS.md:26 |
| R041 | Widget Phase 3 is '`crud`: Add / Edit / Delete'. | documented | /home/user/ArcGISRunner/docs/TASKS.md:35 |
| R042 | Widget Phase 4 is 'Polish'. | documented | /home/user/ArcGISRunner/docs/TASKS.md:44 |
| R043 | The widget README points to `CLAUDE.md` for architecture. | documented | /home/user/ArcGISRunner/README.md:8 |
| R044 | The widget README points to `docs/TASKS.md` for the backlog. | documented | /home/user/ArcGISRunner/README.md:9 |
| R045 | The builder README points to `CLAUDE.md` for architecture and conventions. | documented | /home/user/arcgisbuilderwebapplication/README.md:6 |
| R046 | The builder README points to `docs/TASKS.md` for the backlog. | documented | /home/user/arcgisbuilderwebapplication/README.md:8 |
| R047 | The ArcGISRunner .gitignore ignores `node_modules/`, `dist/`, `.DS_Store`, `*.log`. | documented | /home/user/ArcGISRunner/.gitignore:1-4 |
| R048 | The builder repo .gitignore ignores `node_modules/`, `dist/`, `vendor/`, `.env`, `*.log`, `.DS_Store`. | documented | /home/user/arcgisbuilderwebapplication/.gitignore:1-6 |
| R049 | Nothing was designed yet when this started. | confirmed-by-user | decision-log:D5 |
| R050 | There are two separate repos. | confirmed-by-user | decision-log:D6 |
| R051 | The user's wizard numbering skipped step 3; the user said 'nothing, just renumber'. | confirmed-by-user | decision-log:D8 |
| R052 | Create a full plan from everything discussed. | confirmed-by-user | decision-log:D26 |
| R053 | Do not invent anything in the plan. | confirmed-by-user | decision-log:D26 |
| R054 | Document any issues or problems; they will be reviewed as coding starts. | confirmed-by-user | decision-log:D26 |
| R055 | Create `main` in the widget repo from branch claude/arcgis-runner-setup-uwxnyf (user approval given in this request's wording 'before creating main'). | pending | decision-log:O2 |
| R056 | The user deferred the CSP fallback decision until after the spike. | pending | decision-log:O3 |
| R057 | The line-by-line review of the widget CLAUDE.md reached 'How work gets done'. | pending | decision-log:O4 |
| R058 | The widget CLAUDE.md Conventions and Response Style sections were not reviewed. | pending | decision-log:O4 |
| R059 | The builder CLAUDE.md has not been reviewed line by line. | pending | decision-log:O4 |
| R060 | So far the brain has pushed planning docs straight to `main` in the builder repo. | documented | decision-log:O10 |
| R061 | So far the brain has pushed planning docs to claude/arcgis-runner-setup-uwxnyf in the widget repo. | documented | decision-log:O10 |

## cross

| Id | Requirement | Status | Sources |
|---|---|---|---|
| R062 | Project name is ArcGIS Runner. | confirmed-by-user | decision-log:D1 |
| R063 | ArcGIS Runner is one ArcGIS Experience Builder custom widget that acts as an engine for (runs) profiles built in the ArcGIS Builder Web Application (github.com/kschultzBGOH/ArcGISBuilderWebApplication). | documented | CLAUDE.md:5-6, /home/user/ArcGISRunner/README.md:3-5 |
| R064 | The user's intent is a 'multifunctional widget generator' to quickly deploy widgets. | confirmed-by-user | decision-log:D14 |
| R065 | The widget reads settings designed in a separate website (React + PHP 8.4.25). | confirmed-by-user | decision-log:D4 |
| R066 | The JSON settings are stored on a network drive on the org's internal servers: in scope (v1), drafts and published profiles are stored on the org network share. | confirmed-by-user | CLAUDE.md:38, /home/user/arcgisbuilderwebapplication/CLAUDE.md:85, decision-log:D4 |
| R067 | Both apps are hosted locally on IIS on the org's servers (not cloud). The Builder Web Application (`kschultzBGOH/ArcGISBuilderWebApplication`) is a standalone Laravel + React site hosted on the org's own IIS (Windows) servers; it builds and publishes profiles, hosts the Runner widget files, and handles every widget write. | confirmed-by-user | CLAUDE.md:31, /home/user/arcgisbuilderwebapplication/CLAUDE.md:78, /home/user/arcgisbuilderwebapplication/CLAUDE.md:115-116, decision-log:D22 |
| R068 | Deploying new functionality (a new "widget") means publishing a profile in the builder app: no widget rebuild, no Portal re-registration, no file copying. | documented | CLAUDE.md:10-12, /home/user/arcgisbuilderwebapplication/CLAUDE.md:15-16 |
| R069 | You build profiles in a wizard; a profile is a named, published configuration of a given kind. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:8-9 |
| R070 | The first kind is `crud`: List / Add / Edit / View / Delete pages over the webmap's feature layers and tables, for any geometry type. | documented | CLAUDE.md:15-17, /home/user/arcgisbuilderwebapplication/CLAUDE.md:9-10 |
| R071 | More widget kinds beyond list/add/edit/view/delete are planned; more kinds come later. | confirmed-by-user | CLAUDE.md:17, decision-log:D16 |
| R072 | Decision (done): kinds, with `crud` first; 'kind' is a choice from the start. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:9, /home/user/ArcGISRunner/docs/TASKS.md:8, decision-log:D16 |
| R073 | Decision (done): the deployment model is one registered Runner widget + profiles (confirmed twice: 'one widget with a profile is fine'). | confirmed-by-user | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:9, /home/user/ArcGISRunner/docs/TASKS.md:8, decision-log:D15 |
| R074 | Decision (done): the widget is hosted by the builder app; OK to serve the widget from the Laravel server. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:9, /home/user/ArcGISRunner/docs/TASKS.md:8, decision-log:D14 |
| R075 | Portal is ArcGIS Enterprise 12.0 (Experience Builder 1.18, ArcGIS Maps SDK for JavaScript 4.33). | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:124-125, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:8, decision-log:D7 |
| R076 | Edit and View are separate (List, Add, Edit, View, Delete). | confirmed-by-user | decision-log:D10 |
| R077 | The profile JSON contract lives in the builder app repo at `docs/CONFIG_OUTPUT_SCHEMA.md`, versioned by `schemaVersion`. It is the only definition of the `RunnerProfile` and the contract between the ArcGIS Builder Web Application (producer) and the ArcGIS Runner widget (consumer). | documented | CLAUDE.md:19-20, /home/user/arcgisbuilderwebapplication/CLAUDE.md:209, /home/user/arcgisbuilderwebapplication/CLAUDE.md:241-242, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:3-4, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:4-5, /home/user/arcgisbuilderwebapplication/README.md:7 |
| R078 | Changes to the profile contract happen in the builder app repo first. | documented | CLAUDE.md:209-210 |
| R079 | Any change to the profile shape updates `docs/CONFIG_OUTPUT_SCHEMA.md`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:279 |
| R080 | Once the widget ships, any change to the profile shape bumps `schemaVersion` and must be mirrored in the widget. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:279-280, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:5-6 |
| R081 | Until the widget ships, schema v1 can still change freely. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:4-5 |
| R082 | The Runner Profile JSON output schema is v1, draft. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:1 |
| R083 | The builder app handles every widget write: widget saves go through Laravel so hooks run, making it the single path for every widget write so config rules and PHP hooks are enforced on every edit. | confirmed-by-user | CLAUDE.md:31, /home/user/arcgisbuilderwebapplication/CLAUDE.md:24-26, decision-log:D9 |
| R084 | Writes go only through the builder app, to `POST /api/runtime/profiles/{id}/edits/{layerId}`, never `applyEdits()`, so the builder app enforces the profile and runs PHP hooks (consequence of D9). | proposed-not-objected | CLAUDE.md:135-137, decision-log:P13 |
| R085 | Reads (crud List/View) go straight from the widget to the feature service using `queryFeatures`. | proposed-not-objected | CLAUDE.md:135, /home/user/arcgisbuilderwebapplication/CLAUDE.md:160, decision-log:P13 |
| R086 | The profile only narrows what the live service allows; it never unlocks anything. | documented | CLAUDE.md:139-140 |
| R087 | In scope (v1): one kind, `crud`: List / Add / Edit / View / Delete over a webmap's feature layers and tables, all geometry types. | documented | CLAUDE.md:39, /home/user/arcgisbuilderwebapplication/CLAUDE.md:86 |
| R088 | In scope (v1): custom JavaScript event handlers, run by the widget. | documented | CLAUDE.md:40, /home/user/arcgisbuilderwebapplication/CLAUDE.md:87 |
| R089 | In scope (v1): widget build served from the builder app and registered in Portal once. | documented | CLAUDE.md:43, /home/user/arcgisbuilderwebapplication/CLAUDE.md:90 |
| R090 | Access depends on how the feature layers and webmaps are shared in Portal: Runner (runtime) access follows Portal sharing of the webmap and its layers, including public, anonymous sharing. | confirmed-by-user | CLAUDE.md:44, /home/user/arcgisbuilderwebapplication/CLAUDE.md:91, CLAUDE.md:84, /home/user/arcgisbuilderwebapplication/CLAUDE.md:130, decision-log:D18 |
| R091 | The builder app (Laravel) must never add its own sign-in requirement or gate for widget users. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:130-131, decision-log:D18 |
| R092 | When the user is signed in, the widget sends their Portal token from Experience Builder's session as `Authorization: Bearer` when calling builder app endpoints; anonymous users send none. | documented | CLAUDE.md:84-86, /home/user/arcgisbuilderwebapplication/CLAUDE.md:131-133, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:14 |
| R093 | The builder app and the feature service decide what each identity may see or edit. | documented | CLAUDE.md:86-87 |
| R094 | Profiles are readable by anyone who can open the profile's webmap. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:134 |
| R095 | The service's own sharing and editing settings decide edits, so editor tracking and permissions behave exactly as they do in Portal. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:138-139 |
| R096 | Out of scope (v1): kinds other than `crud`; each is designed in the brain session before it's built. | documented | CLAUDE.md:50, /home/user/arcgisbuilderwebapplication/CLAUDE.md:97 |
| R097 | Out of scope (v1): registering each profile as its own Portal widget (could later be added as "publish as its own widget"). | documented | CLAUDE.md:51, /home/user/arcgisbuilderwebapplication/CLAUDE.md:98 |
| R098 | Out of scope (v1): editing services that aren't in the profile's webmap; Runner can't edit them. | documented | CLAUDE.md:53, /home/user/arcgisbuilderwebapplication/CLAUDE.md:100, decision-log:O8 |
| R099 | Out of scope (v1) / explicitly deferred: attachments. | documented | CLAUDE.md:54, /home/user/arcgisbuilderwebapplication/CLAUDE.md:101, /home/user/ArcGISRunner/docs/TASKS.md:48-50 |
| R100 | Out of scope (v1) / explicitly deferred: related records. | documented | CLAUDE.md:54, /home/user/arcgisbuilderwebapplication/CLAUDE.md:101, /home/user/ArcGISRunner/docs/TASKS.md:48-50 |
| R101 | Out of scope (v1) / explicitly deferred: offline editing. | documented | CLAUDE.md:54, /home/user/arcgisbuilderwebapplication/CLAUDE.md:101, /home/user/ArcGISRunner/docs/TASKS.md:48-50 |
| R102 | Out of scope (v1): ArcGIS Online. | documented | CLAUDE.md:56, /home/user/arcgisbuilderwebapplication/CLAUDE.md:103 |
| R103 | Out of scope (v1): Portal versions other than 12.0. | documented | CLAUDE.md:56, /home/user/arcgisbuilderwebapplication/CLAUDE.md:103 |
| R104 | Out of scope (v1): Web AppBuilder. | documented | CLAUDE.md:56, /home/user/arcgisbuilderwebapplication/CLAUDE.md:103 |
| R105 | Out of scope (v1): apps outside Experience Builder. | documented | CLAUDE.md:56, /home/user/arcgisbuilderwebapplication/CLAUDE.md:103 |
| R106 | Done when: a builder-group member publishes a `crud` profile for a real webmap without writing code. | documented | CLAUDE.md:60, /home/user/arcgisbuilderwebapplication/CLAUDE.md:107 |
| R107 | Done when: an app author adds Runner to an experience in Portal 12.0 and picks that profile. | documented | CLAUDE.md:61, /home/user/arcgisbuilderwebapplication/CLAUDE.md:108 |
| R108 | Done when: end users can list, add, edit, view, and delete exactly as the profile allows, for every geometry type and for tables. | documented | CLAUDE.md:62, /home/user/arcgisbuilderwebapplication/CLAUDE.md:109 |
| R109 | Done when: every edit runs the layer's PHP hook and respects the service's own permissions. | documented | CLAUDE.md:63, /home/user/arcgisbuilderwebapplication/CLAUDE.md:110 |
| R110 | Done when: republishing the profile changes the experience with no widget rebuild and no Portal step. | documented | CLAUDE.md:64, /home/user/arcgisbuilderwebapplication/CLAUDE.md:111 |
| R111 | Widget hosting: the Developer Edition 1.18 build output (`client/dist/widgets/arcgis-runner/`) is deployed to the builder app's `public/widgets/arcgis-runner/` on its IIS site, copied there by a widget deploy script (builder Phase 3 task) and not committed (gitignored). | documented | CLAUDE.md:75-77, /home/user/arcgisbuilderwebapplication/CLAUDE.md:148-150, /home/user/arcgisbuilderwebapplication/CLAUDE.md:230, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:43 |
| R112 | IIS serves the widget build as static files directly, so a `web.config` in the widget folder (not Laravel) adds the CORS headers for the Portal origin. | proposed-not-objected | CLAUDE.md:77-78, /home/user/arcgisbuilderwebapplication/CLAUDE.md:151-153, decision-log:P20 |
| R113 | Portal's widget item points at `{APP_URL}/widgets/arcgis-runner/manifest.json`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:150-151 |
| R114 | Publish makes the profile live (available to the widget). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:44-45, decision-log:P3 |
| R115 | Republishing updates every experience using that profile. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:45-46 |
| R116 | Profiles have an immutable slug id: `profileId` is a generated slug (e.g. "hydrant-inspections") that never changes after creation; `ProfileStore` uses slug ids. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:146-147, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:16, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:12, decision-log:P3 |
| R117 | A kind is a plug-in on both sides, sharing a `kind` key such as `crud`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:30 |
| R118 | Shared across all kinds: name, kind, target webmap, custom CSS, custom JavaScript, publish/draft lifecycle. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:36-37 |
| R119 | Adding a kind means adding both halves, plus its settings shape in `docs/CONFIG_OUTPUT_SCHEMA.md`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:37-38 |
| R120 | Custom CSS styles the whole experience, not only the widget: the Custom CSS wizard step produces one stylesheet per profile (`customCss: string`, unscoped), applied to the whole experience once a Runner widget with that profile loads. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:52-53, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:23, decision-log:D19 |
| R121 | One CSS stylesheet per profile ('calls you can override', not overridden). | proposed-not-objected | decision-log:P7 |
| R122 | CSS rules that start with `.arcgis-runner` (or `[data-profile="{id}"]`) target only the widget. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:53-54 |
| R123 | Runtime endpoint `GET /api/runtime/profiles?webmapId=` returns published profiles (id, name, kind, webmapId) for the widget's settings dropdown, if the caller can open that webmap; the widget calls `GET {base}/api/runtime/profiles?webmapId=` for the settings dropdown (builder Phase 3 task). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:155-156, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41, decision-log:P4, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:15 |
| R124 | Runtime endpoint `GET /api/runtime/profiles/{profileId}` returns (serves) one published profile; the widget calls `GET {base}/api/runtime/profiles/{profileId}` at runtime (builder Phase 3 task). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:157, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:8, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41, decision-log:P4, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:16 |
| R125 | Runtime endpoint `POST /api/runtime/profiles/{profileId}/edits/{layerId}` handles `crud` writes: config check (`EditGate`), PHP `before*` hook, `applyEdits` as the user or anonymously, `after*` hook. The widget's edit client calls `POST {base}/api/runtime/profiles/{profileId}/edits/{layerId}` for `crud` writes (builder Phase 3 / widget Phase 3 tasks). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:158-159, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:42, decision-log:P4, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:17, /home/user/ArcGISRunner/docs/TASKS.md:40 |
| R126 | A `crud` input type is a key plus optional `inputOptions`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:164 |
| R127 | The input type keys in `app/Runner/Kinds/Crud/InputTypes.php` and the widget's renderers must match. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:164-165 |
| R128 | Adding an input type key is a schema change. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:165 |
| R129 | Starting input types: `InputType = 'text' \| 'textarea' \| 'number' \| 'date' \| 'datetime' \| 'dropdown' \| 'readonly'`. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:83, decision-log:P5 |
| R130 | Input types are filtered by field type. | proposed-not-objected | decision-log:P5 |
| R131 | Input types `text` and `textarea` are valid for string fields. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:169 |
| R132 | Input type `number` is valid for integer, small integer, double, single fields. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:170 |
| R133 | Input type `date` is valid for date and date-only fields. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:171 |
| R134 | Input type `datetime` is valid for date fields. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:172 |
| R135 | Input type `dropdown` is valid for any field with a coded-value domain. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:173 |
| R136 | Input type `readonly` is valid for any field. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:174 |
| R137 | Custom JavaScript is stored as source text in the profile and run by the widget with a `ctx` object. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:178 |
| R138 | Each kind defines its own custom JavaScript event set (see the schema doc). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:179 |
| R139 | Risk: custom JavaScript runs in every widget user's browser with their Portal session, so keep the builder group small. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:180-181 |
| R140 | Early spike (open question): Experience Builder's Content-Security-Policy may block compiling JS handlers from text; the CSP spike decides whether handler code compiled from text (`new Function`) can run in a Portal-hosted experience. | pending | CLAUDE.md:106-107, /home/user/arcgisbuilderwebapplication/CLAUDE.md:181-182, /home/user/ArcGISRunner/docs/TASKS.md:15, decision-log:O3 |
| R141 | The builder custom code step depends on the widget repo CSP spike; the spike result feeds builder app Phase 4. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47, /home/user/ArcGISRunner/docs/TASKS.md:15 |
| R142 | If the CSP spike shows handlers are blocked, documented fallback (a): a fixed set of built-in no-code actions. | pending | decision-log:O3 |
| R143 | If the CSP spike shows handlers are blocked, documented fallback (b): self-hosted experiences downloaded from Developer Edition. | pending | decision-log:O3 |
| R144 | Risk: if the Laravel server is down, every Runner widget stops working (it serves widget files, profiles and edits). | documented | decision-log:O5 |
| R145 | A `RunnerProfile` holds the shared fields (id, name, kind, webmap, CSS, JS) plus a kind-specific `settings` object; the Profile shape is shared by every kind. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:240-241, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:11-14 |
| R146 | RunnerProfile field `schemaVersion: 1`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:15 |
| R147 | RunnerProfile field `name: string`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:17 |
| R148 | RunnerProfile field `kind: 'crud'`; more kinds later. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:18 |
| R149 | RunnerProfile field `webmapId: string`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:19 |
| R150 | RunnerProfile field `portalUrl: string`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:20 |
| R151 | RunnerProfile field `publishedAt: string` (ISO 8601). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:21 |
| R152 | RunnerProfile field `publishedBy: string` (Portal username). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:22 |
| R153 | RunnerProfile field `settings: CrudSettings`; its shape depends on `kind`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:24 |
| R154 | `CrudSettings.layers: LayerConfig[]`; array order = the widget's layer picker order. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:38-40 |
| R155 | `PageKey = 'list' \| 'add' \| 'edit' \| 'view' \| 'delete'`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:42 |
| R156 | LayerConfig field `layerId: string` is the operational layer / table id in the webmap. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:45 |
| R157 | LayerConfig field `url: string` is the service layer URL (fallback match). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:46 |
| R158 | LayerConfig field `kind: 'layer' \| 'table'`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:47 |
| R159 | LayerConfig field `title: string`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:48 |
| R160 | LayerConfig field `geometryType: 'point' \| 'multipoint' \| 'polyline' \| 'polygon' \| null`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:49 |
| R161 | LayerConfig field `objectIdField: string`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:50 |
| R162 | LayerConfig field `globalIdField?: string` (optional). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:51 |
| R163 | LayerConfig field `fields: FieldConfig[]`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:53 |
| R164 | LayerConfig field `pages: Record<PageKey, boolean>`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:54 |
| R165 | LayerConfig field `layouts` has `list: ListLayout`, `add: FormLayout`, `edit: FormLayout`, `view: FormLayout`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:55-60 |
| R166 | LayerConfig field `customJs: Partial<Record<CrudJsEvent, string>>` holds function bodies. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:61 |
| R167 | LayerConfig field `phpHook: string \| null` is a key from HookRegistry. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:62 |
| R168 | LayerConfig field `capabilities: { supportsAdd: boolean; supportsUpdate: boolean; supportsDelete: boolean }` is a snapshot at publish; the widget re-checks live. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:64-68 |
| R169 | FieldConfig field `name: string`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:72 |
| R170 | FieldConfig field `label: string`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:73 |
| R171 | FieldConfig field `type: string` (esriFieldType*). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:74 |
| R172 | FieldConfig field `nullable: boolean`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:75 |
| R173 | FieldConfig field `editable: boolean`, as reported by the service. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:76 |
| R174 | FieldConfig field `length?: number` (optional). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:77 |
| R175 | FieldConfig field `domain?: CodedValueDomain \| RangeDomain` (optional). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:78 |
| R176 | FieldConfig field `inputType: InputType`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:79 |
| R177 | FieldConfig field `inputOptions?: Record<string, unknown>` (optional). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:80 |
| R178 | ListLayout field `columns: string[]`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:86 |
| R179 | ListLayout field `sortField?: string` (optional). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:87 |
| R180 | ListLayout field `sortOrder?: 'asc' \| 'desc'` (optional). | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:88 |
| R181 | ListLayout field `pageSize: number`, default 25. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:89 |
| R182 | FormLayout field `sections: Array<{ title: string; fields: string[] }>`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:92-94 |
| R183 | `CodedValueDomain { type: 'codedValue'; codedValues: Array<{ name: string; code: string \| number }> }`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:96 |
| R184 | `RangeDomain { type: 'range'; minValue: number; maxValue: number }`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:97 |
| R185 | crud JS events: `CrudJsEvent = 'onPageLoad' \| 'onFieldChange' \| 'beforeSave' \| 'afterSave' \| 'beforeDelete'`. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:103, decision-log:P6 |
| R186 | crud JS events receive a ctx object (setValue, cancel). | proposed-not-objected | decision-log:P6 |
| R187 | CrudJsContext field `layerId: string`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:118 |
| R188 | CrudJsContext field `page: PageKey`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:119 |
| R189 | CrudJsContext field `attributes: Record<string, unknown>`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:120 |
| R190 | CrudJsContext field `changedField?: string` is present for onFieldChange only. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:121 |
| R191 | CrudJsContext method `setValue(field: string, value: unknown): void`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:122 |
| R192 | CrudJsContext method `cancel(message: string): void`, for before* events only. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:123 |
| R193 | crud rule: every field name in `layouts` must be in `fields`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:129 |
| R194 | crud rule: a field is editable on Add/Edit only if `editable` is true and its `inputType` isn't `readonly`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:130-131 |
| R195 | crud rule: system fields (objectId, globalId, editor-tracking, Shape__Area/Length) are always `readonly`. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:131-132, decision-log:P16 |
| R196 | crud rule: `pages.add/edit/delete` can't be true when the matching capability is false. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:133 |
| R197 | List page = columns only (no sections) ('calls you can override', not overridden). | proposed-not-objected | decision-log:P7 |
| R198 | Polish includes a config drift warning for saved layers/fields no longer in the service (builder Phase 5 task). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:54, decision-log:P19 |
| R199 | `docs/DEPLOYMENT.md` covers the widget folder `web.config` CORS. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14 |
| R200 | `docs/DEPLOYMENT.md` covers Portal widget registration. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14 |
| R201 | Spike — deployment (widget Phase 0, open): host the build on the builder app server (or any HTTPS server with CORS for the Portal origin). | documented | /home/user/ArcGISRunner/docs/TASKS.md:12 |
| R202 | Deployment spike: register the widget in Portal 12.0 and add it to an experience. | documented | /home/user/ArcGISRunner/docs/TASKS.md:12 |

## builder

| Id | Requirement | Status | Sources |
|---|---|---|---|
| R203 | The builder repo is kschultzBGOH/ArcGISBuilderWebApplication (an earlier name ArcGISRunnerConfigSite was replaced): Laravel on PHP 8.4 + React, Portal 12.0. | confirmed-by-user | CLAUDE.md:6-8, decision-log:D6 |
| R204 | The ArcGIS Builder Web Application is a Laravel (PHP 8.4) + React web app, in the spirit of PHPRunner, for generating Experience Builder widget functionality without writing code; a tool for building the JSON configs consumed by the ArcGIS Runner widget (github.com/kschultzBGOH/ArcGISRunner). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:5-6, /home/user/arcgisbuilderwebapplication/README.md:3-4 |
| R205 | The Builder Web Application builds and publishes profiles (Job 1, Builder: the wizard UI and the profiles it produces). | documented | CLAUDE.md:31, /home/user/arcgisbuilderwebapplication/CLAUDE.md:20-21 |
| R206 | The Builder Web Application hosts the Runner widget files (Job 2, Widget host: serves the compiled Runner widget files that Portal's registered widget item points at). | documented | CLAUDE.md:31, /home/user/arcgisbuilderwebapplication/CLAUDE.md:22-23 |
| R207 | Job 3, Runtime backend: the app serves published profiles to the widget. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:24 |
| R208 | The builder site must access Portal webmaps and feature services to build configurations. | confirmed-by-user | decision-log:D5 |
| R209 | The Laravel server will be reachable from everywhere Portal users work. | confirmed-by-user | decision-log:D17 |
| R210 | Decision (done): the builder app uses Laravel. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:8, decision-log:D7 |
| R211 | Decision (done): OAuth app. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:8 |
| R212 | Only members of one designated Portal group may use the builder (decision: single-group access). | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:18, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:8, decision-log:D7 |
| R213 | In scope (v1): builder sign-in with Portal 12.0 OAuth, limited to one Portal group. | documented | CLAUDE.md:36, /home/user/arcgisbuilderwebapplication/CLAUDE.md:83 |
| R214 | In scope (v1): profile wizard with steps name & kind, webmap, `crud` steps (layers & fields, pages, input types, designer), custom CSS, custom code, review & publish. | documented | CLAUDE.md:37, /home/user/arcgisbuilderwebapplication/CLAUDE.md:84 |
| R215 | In scope (v1): PHP hooks as reviewed classes in the builder repo, chosen per layer in the wizard. | documented | CLAUDE.md:41, /home/user/arcgisbuilderwebapplication/CLAUDE.md:88 |
| R216 | PHP typed into the builder was rejected; out of scope (v1): PHP typed into the builder or stored in a profile. | confirmed-by-user | CLAUDE.md:55, /home/user/arcgisbuilderwebapplication/CLAUDE.md:102, decision-log:D9 |
| R217 | Backend: Laravel (current major supporting PHP 8.4) on PHP 8.4.25. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:115 |
| R218 | IIS: PHP 8.4 runs as FastCGI (non-thread-safe x64 build). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:116-117, decision-log:P20 |
| R219 | The IIS URL Rewrite module sends every request that isn't a real file to `public/index.php`, configured in `public/web.config`, which is committed. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:117-118, decision-log:P20 |
| R220 | The IIS application pool runs as a domain service account with write access to the network share. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:119-120, decision-log:P20 |
| R221 | Frontend: Laravel serves a React + TypeScript SPA in `resources/js`, built with Vite (`laravel-vite-plugin`), with Calcite Components. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:121-122, decision-log:P1 |
| R222 | The builder SPA uses Calcite Components (the Phase 0 scaffold includes Calcite Components). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11, decision-log:P1 |
| R223 | The builder frontend uses same-origin session cookies. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:122, decision-log:P1 |
| R224 | The SPA never calls Portal directly. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:122-123, decision-log:P1 |
| R225 | All Portal calls go through `PortalClient`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:125 |
| R226 | Builder sign-in (auth) = Portal OAuth2 authorization-code flow with redirect `{APP_URL}/auth/callback`. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:126-127, decision-log:P2 |
| R227 | Builder auth (Portal) tokens are kept in the server session only. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:127, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:19, decision-log:P2 |
| R228 | Builder authorization: members of `PORTAL_ALLOWED_GROUP_ID`; group membership is checked via /sharing/rest/community/self at login and on every save/publish. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:128-129, decision-log:P2 |
| R229 | `ResolvePortalIdentity` middleware (`app/Http/Middleware/ResolvePortalIdentity.php`, builder Phase 3 task) turns the optional token (or its absence) into a Portal user or "anonymous". | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:133, /home/user/arcgisbuilderwebapplication/CLAUDE.md:218, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:39 |
| R230 | `WebmapAccess` (`app/Services/WebmapAccess.php`, builder Phase 3 task): Laravel asks Portal for the webmap item as that user, or anonymously, to check whether this identity can open the webmap, and caches the answer briefly (short cache). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:135-136, /home/user/arcgisbuilderwebapplication/CLAUDE.md:226, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:40 |
| R231 | Edits are sent to the feature service as that user, or anonymously. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:137-138 |
| R232 | Laravel only adds the profile rules (`EditGate`) and PHP hooks on top of the service's own edit settings. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:139-140 |
| R233 | Anonymous edits are rate-limited per IP (`RUNNER_ANON_EDITS_PER_MINUTE`), because the edit endpoint is open whenever a public editable layer is behind it. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:141-142, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:42, decision-log:P14 |
| R234 | Storage uses Laravel disk `runner_configs`, root `CONFIG_ROOT` on the network share (builder Phase 0 task: `runner_configs` disk). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:143, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:12 |
| R235 | Storage paths under CONFIG_ROOT on the share: published profiles at `{CONFIG_ROOT}/profiles/{profileId}.json`; drafts at `{CONFIG_ROOT}/drafts/{profileId}.json` (builder only). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:144, /home/user/arcgisbuilderwebapplication/CLAUDE.md:145, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:8, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:9, decision-log:P3 |
| R236 | Profile writes are atomic (temp file + rename); `ProfileStore` uses atomic writes. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:146, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:12, decision-log:P3 |
| R237 | Runtime endpoints follow Portal-sharing access, with CORS limited to `RUNNER_ALLOWED_ORIGINS` (the runtime profile endpoints have CORS). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:154, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:41, decision-log:P4 |
| R238 | Listing endpoint `GET /api/runtime/profiles?webmapId=` returns `Array<Pick<RunnerProfile, 'id' \| 'name' \| 'kind' \| 'webmapId' \| 'publishedAt'>>`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:32-33 |
| R239 | The server enforces all of the crud rules again on every edit. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:134 |
| R240 | PHP hooks are classes in `app/Hooks/` implementing `App\Runner\LayerHook` (`beforeAdd`, `afterAdd`, `beforeUpdate`, `afterUpdate`, `beforeDelete`, `afterDelete`). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:186-187 |
| R241 | `before*` hooks can change attributes or throw `HookRejected($message)`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:188 |
| R242 | Custom PHP = PHP hook files in git, reviewed and deployed like any other code (decision: PHP hooks as reviewed code). | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:188-189, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:8, decision-log:D9 |
| R243 | The builder only turns PHP hooks on per layer: it only picks a hook class per layer. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:189-190, decision-log:D9 |
| R244 | The server never runs PHP text from a profile or the browser. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:190 |
| R245 | Env var `PORTAL_URL`, e.g. `https://gis.example.org/portal`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:196 |
| R246 | Env vars `PORTAL_OAUTH_CLIENT_ID` / `PORTAL_OAUTH_CLIENT_SECRET` for the Portal OAuth app. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:197 |
| R247 | Env var `PORTAL_ALLOWED_GROUP_ID`: group allowed to use the builder. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:198 |
| R248 | Env var `CONFIG_ROOT` is a UNC path of the network share, e.g. `\\fileserver\gis\runner` (mapped drive letters aren't visible to the IIS app pool). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:199, decision-log:P20 |
| R249 | Env var `RUNNER_ALLOWED_ORIGINS`: origins where experiences run (normally the Portal host). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:200 |
| R250 | Env var `RUNNER_ANON_EDITS_PER_MINUTE`: per-IP rate limit for anonymous edits. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:201 |
| R251 | Builder repo file `/CLAUDE.md`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:206 |
| R252 | Builder repo file `/docs/TASKS.md`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:207-208 |
| R253 | Write `docs/DEPLOYMENT.md` (builder Phase 0, open): IIS + PHP FastCGI, app pool identity, share access, OAuth app, widget hosting + CORS, Portal registration. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:210, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14 |
| R254 | `docs/DEPLOYMENT.md` covers the IIS site. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14 |
| R255 | `docs/DEPLOYMENT.md` covers PHP 8.4 NTS FastCGI. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14 |
| R256 | `docs/DEPLOYMENT.md` covers URL Rewrite + `public/web.config`. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14 |
| R257 | `docs/DEPLOYMENT.md` covers the app pool as domain service account. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14 |
| R258 | `docs/DEPLOYMENT.md` covers UNC share permissions. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:14 |
| R259 | Builder repo file `app/Http/Controllers/AuthController.php`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:212-213 |
| R260 | Builder repo folder `app/Http/Controllers/Builder/`: webmaps, layers, profiles, drafts, publish (group-gated). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:214 |
| R261 | Builder repo folder `app/Http/Controllers/Runtime/`: profiles + edits for the widget (access follows Portal sharing). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:215 |
| R262 | `EnsurePortalGroupMember` middleware (`app/Http/Middleware/EnsurePortalGroupMember.php`, builder Phase 1 task). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:216-217, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:20 |
| R263 | `app/Runner/KindRegistry.php` (PHP): kind key -> settings validator + runtime handlers, with `crud` registered (builder Phase 1 task). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:219-220, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:21 |
| R264 | Builder repo folder `app/Runner/Kinds/Crud/`: InputTypes, EditGate, CrudSettingsValidator. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:221 |
| R265 | Builder repo files `app/Runner/LayerHook.php`, `HookRejected.php`, `HookRegistry.php` (builder Phase 4 tasks: `LayerHook`, `HookRejected`). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:222, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:48 |
| R266 | Builder repo folder `app/Hooks/`: PHP hooks (reviewed code). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:223 |
| R267 | Builder repo file `app/Services/PortalClient.php`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:224-225 |
| R268 | `ProfileStore` (`app/Services/ProfileStore.php`) with drafts + published (builder Phase 0 task). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:227, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:12 |
| R269 | Builder repo file `/config/runner.php` (included in the Phase 0 scaffold). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:228, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11 |
| R270 | Builder repo files `/routes/web.php`, `/routes/api.php`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:229 |
| R271 | Builder repo folder `resources/js/wizard/common/`: name & kind, webmap, CSS, code, review. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:231-232 |
| R272 | Builder repo folder `resources/js/wizard/kinds/crud/`: crud steps. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:233 |
| R273 | Builder repo folders `resources/js/components/` and `resources/js/api/`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:234 |
| R274 | Builder tests use Pest (`/tests/`) with Portal faked by `Http::fake()` (Phase 0 test setup with `Http::fake()` Portal 12.0 fixtures). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:235, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:13, decision-log:P18 |
| R275 | Tests fake Portal with `Http::fake()` and use a temporary local folder for `CONFIG_ROOT`. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:267-268 |
| R276 | Tests never hit a real Portal; nothing hits a real Portal in CI. | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:276-277, decision-log:P18 |
| R277 | The builder half of a kind lives in `app/Runner/Kinds/{Kind}/` + `resources/js/wizard/kinds/{kind}/`: its wizard steps, its settings validator, and any runtime endpoints it needs. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:31-32 |
| R278 | Drafts autosave: wizard progress autosaves to a server-side draft (wizard shell draft autosave/load). | proposed-not-objected | /home/user/arcgisbuilderwebapplication/CLAUDE.md:44, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:23, decision-log:P3 |
| R279 | Common wizard step (every kind) Name & kind: profile name and kind (builder Phase 1 task). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:48-49, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:24 |
| R280 | Common wizard step Select webmap: select a web map by searching webmaps the signed-in user can access in Portal (builder Phase 1 task). | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:50, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:24, decision-log:D8 |
| R281 | Kind-specific steps come after Select webmap and before Custom CSS in the common step order. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:49-52 |
| R282 | Wizard: a page for customizable CSS. | confirmed-by-user | decision-log:D8 |
| R283 | The Custom CSS live preview shows the widget only, not the full experience (builder Phase 3 task: custom CSS step with widget-only live preview). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:54-55, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:37 |
| R284 | The custom CSS step notes in the UI that rules apply to the whole experience. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:37 |
| R285 | Common wizard step Custom code: a place for customizable JavaScript and PHP code — JavaScript event handlers (events are defined by the kind) and, for kinds that write data, a PHP hook per layer. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:56-57, decision-log:D8 |
| R286 | Common wizard step Review & Publish: show the profile JSON, validate it, publish. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:58 |
| R287 | Builder Phase 3 (open): Review & Publish validates via the kind validator. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38 |
| R288 | Review & Publish writes the published file. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:38 |
| R289 | `crud` step 1 Layers & fields: auto-detect every feature layer and table in the webmap (group layers flattened), populate them with their fields and field types, and select which to include (builder Phase 2 task: layers & fields step). | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:61-63, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:29, decision-log:D8 |
| R290 | Builder Phase 2 (open): layer detection flattens operational layers + tables (incl. group layers). | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:28 |
| R291 | Layer detection fetches schemas. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:28 |
| R292 | `crud` step 2 Pages: for each feature layer, checkboxes for List, Add, Edit, View, Delete. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:64, decision-log:D8 |
| R293 | In the Pages step, a box is disabled if the service doesn't support it (pages disabled by capabilities). | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:65, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:30 |
| R294 | `crud` step 3 Input types: each field listed with options for what kind of input it should be, per field, filtered to what's valid for its field type (more options will be given later) (builder Phase 2 task: input types step). | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:66, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:31, decision-log:D8 |
| R295 | Builder Phase 2 (open): `InputTypes` registry. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:31 |
| R296 | In the input types step, system fields are forced `readonly`. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:31 |
| R297 | `crud` step 4 Designer, List layout: column order, sort, page size. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:67, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:32 |
| R298 | Designer = a page to order the fields and group them into sections: for Add/Edit/View, titled sections, drag to reorder sections and fields. | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:67-68, /home/user/arcgisbuilderwebapplication/docs/TASKS.md:33, decision-log:D8, decision-log:D11 |
| R299 | The Designer has a separate layout per page. | confirmed-by-user | decision-log:D11 |
| R300 | Builder Phase 0 (open): scaffold Laravel (PHP 8.4) + React/TS/Vite in `resources/js`. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11 |
| R301 | The builder Phase 0 scaffold includes `.env.example`. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:11 |
| R302 | Builder Phase 1 (open): `PortalClient` builds the authorize URL. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18 |
| R303 | `PortalClient` does the code exchange. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18 |
| R304 | `PortalClient` does refresh. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18 |
| R305 | `PortalClient` calls `community/self`. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:18 |
| R306 | Builder Phase 1 (open): `/auth/*` routes. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:19 |
| R307 | Builder Phase 1 (open): not-authorized page. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:20 |
| R308 | Builder Phase 1 (open): kind registry (SPA) with `crud` registered. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:21 |
| R309 | Builder Phase 1 (open): profile list page (create, open, duplicate, delete draft). | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:22 |
| R310 | Builder Phase 1 (open): wizard shell with common steps + kind steps. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:23 |
| R311 | Builder Phase 3 (open): `EditGate`. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:42 |
| R312 | Builder Phase 4 (open): custom code step with a JS handler editor per layer/event. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:47 |
| R313 | Builder Phase 4 (open): `HookRegistry` discovers `app/Hooks/*`. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:48 |
| R314 | Builder Phase 4 (open): run hooks around `applyEdits`. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:49 |
| R315 | Builder Phase 4 (open): hook picker in the custom code step. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:49 |
| R316 | Builder Phase 4 (open): example hook + tests. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:50 |
| R317 | Builder Phase 5 (open): error handling. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:55 |
| R318 | Builder Phase 5 (open): session timeout UX. | documented | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:55 |

## widget

| Id | Requirement | Status | Sources |
|---|---|---|---|
| R319 | The ArcGIS Runner widget (repo kschultzBGOH/ArcGISRunner) is one Experience Builder custom widget (Developer Edition 1.18 / ArcGIS Enterprise 12.0), registered in Portal 12.0 once; it renders the profile chosen in its settings. | documented | CLAUDE.md:10, CLAUDE.md:32, /home/user/arcgisbuilderwebapplication/CLAUDE.md:11-12, /home/user/arcgisbuilderwebapplication/CLAUDE.md:79, /home/user/ArcGISRunner/README.md:3 |
| R320 | The widget repo is kschultzBGOH/ArcGISRunner. | confirmed-by-user | decision-log:D6 |
| R321 | Platform is an ArcGIS Experience Builder custom widget. | confirmed-by-user | decision-log:D3 |
| R322 | Original goal: a verbose ArcGIS widget that auto-detects map layers and tables. | confirmed-by-user | decision-log:D2 |
| R323 | Original goal: the widget allows listing, adding, editing and deleting features of all feature types. | confirmed-by-user | decision-log:D2 |
| R324 | Original goal: the widget is highly configurable, including which fields are displayed. | confirmed-by-user | decision-log:D2 |
| R325 | Only Portal admins can register custom widgets. | confirmed-by-user | decision-log:D12 |
| R326 | The user has Experience Builder Developer Edition 1.18 installed locally. | confirmed-by-user | decision-log:D13 |
| R327 | Version: the widget is built with Experience Builder Developer Edition 1.18 (ArcGIS Maps SDK for JavaScript 4.33), matching ArcGIS Enterprise 12.0 (decision done: Developer Edition 1.18). | documented | CLAUDE.md:68-70, /home/user/ArcGISRunner/docs/TASKS.md:8 |
| R328 | Custom widgets must use the same Maps SDK version as the Portal. | documented | CLAUDE.md:71 |
| R329 | Stack: the widget is built with TypeScript + React on `jimu-core` / `jimu-arcgis` / `jimu-ui`, with Calcite Components (via `jimu-ui` where wrapped) for controls. | confirmed-by-user | CLAUDE.md:72-73, decision-log:D3 |
| R330 | State is local React state; no Redux unless something is genuinely shared across widgets. | documented | CLAUDE.md:73-74 |
| R331 | In scope (v1): widget Map widget connection, profile dropdown in settings, `crud` rendering, writes through the builder app. | documented | CLAUDE.md:42, /home/user/arcgisbuilderwebapplication/CLAUDE.md:89 |
| R332 | In scope (v1): two-way selection, where List rows highlight on the map and map clicks open the feature in Runner's View screen. | documented | CLAUDE.md:45, /home/user/arcgisbuilderwebapplication/CLAUDE.md:92 |
| R333 | In scope (v1), needed in v1: record links, a URL that opens a specific feature's View or Edit screen. | confirmed-by-user | CLAUDE.md:46, /home/user/arcgisbuilderwebapplication/CLAUDE.md:93, decision-log:D21 |
| R334 | Out of scope (v1): end users switching profiles at runtime; the profile is fixed per widget instance. | documented | CLAUDE.md:52, /home/user/arcgisbuilderwebapplication/CLAUDE.md:99, decision-log:O8 |
| R335 | Out of scope (v1) / explicitly deferred: multiple Map widgets. | documented | CLAUDE.md:54, /home/user/arcgisbuilderwebapplication/CLAUDE.md:101, /home/user/ArcGISRunner/docs/TASKS.md:48-50 |
| R336 | In the widget's settings panel (`src/setting/setting.tsx`), the app author connects a Map widget (map selector) and picks a profile from a dropdown loaded from the builder app via `GET /api/runtime/profiles?webmapId=` (widget Phase 1 task). | documented | CLAUDE.md:13-14, CLAUDE.md:155, /home/user/ArcGISRunner/docs/TASKS.md:20 |
| R337 | The app author chooses a profile in the Runner widget's settings panel when adding Runner to an experience (after registration). | confirmed-by-user | /home/user/arcgisbuilderwebapplication/CLAUDE.md:12-13, /home/user/ArcGISRunner/README.md:6, decision-log:D15 |
| R338 | The settings dropdown lists only profiles built for the connected map's webmap. | proposed-not-objected | CLAUDE.md:89-90, decision-log:P10 |
| R339 | Webmap: read from the connected Map widget's webmap item. | documented | CLAUDE.md:89 |
| R340 | The connected map comes from Experience Builder's standard `useMapWidgetIds`. | documented | /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:19 |
| R341 | The widget's own Experience Builder `config.json` only records which profile it shows. | documented | /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:3-4 |
| R342 | Everything else comes from the profile. | documented | /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:4 |
| R343 | Widget Config field `profileId: string` is chosen in the settings dropdown. | documented | /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:9 |
| R344 | `src/config.ts` defines the config shape `{ profileId, builderBaseUrl? }` (widget Phase 1 task). | documented | CLAUDE.md:154, /home/user/ArcGISRunner/docs/TASKS.md:19 |
| R345 | The widget derives the builder app address from its own hosting location (`props.context.folderUrl` minus `/widgets/arcgis-runner/`), so app authors never type a URL. | proposed-not-objected | CLAUDE.md:80-82, /home/user/ArcGISRunner/docs/TASKS.md:19, decision-log:P9 |
| R346 | Optional `builderBaseUrl` in `config.json` overrides the derived builder app address, for local Developer Edition work only; otherwise the base URL is derived from the widget's hosting URL. | proposed-not-objected | CLAUDE.md:82-83, /home/user/ArcGISRunner/docs/CONFIG_SCHEMA.md:10, decision-log:P9 |
| R347 | `/docs/CONFIG_SCHEMA.md` documents this widget's own config.json (dev override only). | documented | CLAUDE.md:147 |
| R348 | The widget never asks for sign-in itself. | documented | CLAUDE.md:87-88 |
| R349 | If an action needs sign-in, show the service's message. | documented | CLAUDE.md:88 |
| R350 | At runtime the widget loads (fetches) the chosen profile and renders its (matching) kind (widget Phase 1 task). | documented | CLAUDE.md:15, /home/user/arcgisbuilderwebapplication/CLAUDE.md:13-14, /home/user/ArcGISRunner/docs/TASKS.md:21 |
| R351 | `src/runtime/shell/` handles everything that isn't kind-specific (shell for all kinds). | documented | CLAUDE.md:93-95 |
| R352 | Shell step 1: connect to the Map widget (`JimuMapViewComponent`). | documented | CLAUDE.md:96 |
| R353 | Shell step 2: fetch the profile and check `schemaVersion`, `kind` and webmap. | documented | CLAUDE.md:97, /home/user/ArcGISRunner/docs/TASKS.md:21 |
| R354 | Runtime error states: the shell shows clear errors for an unknown version or kind, an unreachable server, or a webmap mismatch. | documented | CLAUDE.md:97-98, /home/user/ArcGISRunner/docs/TASKS.md:21 |
| R355 | If the connected webmap doesn't match the profile, show a clear error. | proposed-not-objected | CLAUDE.md:90-91, decision-log:P10 |
| R356 | A widget that doesn't know a profile's kind shows "update the Runner widget" instead of guessing. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:38-40 |
| R357 | Shell step 3: inject `customCss` into the page `<head>` unscoped, once per profile, as one `<style data-runner-profile="{id}">` tag per profile (reused rather than duplicated); keep it on unmount so styling doesn't flicker as users move between pages; it stays until the page reloads (widget Phase 1 task). | proposed-not-objected | CLAUDE.md:99, CLAUDE.md:100-101, CLAUDE.md:101-102, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:28-29, /home/user/ArcGISRunner/docs/TASKS.md:23, decision-log:P23 |
| R358 | The widget root has `class="arcgis-runner"` and `data-profile="{id}"` so authors can target Runner alone. | proposed-not-objected | CLAUDE.md:102-104, /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:29-30, /home/user/ArcGISRunner/docs/TASKS.md:23, decision-log:P23 |
| R359 | Shell step 4: provide the JS handler runner (only if the CSP spike passed); handlers are compiled from text and called with a kind-defined `ctx`. | documented | CLAUDE.md:105-106, /home/user/ArcGISRunner/docs/TASKS.md:24 |
| R360 | Shell step 5: kind modules are lazy-loaded from the kind registry (`src/runtime/kinds/registry.ts` maps kind key -> lazy module), so only the kind in use is downloaded. | proposed-not-objected | CLAUDE.md:108, CLAUDE.md:161, /home/user/ArcGISRunner/docs/TASKS.md:22, decision-log:P12 |
| R361 | Kind `crud` lives in `src/runtime/kinds/crud/`; the widget half of a kind lives in `widgets/arcgis-runner/src/runtime/kinds/{kind}/` (its React UI, loaded only when a profile of that kind is used). | documented | CLAUDE.md:110-112, /home/user/arcgisbuilderwebapplication/CLAUDE.md:33-34 |
| R362 | crud layers are matched by `layerId`, falling back to `url`, against the connected map, using the Map widget's layer data sources (widget Phase 2 task). | documented | CLAUDE.md:113-114, /home/user/ArcGISRunner/docs/TASKS.md:28 |
| R363 | Reuse the data sources the Map widget already created for each layer (its layer views), so selection syncs with the map and other widgets without the author picking data sources. | proposed-not-objected | CLAUDE.md:114-116, decision-log:P11 |
| R364 | Widget Phase 2 (open): layer picker. | documented | /home/user/ArcGISRunner/docs/TASKS.md:29 |
| R365 | List, View, Add and Edit are screens inside the widget (in-widget page navigation), not Experience Builder pages. | documented | CLAUDE.md:117-118, /home/user/ArcGISRunner/docs/TASKS.md:29 |
| R366 | Widget Phase 2 (open): List page with columns, labels, sort, page size, pagination. | documented | /home/user/ArcGISRunner/docs/TASKS.md:30 |
| R367 | Selecting a List row highlights and zooms on the map. | documented | /home/user/ArcGISRunner/docs/TASKS.md:30 |
| R368 | Widget Phase 2 (open): View page with read-only sections. | documented | /home/user/ArcGISRunner/docs/TASKS.md:31 |
| R369 | Delete is an action with a confirmation dialog on List and View, not a screen (widget Phase 3 task). | proposed-not-objected | CLAUDE.md:118, /home/user/ArcGISRunner/docs/TASKS.md:41, decision-log:P8 |
| R370 | Delete is gated on profile pages + live capabilities. | documented | /home/user/ArcGISRunner/docs/TASKS.md:41 |
| R371 | Map -> Runner: clicking a feature of a profile layer on the map opens it in Runner's View screen (hitTest on profile layers; two-way map/list selection). | confirmed-by-user | CLAUDE.md:119, /home/user/ArcGISRunner/docs/TASKS.md:32, decision-log:D20 |
| R372 | Several map-click hits at once show a short pick list. | proposed-not-objected | CLAUDE.md:120, /home/user/ArcGISRunner/docs/TASKS.md:32, decision-log:P22 |
| R373 | Map clicks don't navigate while on Add/Edit, because those clicks are for drawing geometry (map click -> View is disabled on Add/Edit). | proposed-not-objected | CLAUDE.md:120-121, /home/user/ArcGISRunner/docs/TASKS.md:32, decision-log:P22 |
| R374 | Leaving a form with unsaved changes asks first (unsaved-changes prompt). | proposed-not-objected | CLAUDE.md:121-122, /home/user/ArcGISRunner/docs/TASKS.md:32, decision-log:P22 |
| R375 | The map's own popup is left alone; authors turn it off in the webmap or Map widget if they don't want both. | proposed-not-objected | CLAUDE.md:122-123, decision-log:P22 |
| R376 | Record links: Runner writes and reads its location in the URL hash as `#runner={profileId}:{layerId}:{view\|edit}:{featureKey}`. | proposed-not-objected | CLAUDE.md:124-125, /home/user/ArcGISRunner/docs/TASKS.md:33, decision-log:P15 |
| R377 | The `#runner=` hash merges with Experience Builder's own hash parameters and never replaces them. | documented | CLAUDE.md:125-126 |
| R378 | `featureKey` is the GlobalID when the layer has one, otherwise the ObjectID. | proposed-not-objected | CLAUDE.md:126-127, /home/user/ArcGISRunner/docs/TASKS.md:33, decision-log:P15 |
| R379 | Opening a record link goes to that screen and selects and zooms to the feature. | documented | CLAUDE.md:127-128, /home/user/ArcGISRunner/docs/TASKS.md:33 |
| R380 | With two Runner widgets on a page, only the one whose profile matches the record link responds. | documented | CLAUDE.md:128-129 |
| R381 | View and Edit have a "Copy link" button for record links. | proposed-not-objected | CLAUDE.md:129-130, /home/user/ArcGISRunner/docs/TASKS.md:33, decision-log:P15 |
| R382 | Record links never grant access; an Edit link still needs edit permission. | proposed-not-objected | CLAUDE.md:130, decision-log:P15 |
| R383 | Inputs: one renderer per `inputType` key (+ `readonly` fallback) (widget Phase 3 task). | documented | CLAUDE.md:131, /home/user/ArcGISRunner/docs/TASKS.md:37 |
| R384 | An unknown `inputType` key falls back to `readonly` and logs a warning. | proposed-not-objected | CLAUDE.md:131-132, decision-log:P16 |
| R385 | Widget Phase 3 (open): Add/Edit forms from section layouts. | documented | /home/user/ArcGISRunner/docs/TASKS.md:38 |
| R386 | Geometry add/edit uses `SketchViewModel` on the connected map. | documented | CLAUDE.md:133, /home/user/ArcGISRunner/docs/TASKS.md:39 |
| R387 | Tables skip geometry. | documented | CLAUDE.md:133-134, /home/user/ArcGISRunner/docs/TASKS.md:39 |
| R388 | Show server and hook rejection messages to the user (the edit client shows server/hook messages). | documented | CLAUDE.md:138, /home/user/ArcGISRunner/docs/TASKS.md:40 |
| R389 | Recheck live capabilities. | documented | CLAUDE.md:140 |
| R390 | The widget's own checks are only for the UI. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:134-135 |
| R391 | Widget Phase 3 (open): fire `crud` JS events. | documented | /home/user/ArcGISRunner/docs/TASKS.md:42 |
| R392 | `onPageLoad` fires when a page for this layer opens; it cannot cancel. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:108 |
| R393 | `onFieldChange` fires when a form value changes; it cannot cancel. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:109 |
| R394 | `beforeSave` fires before add/update is sent; it can cancel. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:110 |
| R395 | `afterSave` fires after a successful add/update; it cannot cancel. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:111 |
| R396 | `beforeDelete` fires before delete is sent; it can cancel. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:112 |
| R397 | Handlers are function bodies called as `(ctx) => { ... }`. | documented | /home/user/arcgisbuilderwebapplication/docs/CONFIG_OUTPUT_SCHEMA.md:114 |
| R398 | `/package.json` is for cloud dev only: TypeScript, Vitest, `@arcgis/core` 4.33 (widget Phase 0 task: cloud test harness). | documented | CLAUDE.md:148, /home/user/ArcGISRunner/docs/TASKS.md:10 |
| R399 | In the cloud test harness, `npm test` runs `lib/` tests. | documented | /home/user/ArcGISRunner/docs/TASKS.md:10 |
| R400 | The widget package lives at `/widgets/arcgis-runner/`. | documented | CLAUDE.md:149 |
| R401 | `/widgets/arcgis-runner/manifest.json` has `exbVersion` 1.18.0 (widget Phase 0 task: manifest at `exbVersion` 1.18.0). | documented | CLAUDE.md:150, /home/user/ArcGISRunner/docs/TASKS.md:11 |
| R402 | The widget package contains `config.json`. | documented | CLAUDE.md:151 |
| R403 | The widget package contains `icon.svg`. | documented | CLAUDE.md:152 |
| R404 | `src/runtime/widget.tsx` mounts the shell. | documented | CLAUDE.md:157 |
| R405 | `src/runtime/shell/` holds profile loading, auth, CSS, JS runner, errors. | documented | CLAUDE.md:158 |
| R406 | `src/runtime/lib/` holds shared jimu-free logic + Vitest tests. | documented | CLAUDE.md:159 |
| R407 | `src/runtime/kinds/crud/` holds LayerPicker, ListScreen, ViewScreen, FormScreen, inputs/, mapSync.ts. | documented | CLAUDE.md:162 |
| R408 | `crud/lib/` (listed under `kinds/crud/`) holds jimu-free crud logic (links, layer matching, input mapping) + tests. | documented | CLAUDE.md:163 |
| R409 | `src/translations/default.ts` holds translations. | documented | CLAUDE.md:164 |
| R410 | Widget logic lives in plain TypeScript `lib/` modules with no `jimu-*` imports (hash parsing, layer matching, profile checks, CSS injection, input-type mapping), unit-tested with Vitest in the cloud. | proposed-not-objected | CLAUDE.md:193-195, CLAUDE.md:195, decision-log:P17 |
| R411 | `@arcgis/core` 4.33 is on npm, so its types are available in the cloud. | documented | CLAUDE.md:196 |
| R412 | `jimu-*` components stay thin: they wire `lib/` logic to Experience Builder. | documented | CLAUDE.md:197 |
| R413 | Widget Phase 0 (open): minimal widget builds in Developer Edition 1.18 (local checklist). | documented | /home/user/ArcGISRunner/docs/TASKS.md:11 |
| R414 | Deployment spike: confirm the widget loads and reads `props.context.folderUrl`. | documented | /home/user/ArcGISRunner/docs/TASKS.md:12 |
| R415 | Spike — auth (open): read the signed-in user's Portal token from the Experience Builder session inside the widget. | documented | /home/user/ArcGISRunner/docs/TASKS.md:13 |
| R416 | Auth spike: confirm the widget loads in an experience shared publicly. | documented | /home/user/ArcGISRunner/docs/TASKS.md:13 |
| R417 | Spike — URL hash (open question): can the widget read and write its own `#runner=` parameter alongside Experience Builder's hash parameters without either side clobbering it or reloading the page? | documented | /home/user/ArcGISRunner/docs/TASKS.md:14 |
| R418 | URL hash spike: check page switches and browser back/forward. | documented | /home/user/ArcGISRunner/docs/TASKS.md:14 |
| R419 | Widget Phase 4 (open): i18n. | documented | /home/user/ArcGISRunner/docs/TASKS.md:46 |
| R420 | Widget Phase 4 (open): tests (Developer Edition jest setup). | documented | /home/user/ArcGISRunner/docs/TASKS.md:46 |
| R421 | widgets/arcgis-runner/manifest.json declares name `arcgis-runner`, type `Widget`, label `ArcGIS Runner`. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:2-6 |
| R422 | manifest.json already declares `version` 1.18.0 and `exbVersion` 1.18.0 (updated in the profile-engine commit); the Phase 0 task to confirm a minimal widget builds in Developer Edition 1.18 is still unchecked. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:3-4 |
| R423 | manifest.json description is 'Runs profiles built in the ArcGIS Builder Web Application.' | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:7 |
| R424 | manifest.json `author` is an empty string. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:8 |
| R425 | manifest.json properties: `hasSettingPage` true, `canDrag` true, `isDefault` false. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:9-13 |
| R426 | manifest.json dependencies: `jimu-core`, `jimu-ui`, `jimu-arcgis`. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/manifest.json:14-18 |
| R427 | widgets/arcgis-runner/config.json currently holds `useMapWidgetIds: []`, `layers: {}`, `defaultPageSize: 25`. It predates the current design (original 2026-08-17 scaffold); widget Phase 1 replaces the config shape with `{ profileId, builderBaseUrl? }`. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/config.json:1-5 |
| R428 | src/config.ts currently defines `RunnerLayerConfig` with `layerId`, `enabled`, `visibleFields`, `editableFields`, optional `fieldLabels`, `sortField`, `sortOrder` ('asc' \| 'desc'), and `pageSize`. It predates the current design (original scaffold). | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/config.ts:3-12 |
| R429 | src/config.ts currently defines `Config` as `{ useMapWidgetIds: string[], layers: { [layerId]: RunnerLayerConfig }, defaultPageSize: number }` and `IMConfig = ImmutableObject<Config>`. It predates the current design; widget Phase 1 changes it to `{ profileId, builderBaseUrl? }`. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/config.ts:14-20 |
| R430 | src/runtime/widget.tsx is currently a placeholder: it keeps the active `JimuMapView` in local state through `onActiveViewChange`, and renders `JimuMapViewComponent` only when exactly one map widget id is connected. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:9-23 |
| R431 | src/runtime/widget.tsx currently shows the hardcoded text 'Connect a Map widget to get started.' until a view is active, then 'Map connected. Layer detection not implemented yet.' | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:24-25 |
| R432 | src/runtime/widget.tsx currently renders its root as `className="widget-arcgis-runner p-2"` (widget Phase 1 calls for root `class="arcgis-runner"` + `data-profile`). | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:17 |
| R433 | src/runtime/widget.tsx predates the current design (original scaffold); its comment says layer auto-detection, list/add/edit/delete UI and config-driven field rendering land in 'docs/TASKS.md Phase 1-3', which refers to the old backlog numbering. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/runtime/widget.tsx:5-8 |
| R434 | src/setting/setting.tsx currently renders a `MapWidgetSelector` and writes the selected `useMapWidgetIds` through `props.onSettingChange`. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/setting/setting.tsx:8-21 |
| R435 | src/setting/setting.tsx currently shows placeholder text: 'Per-layer field configuration will appear here once a map is connected and layer detection is implemented.' | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/setting/setting.tsx:22-25 |
| R436 | src/setting/setting.tsx predates the current design (original scaffold); its comment says per-layer field visibility/editability config lands in Phase 4 once layer auto-detection (Phase 1) exists, whereas the current widget Phase 1 settings task is a map selector + profile dropdown and the current Phase 4 is Polish. | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/setting/setting.tsx:6-7 |
| R437 | src/translations/default.ts currently defines `_widgetLabel` 'ArcGIS Runner', `connectMap`, `addFeature` 'Add', `editFeature` 'Edit', `deleteFeature` 'Delete', `confirmDelete` 'Are you sure you want to delete this feature?', and `noLayers` 'No editable layers or tables were found in the connected map.' It predates the current design (original scaffold). | documented | /home/user/ArcGISRunner/widgets/arcgis-runner/src/translations/default.ts:1-9 |
| R438 | The existing widget source (src/config.ts, src/runtime/widget.tsx, src/setting/setting.tsx, config.json, translations) is from the earlier per-layer-settings design and is stale. | documented | decision-log:O9 |

## user-action

| Id | Requirement | Status | Sources |
|---|---|---|---|
| R439 | The user, a Portal admin, registers the widget in Portal once: `{APP_URL}/widgets/arcgis-runner/manifest.json` (Add Item -> Experience Builder widget). | confirmed-by-user | CLAUDE.md:78-79, /home/user/ArcGISRunner/README.md:6, decision-log:D12 |
| R440 | The user will create the Portal OAuth app. | confirmed-by-user | decision-log:D7 |
| R441 | User (open): register the OAuth app in Portal 12.0 with redirect `{APP_URL}/auth/callback`. | pending | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:10, decision-log:O1 |
| R442 | User (open): provide the allowed group id. | pending | /home/user/arcgisbuilderwebapplication/docs/TASKS.md:10, decision-log:O1 |
| R443 | Widget Phase 0 (open): the user sets `main` as the default branch in GitHub settings. | pending | /home/user/ArcGISRunner/docs/TASKS.md:9, decision-log:O2 |
| R444 | For local dev on Windows, link `widgets/arcgis-runner` into the Experience Builder Developer Edition 1.18 checkout with a directory junction (no admin rights needed): `mklink /J <exb>\client\your-extensions\widgets\arcgis-runner <repo>\widgets\arcgis-runner`. | proposed-not-objected | CLAUDE.md:167-169, /home/user/ArcGISRunner/README.md:11-16, decision-log:P21 |
| R445 | Local dev: after linking, run that Developer Edition checkout's `npm start`. | documented | /home/user/ArcGISRunner/README.md:13-14 |
| R446 | The user merges pull requests. | confirmed-by-user | CLAUDE.md:185, /home/user/arcgisbuilderwebapplication/CLAUDE.md:265, decision-log:D24 |
| R447 | The user runs the widget pull request's local test checklist in Developer Edition 1.18 and reports back before merging. | proposed-not-objected | CLAUDE.md:199-200, decision-log:P17 |
| R448 | Anything that needs the real Portal (Phase 0 spikes, sign-in) is a checklist the user runs locally. | documented | CLAUDE.md:201-202 |
| R449 | The user runs the builder pull request's local test checklist against the real servers before merging. | documented | /home/user/arcgisbuilderwebapplication/CLAUDE.md:269-270 |

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
- **Custom CSS** — one stylesheet per profile, applied to the whole experience
  once a Runner widget with that profile loads. Rules that start with
  `.arcgis-runner` (or `[data-profile="{id}"]`) target only the widget. The live
  preview shows the widget only, not the full experience.
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

## Project scope

This section is identical in both repos. The brain session keeps them in sync.

### System

| Part | Repo | Role |
|---|---|---|
| Builder Web Application | `kschultzBGOH/ArcGISBuilderWebApplication` | Standalone Laravel + React site hosted on the org's own IIS servers. Builds and publishes profiles, hosts the Runner widget files, and handles every widget write. |
| ArcGIS Runner widget | `kschultzBGOH/ArcGISRunner` | One Experience Builder custom widget, registered in Portal 12.0 once. Renders the profile chosen in its settings. |

### In scope (v1)

- Builder sign-in with Portal 12.0 OAuth, limited to one Portal group
- Profile wizard: name & kind, webmap, `crud` steps (layers & fields, pages, input types, designer), custom CSS, custom code, review & publish
- Drafts and published profiles stored on the org network share
- One kind, `crud`: List / Add / Edit / View / Delete over a webmap's feature layers and tables, all geometry types
- Custom JavaScript event handlers, run by the widget
- PHP hooks as reviewed classes in the builder repo, chosen per layer in the wizard
- Widget: Map widget connection, profile dropdown in settings, `crud` rendering, writes through the builder app
- Widget build served from the builder app and registered in Portal once
- Runner access follows Portal sharing of the webmap and its layers, including public, anonymous sharing
- Two-way selection: List rows highlight on the map, and map clicks open the feature in Runner's View screen
- Record links: a URL that opens a specific feature's View or Edit screen

### Out of scope (v1)

- Kinds other than `crud` (each is designed in the brain session before it's built)
- Registering each profile as its own Portal widget (could later be added as "publish as its own widget")
- End users switching profiles at runtime
- Editing services that aren't in the profile's webmap
- Attachments, related records, offline editing, multiple Map widgets
- PHP typed into the builder or stored in a profile
- ArcGIS Online, Portal versions other than 12.0, Web AppBuilder, apps outside Experience Builder

### Done when

- A builder-group member publishes a `crud` profile for a real webmap without writing code
- An app author adds Runner to an experience in Portal 12.0 and picks that profile
- End users can list, add, edit, view, and delete exactly as the profile allows, for every geometry type and for tables
- Every edit runs the layer's PHP hook and respects the service's own permissions
- Republishing the profile changes the experience with no widget rebuild and no Portal step

## Locked-in architecture decisions

- **Backend**: Laravel (current major supporting PHP 8.4) on PHP 8.4.25, hosted on
  **IIS** (Windows) on the org's own servers. PHP runs as FastCGI (non-thread-safe
  x64 build). The IIS URL Rewrite module sends every request that isn't a real
  file to `public/index.php`, configured in `public/web.config`, which is committed.
  The IIS application pool runs as a **domain service account** with write access to
  the network share.
- **Frontend**: React + TypeScript SPA in `resources/js`, built with Vite
  (`laravel-vite-plugin`), Calcite Components. Same-origin session cookies. The
  SPA never calls Portal directly.
- **Portal**: ArcGIS Enterprise **12.0** (Experience Builder 1.18, ArcGIS Maps SDK
  for JavaScript 4.33). All Portal calls go through `PortalClient`.
- **Builder auth**: Portal OAuth2 authorization-code flow (redirect
  `{APP_URL}/auth/callback`). Tokens stay in the server session.
- **Builder authorization**: members of `PORTAL_ALLOWED_GROUP_ID`, checked at
  login and on every save/publish.
- **Runtime access follows Portal sharing.** Laravel never adds its own sign-in
  requirement for widget users. The widget sends the user's Portal token
  (`Authorization: Bearer`) when the user is signed in, and nothing when they're
  anonymous. `ResolvePortalIdentity` turns that into a Portal user or "anonymous".
  - **Profiles** are readable by anyone who can open the profile's webmap.
    Laravel asks Portal for the webmap item as that user, or anonymously, and
    caches the answer briefly (`WebmapAccess`).
  - **Edits** are sent to the feature service as that user, or anonymously. The
    service's own sharing and editing settings decide, so editor tracking and
    permissions behave exactly as they do in Portal. Laravel only adds the profile
    rules (`EditGate`) and PHP hooks.
  - **Anonymous edits** are rate-limited per IP (`RUNNER_ANON_EDITS_PER_MINUTE`),
    because the edit endpoint is open whenever a public editable layer is behind it.
- **Storage** (Laravel disk `runner_configs`, root `CONFIG_ROOT` on the network share):
  - `profiles/{profileId}.json` — published
  - `drafts/{profileId}.json` — draft
  - Atomic writes (temp file + rename). `profileId` is a generated slug
    that never changes after creation.
- **Widget hosting**: the compiled widget (Developer Edition 1.18 build output) lives
  in `public/widgets/arcgis-runner/`, copied there by a deploy script and not
  committed. Portal's widget item points at
  `{APP_URL}/widgets/arcgis-runner/manifest.json`. IIS serves those static files
  directly, so a `web.config` in that folder (not Laravel) adds the CORS headers
  for the Portal origin.
- **Runtime endpoints** (access as above, CORS limited to `RUNNER_ALLOWED_ORIGINS`):
  - `GET /api/runtime/profiles?webmapId=` — published profiles (id, name, kind,
    webmapId) for the widget's settings dropdown, if the caller can open that webmap
  - `GET /api/runtime/profiles/{profileId}` — one published profile
  - `POST /api/runtime/profiles/{profileId}/edits/{layerId}` — `crud` writes: config
    check (`EditGate`), PHP `before*` hook, `applyEdits` as the user or anonymously, `after*` hook
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
| `CONFIG_ROOT` | UNC path of the network share, e.g. `\\fileserver\gis\runner` (mapped drive letters aren't visible to the IIS app pool) |
| `RUNNER_ALLOWED_ORIGINS` | origins where experiences run (normally the Portal host) |
| `RUNNER_ANON_EDITS_PER_MINUTE` | per-IP rate limit for anonymous edits |

## Repo layout

```
/CLAUDE.md
/docs/
  TASKS.md
  CONFIG_OUTPUT_SCHEMA.md      <- profile JSON contract with the widget (only definition)
  DEPLOYMENT.md                <- IIS + PHP FastCGI, app pool identity, share access, OAuth app, widget hosting + CORS, Portal registration
/app/
  Http/Controllers/
    AuthController.php
    Builder/                   <- webmaps, layers, profiles, drafts, publish (group-gated)
    Runtime/                   <- profiles + edits for the widget (access follows Portal sharing)
  Http/Middleware/
    EnsurePortalGroupMember.php
    ResolvePortalIdentity.php  <- optional token -> Portal user or anonymous
  Runner/
    KindRegistry.php           <- kind key -> settings validator + runtime handlers
    Kinds/Crud/                <- InputTypes, EditGate, CrudSettingsValidator
    LayerHook.php, HookRejected.php, HookRegistry.php
  Hooks/                       <- PHP hooks (reviewed code)
  Services/
    PortalClient.php
    WebmapAccess.php           <- can this identity open this webmap? (short cache)
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
keeps the CLAUDE.md files and backlogs current and reviews pull requests.

Per task:
1. One **cloud** Claude Code session per task in `docs/TASKS.md`, started with a
   self-contained prompt.
2. The session reads this file, branches from `main`, does the work, checks the
   box, and opens a **pull request into `main`**. It never pushes to `main` directly.
3. The brain reviews the pull request and the user merges it.

Cloud sessions can't reach Portal, IIS or the network share. Tests fake Portal
with `Http::fake()` and use a temporary local folder for `CONFIG_ROOT`. Pull
requests that touch sign-in, the share or IIS end with a **local test
checklist** for the user to run against the real servers before merging.

## Conventions

- TypeScript strict; no `any` unless unavoidable.
- Laravel conventions: thin controllers, logic in `app/Services` / `app/Runner`,
  constructor injection. Portal is faked with `Http::fake()` in tests; tests
  never hit a real Portal.
- Comments only for a non-obvious *why*.
- Any change to the profile shape updates `docs/CONFIG_OUTPUT_SCHEMA.md`.
  Once the widget ships, it also bumps `schemaVersion`.

## Response Style

Source: [andrewroxby/claude-style-patch `STYLE.md`](https://github.com/andrewroxby/claude-style-patch/blob/main/STYLE.md#response-style) (CC0). Applies to chat replies, docs, and code comments in this project.

**Lodestar: Elegance.** "Things should be expressed as simply as possible, but no simpler."

Treat everything below as defaults that serve natural, readable prose, and apply them with judgment. When building for audiences other than the user, optimizing for the specific genre or type of artifact is fine vs. rigid adherence to this guide.  

Straightforward sentences, plain when plain loses nothing, defaulting mostly to short declaratives with clear transitions, without shading into the robotic or stilted. Stilted is never the target, and plain compound sentences are fine.

For explanations or models, prefer a clean map of the territory over dense or intricate phrasing — when a point can be made plainly, make it plainly. Aim for the reader to leave with a cleaner model than they arrived with. Name the moving parts and show the mechanism. Concretize where natural.

Concise, *not* compressed or telegraphic. Aphorisms are not explanations, so give the reader enough steps to follow the reasoning. Compression for compression's sake is not a virtue.

Drift happens most in long, abstract conversations, so re-check these rules/focus on them/keep them in mind exactly when the material turns philosophical or dense or the thread runs long.

### Cohesion

Before drafting anything substantial, use the thinking block to fix what the response is doing and, as a corollary, what should be left out. Essentially everything in it should serve that job or jobs. Cut the merely also true that isn’t additive. Sometimes the job *is* thinking aloud. Still applies. 

### Sentences

Subject of the sentence as the noun, action as the verb, straight line to the object. Syntactic clarity and straightforwardness. Generally default to short declaratives, concrete nouns, active verbs. Convert abstract nominalizations into verbs.

Generally use Anglo-Saxon words over Latinate iff there is no loss of precision for what you want to say.

**Make your antecedents clear** — the reader shouldn't have to investigate your pronouns' provenance. Similarly with your nouns and noun phrases — always make sure it's clear what they're referring to. ("Drop the counterweight" as an opener — what's the counterweight? Rewrite.) If it's been a few turns, this rule is especially important. Humans often need context refreshed and reminded more often than LLMs.  

#### Colons

Avoid colon-hinged sentences where the left side labels the right side's function ("the clear shape: where da da da," "the honest construction: ..."). Avoid starting with a clause leading to a colon ("the obvious thing you were circling: blah blah blah"). Default to rewriting these sentences as two sentences, or one sentence with a natural connective; lead with subjects or state the thing outright. Natural, varied connectives are fine, but feel free to leave them out when sentence order carries the information naturally.

### Say It, Don't Announce It

Generally, start with the point. When a sentence has two parts where the first names or labels what the second does, delete the first part or turn it into its own sentence. Just say the thing. Don't announce points before making them — no "here's the thing," "the key insight is," "what's worth noting."

Avoid verbless fragments as sentences or paragraph openers ("Two things worth watching." "The difference." "One caution."). The fix is to merge the fragment into the sentence it was introducing — the fragment names a topic, the next sentence says something about it, and one full sentence can do both jobs. "Two things worth watching. Whether it holds on long threads." becomes "The first thing to watch is whether it holds on long abstract threads, because that's where this conversation broke down." Natural compound sentences are fine. 

Drop superfluous depth-signaling ("the real issue underneath," "at a more fundamental level") — if the point is deep, the structure shows it. Avoid using "not X, but Y" antithesis as a rhythmic habit; contrast only genuinely competing explanations.

### Stacked Compression

Watch for stacked compression — it's often made LLM prose hard to absorb. Three moves we've identified as causal: turning a concept into a metaphor, freezing a verb into a noun phrase, then packing the compressed units tight against each other. Any one is fine alone; the damage is adjacency. Keep verbs as verbs rather than nominalizing them, use at most one figure or metaphor per sentence, and never set two compressed units side by side. If a clause makes the reader decode more than one packed phrase at once, unpack it — usually by saying it as a plain spoken sentence with the verbs doing the work. Never leave a reader inside a metaphor — cash them out ~immediately and ~always.

### Structure

A good default is bullets for parallelism, paragraphs for causality and sequence — some explanations need joints; don't force everything into bullets.

Transitions should generally be functional. A good model to default to is that each section should answer an implied reader question, for example "What is the answer?" "Why?" "Where does my current model fail?" "What example makes this concrete?" "What should I do with this?"

Bold/italics only when genuinely additive. For complex, hierarchical, structured responses, use Tractatus numbering (1.1, 1.11, 2.31, 2.45, etc). Don't shoehorn this for short structured lists.

### Proportion and Endings

End when the content ends. No summarizing, uplifting, or resolving/synthesizing closer — if the last sentence adds no information the response doesn't already contain, cut it. A response can stop the moment the point is made; it doesn't need to land a beat.

### Corrections

Corrections should be direct, unabashed, and specific. Say (e.g.) "that frame is partly wrong — the confusion is here," then explain.

### For Documents and Deliverables

By default, aim for an engineer's design doc, scannable in 30 seconds. Headers are labels, not sentences. One idea per bullet, short. Nest only when the hierarchy earns it. Tables for parallel comparisons, key-value pairs for specs. No ornamental connective tissue, no decorative prose. No verbless fragments, no 'its not x, its y' antetheses, no colon weighted sentences. 

### Code Comments

Code comments should be genuinely concise. Avoid verbosity or unnecessary historicizing when commenting, and pay close attention to visual aesthetics, i.e., how the comments sit against the code and that they're structured cleanly. Use newlines before and after for clean visual separation. Comments should be clean, tight, functional, and present state oriented.

When leaving comments in code, especially during multiple rounds of edits, do not unnecessarily describe or historicize about defunct or past paths or a path or approach that was left behind. If there's a genuine risk of retracing an error, it's fine to point that out - otherwise hew towards present behavior and functionality / present state, not archaeology of past approaches. Clear that out and remove it where its extraneous.

### Editing

When editing code, always consider the codebase holistically. Consider whether your edits make sense ecologically - i.e., where do they make the most sense structurally, and whether the entire code base remains harmonious and elegant after the edits are made. Gather needed context to ensure this along the way and keep it top of mind during your reviews. Comments get the same treatment - do they read as part of an integrated whole? This applies to edits in general as well, beyond code.  

### Asking the User Questions 

When using the AskUser Tool (or equivalents in harnesses that support it) to ask questions *or* presenting the user with multiple options at a fork in the road, *make sure the options are clear*. They shouldn't have to backtrack to ask you to explain the options or menu - explain the options *before or as* the decision is requested or possible. 

### Miscellany

- Natural color is welcome — gray is not the target. Playfulness, too, where natural or additive. 
- Never end responses with empty engagement-bait questions.
- **Don't say "honestly" / "Honestly?", "the honest x:" or "load-bearing", ever.**
- Remember Eisenhower: plans are worthless, but planning is everything.
- Remember Einstein: as simple as possible, but no simpler.
- Quick affirmative responses and concise updates along the way are helpful. 

### Exemplar

The following need not be imitated robotically, but serves as an example of the style target to hit:

> *Markets are instruments. We maintain them because competition tends to produce lower costs, better products, and widely shared prosperity. That justification is conditional — if competition stops delivering those outcomes, the case for markets weakens. Predation policy follows from the same logic. We don't curb predatory pricing out of a separate commitment to fairness, or because we revere competition for its own sake. We curb it because predation breaks the mechanism markets are valued for. A price war funded by deep pockets stops selecting for efficient production and starts selecting for financial endurance, and those are different contests with different winners. The same premise settles both questions — whether to let firms compete, and whether to stop them destroying each other. Free markets and antitrust look like rival commitments, but each defends competition from a different threat. Free markets guard it from the state; antitrust guards it from the firms themselves.*

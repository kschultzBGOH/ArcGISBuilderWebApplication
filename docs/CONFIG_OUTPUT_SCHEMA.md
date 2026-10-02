# Runner Config JSON — Output Schema (v1, draft)

This is the **contract** between ArcGIS Builder Web Application (producer) and the
ArcGIS Runner widget (consumer). Until the widget ships, v1 can still change
freely. After that, any change bumps `schemaVersion` and must be mirrored in
the widget.

- Live: `{CONFIG_ROOT}/{webmapId}.json`, served at `GET /api/runtime/configs/{webmapId}`
- Draft: `{CONFIG_ROOT}/drafts/{webmapId}.json` (builder only, never served to the widget)

## TypeScript shape

```ts
interface RunnerConfig {
  schemaVersion: 1
  webmapId: string
  portalUrl: string
  name: string
  publishedAt: string            // ISO 8601
  publishedBy: string            // Portal username
  customCss: string              // step 6; widget scopes it under its root element
  layers: LayerConfig[]          // order = widget's layer picker order
}

type PageKey = 'list' | 'add' | 'edit' | 'view' | 'delete'

interface LayerConfig {
  layerId: string                // operational layer / table id in the webmap
  url: string                    // service layer URL (fallback match)
  kind: 'layer' | 'table'
  title: string
  geometryType: 'point' | 'multipoint' | 'polyline' | 'polygon' | null
  objectIdField: string
  globalIdField?: string

  fields: FieldConfig[]          // step 2 (selected) + step 4 (input types)
  pages: Record<PageKey, boolean>          // step 3
  layouts: {                               // step 5
    list: ListLayout
    add: FormLayout
    edit: FormLayout
    view: FormLayout
  }
  customJs: Partial<Record<JsEvent, string>>  // step 7, function bodies
  phpHook: string | null                      // step 7, key from HookRegistry

  capabilities: {                // snapshot at publish; widget re-checks live
    supportsAdd: boolean
    supportsUpdate: boolean
    supportsDelete: boolean
  }
}

interface FieldConfig {
  name: string
  label: string
  type: string                   // esriFieldType*
  nullable: boolean
  editable: boolean              // as reported by the service
  length?: number
  domain?: CodedValueDomain | RangeDomain
  inputType: InputType
  inputOptions?: Record<string, unknown>   // per-type settings, e.g. { rows: 4 } for textarea
}

type InputType = 'text' | 'textarea' | 'number' | 'date' | 'datetime' | 'dropdown' | 'readonly'

interface ListLayout {
  columns: string[]              // field names, in order
  sortField?: string
  sortOrder?: 'asc' | 'desc'
  pageSize: number               // default 25
}

interface FormLayout {
  sections: Array<{ title: string; fields: string[] }>
}

interface CodedValueDomain { type: 'codedValue'; codedValues: Array<{ name: string; code: string | number }> }
interface RangeDomain { type: 'range'; minValue: number; maxValue: number }
```

## JavaScript events (initial set)

```ts
type JsEvent = 'onPageLoad' | 'onFieldChange' | 'beforeSave' | 'afterSave' | 'beforeDelete'
```

| Event | When | Can cancel |
|---|---|---|
| `onPageLoad` | a page for this layer opens | no |
| `onFieldChange` | a form value changes | no |
| `beforeSave` | before add/update is sent | yes |
| `afterSave` | after a successful add/update | no |
| `beforeDelete` | before delete is sent | yes |

Each handler is a function body called as `(ctx) => { ... }`:

```ts
interface JsContext {
  layerId: string
  page: PageKey
  attributes: Record<string, unknown>          // current form/feature values
  changedField?: string                        // onFieldChange only
  setValue(field: string, value: unknown): void
  cancel(message: string): void                // before* events only
}
```

## Rules

- `fields` only holds fields selected in step 2. Every name in `layouts` must be
  in `fields`.
- A field is editable on Add/Edit only if `editable` is true **and** its
  `inputType` isn't `readonly`. System fields (objectId, globalId,
  editor-tracking, Shape__Area/Length) are always forced to `readonly`.
- `pages.add/edit/delete` can't be true when the matching capability is false.
- The server enforces all of this again on every edit
  (`POST /api/runtime/edits/...`). The widget's own checks are only for the UI.

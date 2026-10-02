# Runner Profile JSON — Output Schema (v1, draft)

The **contract** between ArcGIS Builder Web Application (producer) and the
ArcGIS Runner widget (consumer). Until the widget ships, v1 can still change
freely. After that, any change bumps `schemaVersion` and must be mirrored in
the widget.

- Published: `{CONFIG_ROOT}/profiles/{profileId}.json`, served at `GET /api/runtime/profiles/{profileId}`
- Draft: `{CONFIG_ROOT}/drafts/{profileId}.json` (builder only)

## Profile (shared by every kind)

```ts
interface RunnerProfile {
  schemaVersion: 1
  id: string                     // immutable slug, e.g. "hydrant-inspections"
  name: string
  kind: 'crud'                   // more kinds later
  webmapId: string
  portalUrl: string
  publishedAt: string            // ISO 8601
  publishedBy: string            // Portal username
  customCss: string              // widget scopes it under its root element
  settings: CrudSettings         // shape depends on `kind`
}
```

Listing (`GET /api/runtime/profiles?webmapId=`) returns
`Array<Pick<RunnerProfile, 'id' | 'name' | 'kind' | 'webmapId' | 'publishedAt'>>`.

## Kind `crud`

```ts
interface CrudSettings {
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

  fields: FieldConfig[]
  pages: Record<PageKey, boolean>
  layouts: {
    list: ListLayout
    add: FormLayout
    edit: FormLayout
    view: FormLayout
  }
  customJs: Partial<Record<CrudJsEvent, string>>  // function bodies
  phpHook: string | null                           // key from HookRegistry

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
  inputOptions?: Record<string, unknown>
}

type InputType = 'text' | 'textarea' | 'number' | 'date' | 'datetime' | 'dropdown' | 'readonly'

interface ListLayout {
  columns: string[]
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

### `crud` JavaScript events

```ts
type CrudJsEvent = 'onPageLoad' | 'onFieldChange' | 'beforeSave' | 'afterSave' | 'beforeDelete'
```

| Event | When | Can cancel |
|---|---|---|
| `onPageLoad` | a page for this layer opens | no |
| `onFieldChange` | a form value changes | no |
| `beforeSave` | before add/update is sent | yes |
| `afterSave` | after a successful add/update | no |
| `beforeDelete` | before delete is sent | yes |

Handlers are function bodies called as `(ctx) => { ... }`:

```ts
interface CrudJsContext {
  layerId: string
  page: PageKey
  attributes: Record<string, unknown>
  changedField?: string                        // onFieldChange only
  setValue(field: string, value: unknown): void
  cancel(message: string): void                // before* events only
}
```

### `crud` rules

- Every field name in `layouts` must be in `fields`.
- A field is editable on Add/Edit only if `editable` is true **and** its
  `inputType` isn't `readonly`. System fields (objectId, globalId,
  editor-tracking, Shape__Area/Length) are always `readonly`.
- `pages.add/edit/delete` can't be true when the matching capability is false.
- The server enforces all of this again on every edit. The widget's own checks
  are only for the UI.

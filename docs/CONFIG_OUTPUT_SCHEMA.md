# Runner Config JSON — Output Schema (v1)

This is the **contract** between ArcGIS Builder Web Application (producer) and the
ArcGIS Runner ExB widget (consumer). Any change here must bump `schemaVersion`
and be mirrored in the widget repo.

Served at: `GET /api/configs/{webmapId}` → `application/json`
Stored at: `{CONFIG_ROOT}/{webmapId}.json`

## TypeScript shape

```ts
interface RunnerConfig {
  schemaVersion: 1
  webmapId: string
  portalUrl: string              // e.g. https://gis.example.org/portal
  name: string
  lastModified: string           // ISO 8601
  modifiedBy: string             // Portal username
  layers: LayerConfig[]          // order = order shown in the widget's layer picker
}

interface LayerConfig {
  layerId: string                // operational layer / table id inside the webmap
  url: string                    // feature service layer URL (fallback match if ids drift)
  kind: 'layer' | 'table'
  enabled: boolean
  title: string
  geometryType: 'point' | 'multipoint' | 'polyline' | 'polygon' | null  // null for tables
  objectIdField: string
  globalIdField?: string

  visibleFields: FieldConfig[]   // ordered list columns
  editableFields: FieldConfig[]  // ordered add/edit form fields

  sortField?: string
  sortOrder?: 'asc' | 'desc'
  pageSize?: number              // default 25

  // snapshot of service capabilities at config time; the widget must still
  // re-check live layer capabilities, since services can change after save
  supportsAdd: boolean
  supportsUpdate: boolean
  supportsDelete: boolean
}

interface FieldConfig {
  name: string
  label: string                  // display label (override or service alias)
  type: string                   // esriFieldType*, e.g. esriFieldTypeString
  nullable: boolean
  length?: number
  domain?: CodedValueDomain | RangeDomain
}

interface CodedValueDomain {
  type: 'codedValue'
  codedValues: Array<{ name: string; code: string | number }>
}

interface RangeDomain {
  type: 'range'
  minValue: number
  maxValue: number
}
```

## Rules

- `editableFields` only contains fields the service reports as `editable: true`.
- System fields (objectId, globalId, editor-tracking fields, Shape__Area/Length)
  never appear in `editableFields`.
- The widget treats this file as a filter over the live layer. It never unlocks
  anything the live service does not allow.

## Example

```json
{
  "schemaVersion": 1,
  "webmapId": "a1b2c3d4e5f6",
  "portalUrl": "https://gis.example.org/portal",
  "name": "Street Assets",
  "lastModified": "2026-09-30T14:00:00Z",
  "modifiedBy": "kschultz",
  "layers": [
    {
      "layerId": "hydrants_1234",
      "url": "https://gis.example.org/server/rest/services/Utilities/FeatureServer/0",
      "kind": "layer",
      "enabled": true,
      "title": "Hydrants",
      "geometryType": "point",
      "objectIdField": "OBJECTID",
      "globalIdField": "GlobalID",
      "visibleFields": [
        { "name": "ASSETID", "label": "Asset ID", "type": "esriFieldTypeString", "nullable": false, "length": 20 },
        { "name": "STATUS", "label": "Status", "type": "esriFieldTypeSmallInteger", "nullable": true,
          "domain": { "type": "codedValue", "codedValues": [ { "name": "Active", "code": 1 }, { "name": "Retired", "code": 2 } ] } }
      ],
      "editableFields": [
        { "name": "STATUS", "label": "Status", "type": "esriFieldTypeSmallInteger", "nullable": true,
          "domain": { "type": "codedValue", "codedValues": [ { "name": "Active", "code": 1 }, { "name": "Retired", "code": 2 } ] } }
      ],
      "sortField": "ASSETID",
      "sortOrder": "asc",
      "pageSize": 25,
      "supportsAdd": true,
      "supportsUpdate": true,
      "supportsDelete": false
    }
  ]
}
```

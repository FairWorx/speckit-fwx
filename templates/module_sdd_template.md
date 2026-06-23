# SDD: {{MODULE_NAME}} Module ({{MODULE_ID}})

**Module Version:** 1.0
**Last Updated:** {{CURRENT_DATE}}
**Parent SDD:** `docs/02-agile/SDD.md` Use latest version of this document.
**PRD:** `docs/02-agile/modules/{{MODULE_CODE}}/PRD.md`

> **IMPORTANT:** This document references the parent PRD and SDD. Whenever a new version of the parent files is created, the file references above MUST be updated. This document must never deviate from the original intent and architectural design in the parent PRD and SDD.

## Module Registry

| ID | Module ID | Name | Version | Dependencies | Typical Group | Required Role |
|----|-----------|------|---------|--------------|---------------|---------------|
| {{MODULE_ID}} | {{MODULE_CODE}} | {{MODULE_NAME}} | 1.0 | {{DEPENDENCIES}} | {{TYPICAL_GROUP}} | {{REQUIRED_ROLE}} |

## Module Overview

{{MODULE_OVERVIEW}}

## Tech Stack

- **Backend:** {{BACKEND_TECH}}
- **Frontend:** {{FRONTEND_TECH}}
- **Database:** {{DATABASE_TECH}}

## 1. Data Models

### 1.1 `{{TABLE_1_NAME}}`

| Column | Type | Notes |
|--------|------|-------|
| `id` | UUID PK | |
| `{{TABLE_1_COL_1}}` | `{{TABLE_1_TYPE_1}}` | {{TABLE_1_DESC_1}} |
| `{{TABLE_1_COL_2}}` | `{{TABLE_1_TYPE_2}}` | {{TABLE_1_DESC_2}} |

### 1.2 Alembic Migration

**File:** `backend/app/alembic/versions/{{MIGRATION_FILE}}`

```python
def upgrade():
    # create tables
    ...

def downgrade():
    # drop tables
    ...
```

## 2. API Endpoints

**Router prefix:** `{{API_PREFIX}}`

| Method | Path | Description |
|--------|------|-------------|
| GET | {{API_PREFIX}} | List |
| POST | {{API_PREFIX}} | Create |
| GET | {{API_PREFIX}}/{id} | Get detail |
| PATCH | {{API_PREFIX}}/{id} | Update |
| DELETE | {{API_PREFIX}}/{id} | Delete |

## 3. Frontend Component

**File:** `modules/{{MODULE_CODE}}/frontend/index.tsx`

### File Structure

```
modules/{{MODULE_CODE}}/frontend/
├── index.tsx
├── types.ts
└── components/
    └── ...
```

### Component Tree

```
MainView
├── ...
└── ...
```

### UI Patterns

- {{UI_PATTERN_1}}
- {{UI_PATTERN_2}}

### States

- **Loading:** {{LOADING_STATE}}
- **Error:** {{ERROR_STATE}}
- **Empty:** {{EMPTY_STATE}}

## 4. Module Manifest

```json
{
  "module_id": "{{MODULE_CODE}}",
  "name": "{{MODULE_NAME}}",
  "description": "{{MODULE_DESCRIPTION_SHORT}}",
  "version": "1.0.0",
  "required_permissions": [{{MANIFEST_PERMISSIONS}}],
  "frontend_entry": "{{FRONTEND_ENTRY}}",
  "has_unsaved_check": {{HAS_UNSAVED_CHECK}},
  "dependencies": [{{MANIFEST_DEPENDENCIES}}]
}
```

## Integration with Main App

- **Apps Tab**: {{CHANNEL_LOCATION}}
- **Permissions**: {{PERMISSION_DETAILS}}
- **Other**: {{OTHER_INTEGRATION}}

## Change Log

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | {{CURRENT_DATE}} | Initial version |

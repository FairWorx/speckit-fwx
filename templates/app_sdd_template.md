# SDD: {{APP_NAME}}

**Version:** 1.0
**Last Updated:** {{CURRENT_DATE}}
**PRD:** `docs/02-agile/PRD.md`

## System Overview

{{SYSTEM_OVERVIEW}}

## Tech Stack

- **Backend:** {{BACKEND_TECH}}
- **Frontend:** {{FRONTEND_TECH}}
- **Database:** {{DATABASE_TECH}}
- **Infrastructure:** {{INFRA_TECH}}

## 1. Data Models

### 1.1 `{{ENTITY_1_NAME}}`

| Column | Type | Notes |
|--------|------|-------|
| `id` | UUID PK | |
| `{{ENTITY_1_COL_1}}` | `{{ENTITY_1_TYPE_1}}` | {{ENTITY_1_DESC_1}} |
| `{{ENTITY_1_COL_2}}` | `{{ENTITY_1_TYPE_2}}` | {{ENTITY_1_DESC_2}} |

### 1.2 `{{ENTITY_2_NAME}}`

| Column | Type | Notes |
|--------|------|-------|
| `id` | UUID PK | |
| `{{ENTITY_2_COL_1}}` | `{{ENTITY_2_TYPE_1}}` | {{ENTITY_2_DESC_1}} |
| `{{ENTITY_2_COL_2}}` | `{{ENTITY_2_TYPE_2}}` | {{ENTITY_2_DESC_2}} |

### 1.3 Alembic Migration

**File:** `backend/app/alembic/versions/{{MIGRATION_FILE}}`

```python
def upgrade():
    # create tables
    ...

def downgrade():
    # drop tables
    ...
```

## 2. API Design

**Base URL:** `/api/v1`

| Method | Path | Description |
|--------|------|-------------|
| GET | `{{API_PATH_1}}` | List |
| POST | `{{API_PATH_1}}` | Create |
| GET | `{{API_PATH_1}}/{id}` | Get detail |
| PATCH | `{{API_PATH_1}}/{id}` | Update |
| DELETE | `{{API_PATH_1}}/{id}` | Delete |

## 3. Frontend Architecture

**Entry Point:** `{{FRONTEND_ENTRY}}`

### File Structure

```
{{FRONTEND_STRUCTURE}}
```

### Component Tree

```
{{COMPONENT_TREE}}
```

### UI Patterns

- {{UI_PATTERN_1}}
- {{UI_PATTERN_2}}

### States

- **Loading:** {{LOADING_STATE}}
- **Error:** {{ERROR_STATE}}
- **Empty:** {{EMPTY_STATE}}

## 4. Backend Architecture

{{BACKEND_ARCHITECTURE}}

## 5. Deployment

{{DEPLOYMENT_DETAILS}}

## Change Log

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | {{CURRENT_DATE}} | Initial version |

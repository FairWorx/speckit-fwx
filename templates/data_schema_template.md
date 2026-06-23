# Data Schema ({{PROJECT_NAME}})

**Version:** 1.0
**Last Updated:** {{CURRENT_DATE}}

## Current Database State

{{DATABASE_SUMMARY}}

## Entity Relationship Diagram

```mermaid
erDiagram
    {{ENTITY_1}} ||--o{ {{ENTITY_2}} : "{{RELATIONSHIP_DESC}}"
    {{ENTITY_3}} ||--o{ {{ENTITY_4}} : "{{RELATIONSHIP_DESC}}"
    ...
```

---

## Table Definitions

### Migration {{MIGRATION_NUMBER}} — {{MIGRATION_NAME}}

**Revision:** `{{REVISION}}` | **Down:** `{{DOWN_REVISION}}` | **Tables:** {{TABLE_COUNT}}

#### `{{TABLE_NAME}}`

| Column | Type | Constraints |
|--------|------|-------------|
| `id` | UUID | **PK**, server_default=`gen_random_uuid()` |
| `{{COLUMN_1}}` | {{TYPE_1}} | {{CONSTRAINTS_1}} |
| `{{COLUMN_2}}` | {{TYPE_2}} | {{CONSTRAINTS_2}} |

**Indexes:** `{{INDEX_NAME}}` on `{{COLUMNS}}`

---

## Migration Chain

```
{{MIGRATION_CHAIN}}
```

All migrations are sequential (single linear chain).

## Migration History

| Revision | Description | Tables Created | Applied |
|----------|-------------|----------------|---------|
| {{REVISION_1}} | {{DESCRIPTION_1}} | {{TABLES_1}} | {{STATUS_1}} |
| {{REVISION_2}} | {{DESCRIPTION_2}} | {{TABLES_2}} | {{STATUS_2}} |

**Total:** {{TABLE_COUNT}} tables, {{VIEW_COUNT}} views, {{MATERIALIZED_VIEW_COUNT}} materialized views

## Connection Details

- **Host**: {{HOST}}
- **Database**: {{DATABASE_NAME}}
- **User**: {{USER}}
- **Connection String**: `{{CONNECTION_STRING}}`

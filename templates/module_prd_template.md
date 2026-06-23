# PRD: {{MODULE_NAME}} Module ({{MODULE_ID}})

**Module Version:** 1.0
**Last Updated:** {{CURRENT_DATE}}
**Parent PRD:** `docs/02-agile/PRD.md` Use latest version of this document.
**Parent SDD:** `docs/02-agile/SDD.md` Use latest version of this document.
**UI Specification:** `docs/02-agile/UI.md` Use latest version of this document.

> **IMPORTANT:** This document references the parent PRD and SDD. Whenever a new version of the parent files is created, the file references above MUST be updated. This document must never deviate from the original intent and architectural design in the parent PRD and SDD.

## Module Registry

| ID | Module ID | Name | Version | Dependencies | Typical Group | Required Role |
|----|-----------|------|---------|--------------|---------------|---------------|
| {{MODULE_ID}} | {{MODULE_CODE}} | {{MODULE_NAME}} | 1.0 | {{DEPENDENCIES}} | {{TYPICAL_GROUP}} | {{REQUIRED_ROLE}} |

## Module Description

{{MODULE_DESCRIPTION}}

## Core Features

### 1. {{FEATURE_1_TITLE}}
{{FEATURE_1_DESCRIPTION}}

### 2. {{FEATURE_2_TITLE}}
{{FEATURE_2_DESCRIPTION}}

### 3. {{FEATURE_3_TITLE}}
{{FEATURE_3_DESCRIPTION}}

## Data Model

### `{{TABLE_NAME}}`
| Column | Type | Description |
|--------|------|-------------|
| id | UUID PK | |
| {{FIELD_1}} | {{FIELD_1_TYPE}} | {{FIELD_1_DESC}} |
| {{FIELD_2}} | {{FIELD_2_TYPE}} | {{FIELD_2_DESC}} |

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/modules/{{API_BASE}}` | List |
| POST | `/api/v1/modules/{{API_BASE}}` | Create |
| GET | `/api/v1/modules/{{API_BASE}}/{id}` | Get details |
| PATCH | `/api/v1/modules/{{API_BASE}}/{id}` | Update |

## Integration with Main App

- **Apps Tab**: Appears in {{CHANNEL_LOCATION}}.
- **Notifications**: {{NOTIFICATION_BEHAVIOR}}
- **Files**: {{FILE_INTEGRATION}}
- **Other**: {{OTHER_INTEGRATION}}

## Change Log

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | {{CURRENT_DATE}} | Initial version |

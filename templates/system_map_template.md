# System Map ({{PROJECT_NAME}})

**Version:** 1.0
**Last Updated:** {{CURRENT_DATE}}

## Overview

{{PROJECT_OVERVIEW}}

## Architecture

The system follows a {{ARCHITECTURE_PATTERN}} architecture with a {{AUTH_MECHANISM}} auth layer and a {{BACKEND_PATTERN}} backend:


## Service Inventory

| Service | Container | Ports | Health Check | Purpose |
|---------|-----------|-------|-------------|---------|
| {{SERVICE_1}} | {{CONTAINER_1}} | {{PORTS_1}} | {{HEALTH_CHECK_1}} | {{PURPOSE_1}} |
| {{SERVICE_2}} | {{CONTAINER_2}} | {{PORTS_2}} | {{HEALTH_CHECK_2}} | {{PURPOSE_2}} |

## Entry Points

| Entry Point | URL | Authentication | Description |
|-------------|-----|---------------|-------------|
| Frontend | {{FRONTEND_URL}} | {{AUTH_TYPE_1}} | {{FRONTEND_DESC}} |
| API | {{API_BASE_URL}} | {{AUTH_TYPE_2}} | {{API_DESC}} |
| API Docs | {{DOCS_URL}} | {{DOCS_AUTH}} | {{DOCS_DESC}} |

## Auth Module Map

```
{{AUTH_HTTP_METHOD}}   {{AUTH_BASE_PATH}}/{{AUTH_ACTION_1}}
{{AUTH_HTTP_METHOD}}   {{AUTH_BASE_PATH}}/{{AUTH_ACTION_2}}
{{AUTH_HTTP_METHOD}}   {{AUTH_BASE_PATH}}/{{AUTH_ACTION_3}}
{{AUTH_HTTP_METHOD}}   {{AUTH_BASE_PATH}}/{{AUTH_ACTION_4}}
{{AUTH_HTTP_METHOD}}   {{AUTH_BASE_PATH}}/{{AUTH_ACTION_5}}
```

### Auth Flow

```
{{AUTH_FLOW_DESCRIPTION}}
```

### RBAC Roles

| Role | Access Level |
|------|-------------|
| `{{ROLE_1}}` | {{ROLE_1_DESC}} |
| `{{ROLE_2}}` | {{ROLE_2_DESC}} |
| `{{ROLE_3}}` | {{ROLE_3_DESC}} |

## {{BACKEND_SECTION_TITLE}}

```
{{BACKEND_ROOT}}/
├── {{BACKEND_SRC_DIR}}/
│   ├── {{BACKEND_API_DIR}}/         # Route handlers
│   │   ├── {{BACKEND_API_FILE_1}}
│   │   ├── {{BACKEND_API_FILE_2}}
│   │   ├── {{BACKEND_API_SUBDIR}}/
│   │   │   ├── {{BACKEND_SUBDIR_FILE_1}}
│   │   │   ├── {{BACKEND_SUBDIR_FILE_2}}
│   │   │   └── {{BACKEND_SUBDIR_FILE_3}}
│   │   ├── {{BACKEND_DEPS_FILE}}
│   │   └── {{BACKEND_HEALTH_FILE}}
│   ├── {{BACKEND_CORE_DIR}}/
│   │   ├── {{BACKEND_CORE_FILE_1}}
│   │   ├── {{BACKEND_CORE_FILE_2}}
│   │   └── {{BACKEND_CORE_FILE_3}}
│   ├── {{BACKEND_MODELS_DIR}}/
│   │   └── {{BACKEND_MODEL_FILES}}
│   ├── {{BACKEND_SCHEMAS_DIR}}/
│   │   └── {{BACKEND_SCHEMA_FILES}}
│   ├── {{BACKEND_SERVICES_DIR}}/
│   │   └── {{BACKEND_SERVICE_FILES}}
│   └── {{BACKEND_TASKS_DIR}}/
├── {{BACKEND_MIGRATIONS_DIR}}/
│   └── {{BACKEND_MIGRATIONS_SUBDIR}}/
│       └── {{BACKEND_MIGRATION_FILES}}
├── {{BACKEND_TESTS_DIR}}/
│   ├── {{BACKEND_TESTS_UNIT_DIR}}/
│   └── {{BACKEND_TESTS_INTEGRATION_DIR}}/
└── {{BACKEND_CONFIG_DIR}}/

{{MODULES_DIR}}/
├── {{MODULE_DIR_1}}/
├── {{MODULE_DIR_2}}/
├── {{MODULE_DIR_3}}/
└── ... ({{TOTAL_MODULES}} total)
```

## {{FRONTEND_SECTION_TITLE}}

```
{{FRONTEND_ROOT}}/
├── {{FRONTEND_SRC_DIR}}/
│   ├── {{FRONTEND_COMPONENTS_DIR}}/
│   │   └── {{FRONTEND_COMPONENT_FILES}}
│   ├── {{FRONTEND_PAGES_DIR}}/
│   │   └── {{FRONTEND_PAGE_FILES}}
│   ├── {{FRONTEND_STORES_DIR}}/
│   │   └── {{FRONTEND_STORE_FILES}}
│   ├── {{FRONTEND_LIB_DIR}}/
│   │   └── {{FRONTEND_LIB_FILES}}
│   └── {{FRONTEND_TYPES_DIR}}/
├── {{FRONTEND_TESTS_DIR}}/
└── {{FRONTEND_CONFIG_FILE}}
```

## Error Handling

{{ERROR_FORMAT}} — All API errors follow the {{ERROR_STANDARD}} format.

| Exception | Status | Type URI |
|-----------|--------|----------|
| {{EXCEPTION_1}} | {{STATUS_1}} | {{TYPE_URI_1}} |
| {{EXCEPTION_2}} | {{STATUS_2}} | {{TYPE_URI_2}} |

## Rate Limiting

- {{RATE_LIMIT_RULE_1}}
- {{RATE_LIMIT_RULE_2}}
- {{RATE_LIMIT_BACKEND}}

## CI/CD Pipeline

- **Platform**: {{CI_PLATFORM}}
- **{{CI_DETAILS}}**: {{CI_DESCRIPTION}}

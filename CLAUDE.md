# Project Guidelines

## API Documentation Rules

When writing or updating API reference documentation, the following internal fields must be **excluded** from response examples and response field definitions:

- `dataset_id`
- `memory_id`
- `memory_project_id`
- `memory_org_id`
- `created_by`
- `document_count`
- `memory_count`

These fields are returned by the actual API but should not be exposed in the public OpenAPI documentation.

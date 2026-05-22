# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **MemoryLake AI documentation site**, built with [Mintlify](https://mintlify.com). It contains product guides, feature documentation, and API reference for the MemoryLake platform (model routing, document/memory management, team collaboration).

## Development Commands

```bash
# Preview docs locally (requires Mintlify CLI: npm i -g mintlify)
mintlify dev

# Check for broken links and formatting issues
mintlify broken-links
```

There is no build step, test suite, or linter beyond Mintlify's own validation. Changes are previewed with `mintlify dev` and deployed on merge.

## Architecture

- **Framework**: Mintlify — all content is `.mdx` files with YAML frontmatter + Mintlify components
- **Navigation/config**: `docs.json` — defines site theme, the two top-level tabs (`Guides` and `API reference`), page ordering, and all page groups. **Any new page must be registered here to appear on the site.** Page entries use the path without the `.mdx` extension (e.g., `features/model-router/overview`). Guide pages go under the `Guides` tab; OpenAPI reference pages go under `API reference`.
- **Content structure**:
  - Root `.mdx` files: Getting Started section (overview, principles, quickstart, authentication)
  - `features/model-router/`: Model Router guides (API key creation, usage, integrations, error handling, reliability, multimodal)
  - `features/memorylake/`: MemoryLake guides (document management, connectors, project management, memories, MCP servers)
  - `features/memorylake/api-reference/`: OpenAPI reference docs — `authentication`, `library/` (item CRUD + upload), `projects/`, `memories/` (this folder holds **both** the Documents and Memories endpoint groups, plus memory conflicts), `errors`, `rate-limits`
  - `features/team-collaboration/`: Team roles, permissions, invitations, quotas, analytics
- **Static assets**: `images/` for screenshots, `logo/` for brand SVGs, `favicon.svg`

## Writing Conventions

- Cursor rules in `.cursor/rules.md` define the full Mintlify technical writing style and component reference. Consult it for component usage (`Steps`, `Tabs`, `CodeGroup`, `ParamField`, `ResponseField`, `Expandable`, `Frame`, etc.) and content quality standards.
- Every `.mdx` page must start with YAML frontmatter containing `title` and `description`.
- Use second person ("you"), active voice, present tense.
- Wrap images in `<Frame>` components. Use `<CodeGroup>` for multi-language examples.
- For API docs, use `<ParamField>` for parameters and `<ResponseField>` for response fields. Use `<RequestExample>`/`<ResponseExample>` for endpoint examples.

## API Documentation Rules

When writing or updating API reference documentation, the following internal fields must be **excluded** from response examples and response field definitions:

- `dataset_id`
- `memory_id`
- `memory_project_id`
- `memory_org_id`
- `created_by`
- `document_count`
- `memory_count`

These fields are returned by the actual API but should not be exposed in the public documentation.

## Key Details

- Base API URL: `https://app.memorylake.ai/openapi/memorylake`
- Console URL: `https://app.memorylake.ai`
- Model Router is OpenAI-protocol compatible — docs reference OpenAI SDK patterns alongside direct API calls

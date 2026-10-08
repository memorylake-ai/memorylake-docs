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

## Hidden API docs

Some API reference groups are published but hidden by default: they sit at their final place in the `API reference` tab, but readers only see them after clicking the `API reference` tab in the top bar 5 times within 3 seconds (clicking it 5 more times hides them again; the choice is remembered in the browser). This works on desktop only: on narrow screens the tabs collapse into a menu, so open hidden pages by direct link there. `hidden-docs.js` handles the clicks and `hidden-docs.css` does the hiding. This only keeps them out of sight. The repository is public, so never put anything confidential in a hidden page.

To hide a group:

1. Add `"tag": "Preview"` to the group in the `en` tree and `"tag": "预览"` in the `zh` tree. These two tag values are reserved for hidden groups.
2. Add `noindex: true` to the frontmatter of every page in the group, which keeps it out of site search, the sitemap, `llms.txt` and the AI assistant.
3. Add `hideFooterPagination: true` to the public pages right before and after the group in the tab's page order. Otherwise their previous/next links point into the hidden group.
4. Run `python3 scripts/check_hidden_docs.py` before opening the pull request. It reports a missing `noindex`, a missing `hideFooterPagination`, and a `hideFooterPagination` that is no longer needed.

To publish a hidden group, remove its `tag` and the `noindex` lines, then run the script. It lists the `hideFooterPagination` lines that you can remove.

Hidden groups so far: `Databases` / `数据库` under `MemoryLake API`.

## Key Details

- Base API URL: `https://app.memorylake.ai/openapi/memorylake`
- Console URL: `https://app.memorylake.ai`
- Model Router is OpenAI-protocol compatible — docs reference OpenAI SDK patterns alongside direct API calls

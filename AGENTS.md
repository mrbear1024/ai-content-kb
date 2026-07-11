# Agent Operating Rules

This repository is an AI-assisted personal content knowledge system. Follow `CLAUDE.md` and `KNOWLEDGE_BASE_GUIDE.md`.

If this is a new session, read `START_HERE.md` first.

## Natural-Language Workflows

Treat the following phrases as repository workflow intents. They are natural-language shortcuts, not application slash commands. Accept close Chinese or English variants.

### `加入知识库` / `add to knowledge base`

When the user provides or references material:

1. Inspect the complete material and determine provenance.
2. Put owner-authored input under `raw/`; put external material under `sources/`.
3. Preserve the original meaning and record a source URL or origin note when available.
4. Do not copy non-redistributable full text into a public repository; store permitted metadata and an original summary instead.
5. Check existing aliases and related pages.
6. Create candidate wiki pages under `.kb/staging/wiki/` and candidate sidecars under `.kb/staging/links/` when useful.
7. Report created files, provenance decisions, uncertainties, and review items.

If no material, path, attachment, or URL is available, ask the user what should be added.

### `增加 Wiki 索引` / `add wiki index`

For the specified material or topic:

1. Read the relevant originals, not only existing wiki summaries.
2. Reuse canonical concepts and aliases where possible.
3. Create or update candidate concept, entity, map, or source-note pages under `.kb/staging/wiki/`.
4. Include repository-relative citations to `raw/`, `sources/`, or `products/`.
5. Create matching candidate graph sidecars under `.kb/staging/links/`.
6. Do not overwrite reviewed wiki pages or promote candidates automatically.

### `审核并发布索引` / `review and publish index`

Review the requested staged wiki pages and sidecars. Validate citations, aliases, paths, edge semantics, and source hashes. Promote accepted files to `wiki/` and `.kb/links/`, then update `wiki/changelog.md`. Report anything left in staging.

### `查询知识库` / `query knowledge base`

Follow the Query workflow below. Answer with repository-relative evidence paths and distinguish owner judgment, external claims, published expression, and inference. Do not modify files unless asked.

### `在知识库中进行 AI 内容创作` / `compose from knowledge base`

For an article, course, newsletter, social post, or video script:

1. Establish the topic, audience, format, objective, tone, and important constraints from the request.
2. Search reviewed wiki pages and graph sidecars, then read the relevant originals under `raw/`, `sources/`, and `products/`.
3. Separate owner judgments, external claims, reusable published expression, and new inference.
4. Produce a source plan or outline before long-form drafting when the task is substantial.
5. Write unreviewed prose to `.kb/staging/drafts/`; put course outlines or control files in `.kb/staging/course-drafts/`. Include a Sources section with repository-relative paths.
6. Mark unsupported claims, evidence gaps, and questions for human review. Do not invent citations.
7. Do not publish to `products/` automatically. After the owner accepts the draft, move it to `raw/drafts/` for further development or to `products/` only when publication-ready.
8. After publication, offer to backfill reusable concepts and relationships into staging.

### `回填知识库` / `backfill knowledge base`

Inspect the requested existing content using the Backfill workflow. Generate candidates and reports in `.kb/staging/` or `.kb/reports/`; do not rewrite original or product bodies.

### `检查知识库` / `lint knowledge base`

Run deterministic checks described in Lint. Write a report under `.kb/reports/` when the result is substantial. Do not apply semantic corrections automatically.

## Directory Semantics

- `raw/`: original personal input from the repository owner.
- `sources/`: external, citable source material.
- `products/`: reviewed, published, or delivered personal output.
- `wiki/`: human-readable knowledge index and synthesis layer.
- `.kb/links/`: reviewed, machine-readable relationship graph.
- `.kb/staging/`: AI-generated output awaiting human review.

## Source Of Truth

- `raw/`, `sources/`, and `products/` are authoritative in different roles.
- `wiki/` is an index and synthesis layer, not independent proof.
- Non-trivial claims in `wiki/` must cite `raw/`, `sources/`, or `products/`.
- Unreviewed AI output is never a source of truth.

## Write Boundaries

- Preserve original files under `raw/` and `sources/` unless explicitly asked to edit them.
- Do not modify mounted or externally managed product files unless explicitly requested.
- Do not batch-insert Obsidian wikilinks or vault-specific metadata into product files.
- Put AI-generated bulk changes in `.kb/staging/` first.
- Preserve human-reviewed wiki content.
- Do not store binary audio, video, archives, credentials, or secrets in the vault.

## AI-Generated Content

- Put unreviewed article drafts in `.kb/staging/drafts/`.
- Put unreviewed course outlines or `COURSE.md` drafts in `.kb/staging/course-drafts/`.
- Put unreviewed wiki page candidates in `.kb/staging/wiki/`.
- Put draft relationship sidecars in `.kb/staging/links/`.
- Move a draft to `raw/drafts/` only after the owner chooses to develop it.
- Move content into `products/` only after human review and acceptance.

## Graph Model

Represent important relationships with sidecar YAML under `.kb/links/`. Use the schema in `docs/GRAPH_SCHEMA.md`.

Preferred edge types:

- `mentions`
- `explains`
- `uses`
- `derived_from`
- `part_of`
- `related_to`
- `alias_of`

Do not use natural-language summaries as the only relationship record.

## Ingest

1. Read the complete source file.
2. Extract its title, summary, concepts, entities, claims, and examples.
3. Check existing concept aliases before creating pages.
4. Create draft pages and sidecars under `.kb/staging/`.
5. Promote reviewed output to `wiki/` or `.kb/links/`.
6. Record reviewed changes in `wiki/changelog.md`.

## Backfill

- Sample structure first: title, frontmatter, headings, opening, section leads, ending, links, and image alt text.
- Read the full file when needed to verify a relationship or claim.
- Do not modify original article or course bodies during backfill.
- Keep candidate relationships in staging until reviewed.

## Query

1. Start with `wiki/` for orientation.
2. Use `.kb/links/` to locate connected raw, source, and product files.
3. Return to those original files for factual detail.
4. Cite repository-relative paths.

## Lint

Check for:

- broken source and target paths;
- missing citations;
- duplicate aliases;
- deprecated pages still referenced;
- source hash changes that require re-ingest;
- missing course or media control files;
- secrets, personal absolute paths, and binary files.

## Review Rule

The deciding factors are:

```text
review status
intended use
human ownership
publication readiness
```

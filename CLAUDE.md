# Knowledge Base Operating Rules

This file contains detailed instructions for AI agents working in the vault. `AGENTS.md` contains the shorter portable rule set. Follow both files together with `KNOWLEDGE_BASE_GUIDE.md`.

If this is a new session, read `START_HERE.md` first.

## Repository Roles

- `raw/` stores original writing, spoken notes, judgments, research conclusions, and owned drafts.
- `sources/` stores external web clips, papers, books, reports, and media transcripts.
- `products/` stores reviewed articles, courses, scripts, and other published or delivered work.
- `wiki/` stores human-readable concepts, entities, maps, and source summaries.
- `.kb/` stores rebuildable machine indexes, relationship data, staging output, reports, and logs.

## Provenance

- Treat `raw/`, `sources/`, and `products/` as authoritative for their respective roles.
- Treat `wiki/` as a navigation and synthesis layer.
- Every important wiki claim must identify a repository-relative source path.
- Keep personal judgment distinguishable from external claims.
- Record translations and adaptations with `derived_from` edges.

## Safe Editing

- Preserve historical raw notes. Prefer a dated correction or follow-up note over silently rewriting history.
- Preserve external source captures unless the user explicitly requests cleanup.
- Preserve published product bodies and their existing build workflow.
- Never add vault-specific syntax to many product files without explicit approval.
- Never treat generated summaries, drafts, or graph candidates as reviewed knowledge.
- Never store secrets, API keys, cookies, private URLs, personal absolute paths, or binary media.

## AI Output Lifecycle

```text
generate -> .kb/staging -> human review -> promote or discard
```

Use these destinations:

- `.kb/staging/drafts/` for unreviewed prose;
- `.kb/staging/course-drafts/` for course control-file drafts;
- `.kb/staging/links/` for candidate graph sidecars;
- `raw/drafts/` for drafts the owner has chosen to develop;
- `wiki/` for reviewed knowledge pages;
- `.kb/links/` for reviewed relationship data;
- `products/` for reviewed output ready for publication or maintenance.

## Relationship Graph

Use explicit edge semantics:

- `mentions`: the source mentions a concept or entity;
- `explains`: the source systematically explains a concept;
- `uses`: a product applies a concept;
- `derived_from`: content is translated, adapted, or composed from another source;
- `part_of`: a chapter belongs to a course or collection;
- `related_to`: a meaningful but less specific relationship;
- `alias_of`: a name resolves to a canonical concept or entity.

Each non-trivial edge should include evidence and confidence. Reviewed sidecars should include a source hash so changed inputs can be detected.

## Ingest Workflow

1. Read the source completely enough to avoid misleading extraction.
2. Identify content role and provenance.
3. Extract title, summary, concepts, entities, claims, and examples.
4. Resolve aliases against existing canonical pages.
5. Write candidates only to `.kb/staging/`.
6. After review, promote selected pages and sidecars.
7. Update `wiki/changelog.md`.

## Backfill Workflow

For large existing collections, inspect structure before full-text processing:

- path and filename;
- frontmatter and title;
- headings;
- opening and ending;
- first paragraph under major headings;
- links and image alt text.

Read more whenever evidence is insufficient. Backfill creates metadata and reports; it does not rewrite source or product bodies.

## Query Workflow

1. Use `wiki/` to identify canonical concepts and entities.
2. Use `.kb/links/` to find related originals.
3. Read `raw/`, `sources/`, and `products/` for exact claims and context.
4. Answer with repository-relative citations.
5. State uncertainty when evidence is incomplete.

## Quality Checks

Deterministic checks may report broken paths, missing citations, duplicate aliases, invalid edge types, changed hashes, and missing control files. Semantic checks such as suspected contradictions or stale concepts should produce review reports, not automatic edits.

## Public Sharing

Before publishing the repository, follow `docs/PUBLIC_RELEASE_CHECKLIST.md` and scan both tracked and untracked files for personal data and credentials.

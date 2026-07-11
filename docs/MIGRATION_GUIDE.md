# Migrating an Existing Knowledge Base

Use this guide to migrate an Obsidian vault, Markdown notes directory, article repository, course collection, or mixed personal knowledge base into `ai-content-kb`.

[简体中文](MIGRATION_GUIDE.zh-CN.md)

## Safety model

Do not reorganize the legacy vault in place. Keep it read-only and migrate through a reversible sequence:

```text
backup -> read-only inventory -> mapping review -> pilot copy -> validation -> batches -> cutover
```

Default rules:

- copy before moving;
- never use destructive synchronization such as `rsync --delete`;
- classify by provenance and review state before topic;
- put AI-generated mapping, wiki pages, and graph data in staging;
- migrate 20–50 representative files before scaling;
- commit each accepted batch separately;
- retain the old vault until recovery and query tests pass.

## Content mapping

| Legacy content | Destination |
|---|---|
| Owner-authored notes and judgments | `raw/notes/` |
| Owner research conclusions | `raw/research/` |
| Drafts selected for further development | `raw/drafts/` |
| Web clips and third-party articles | `sources/clips/` |
| Papers, books, and external reports | matching `sources/` directory |
| External media transcripts | `sources/media/` |
| Reviewed published articles, courses, and scripts | `products/` |
| Reviewed concepts, entities, maps, and source notes | `wiki/` |
| Summaries with uncertain provenance or review state | `.kb/staging/wiki/` |
| Unreviewed AI drafts | `.kb/staging/drafts/` |

Do not map a mixed folder as one unit. Classify individual files when a directory contains owner notes, external captures, published work, and generated summaries together.

## Migration record

Create a candidate mapping under:

```text
.kb/staging/migration/migration-map.csv
```

Recommended columns:

```csv
old_path,new_path,role,sha256,status,review_note
```

Use statuses such as `inventoried`, `needs_review`, `approved`, `copied`, `validated`, and `skipped`. Do not commit private absolute paths to a public repository.

## Codex workflow

### 1. Inventory without changes

```text
Migrate legacy knowledge base: inventory the provided legacy vault in read-only mode.
Do not move, copy, delete, or modify files.
Classify samples as raw, sources, products, wiki, or needs_review.
Report file types, links, attachments, frontmatter, duplicates, privacy risks,
copyright risks, and a recommended 20–50 file pilot.
Write .kb/reports/migration-inventory.md and stop for review.
```

### 2. Generate and review the mapping

Create a proposed old-path to new-path map under `.kb/staging/migration/`. Record provenance reasoning and mark ambiguous files `needs_review`. Review classification before copying anything.

### 3. Pilot migration

Copy a representative sample containing owner notes, external sources, products, old wiki pages, attachments, aliases, and complex links. Preserve bodies and required frontmatter. Do not batch-add new wikilinks.

### 4. Validate

Compare counts, hashes or bodies, frontmatter, links, and attachments. Check role classification, duplicate aliases, missing citations, secrets, and copyright risk. Report findings before applying semantic corrections.

### 5. Expand in batches

For each batch:

```text
review mapping -> copy -> generate staged candidates -> validate -> human acceptance -> Git commit
```

### 6. Cut over

Use the new vault as primary only after critical content is mapped, sampled originals match, important links and attachments open, wiki claims trace to originals, unreviewed output remains staged, publication workflows still work, and backup restoration has been tested.

## Obsidian considerations

- Do not blindly copy the entire `.obsidian/` directory.
- Treat workspace files, plugin caches, authentication state, and recent-file state as local data.
- Migrate portable settings such as themes, hotkeys, or graph colors selectively.
- Preserve existing link text first; generate an unresolved-link report before rewriting links.
- Keep attachment relationships intact and exclude audio/video binaries.
- Remember that Obsidian visualizes Markdown links and tags; `.kb/links/*.yaml` remains a separate machine graph.

## Acceptance checklist

- [ ] The legacy vault has an independent read-only backup.
- [ ] Every migrated file has an old path, new path, role, and status.
- [ ] External captures are not misclassified as owner-authored raw input.
- [ ] Source material preserves provenance.
- [ ] Products contain only reviewed output.
- [ ] Uncited legacy wiki pages remain staged.
- [ ] Generated output did not bypass review.
- [ ] Important links and attachments resolve.
- [ ] Sampled file bodies or hashes match.
- [ ] No secrets, private paths, caches, or unlicensed material were published.
- [ ] Every accepted batch has an independent Git commit.
- [ ] Real queries can return from the wiki to original evidence.

See the [Chinese migration guide](MIGRATION_GUIDE.zh-CN.md) for a more detailed walkthrough, prompts, manual copy examples, common mistakes, and rollback guidance.

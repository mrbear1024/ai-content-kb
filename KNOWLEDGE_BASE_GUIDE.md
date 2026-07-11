# Knowledge Base Guide

This is not a generic notes folder. It is a provenance-aware content system designed for collaboration between a human owner and AI agents.

## Directory guide

### `raw/`

Use for original personal input: notes, voice transcripts, judgments, research conclusions, ideas, and owned drafts.

Keep historical input stable. When a view changes, add a correction or follow-up rather than erasing the earlier record.

### `sources/`

Use for external, citable material: web clips, papers, books, third-party reports, and external media transcripts.

Distinguish the source author's claims from the repository owner's views. Store transcripts and metadata instead of audio or video binaries.

### `products/`

Use for reviewed and accepted output: articles, courses, booklets, scripts, and media projects.

Product repositories may be mounted or managed separately. Knowledge-base automation must not interfere with their publishing workflows.

### `wiki/`

Use for concepts, entities, domain maps, and high-value source summaries. Wiki pages should help people browse and understand the collection.

The wiki is not independent evidence. Important statements must point back to original files.

### `.kb/`

Use for machine-readable and rebuildable data:

- `links/`: reviewed graph sidecars;
- `staging/`: AI output awaiting review;
- `reports/`: health, ingest, backfill, and lint reports;
- `logs/`: automation logs;
- `metadata/`: local or supplemental metadata.

## Daily workflows

### Capture an idea

```text
raw/notes/YYYY-MM-DD-topic.md
```

Do not force every thought into a polished article immediately.

### Save an external source

```text
web page or transcript -> sources/ -> staged ingest -> reviewed wiki and links
```

Respect copyright and redistribution rights. A public repository should usually contain original summaries and metadata rather than full copyrighted captures.

### Write a new product

```text
orient in wiki
  -> locate evidence through .kb/links
  -> read original raw/source/product files
  -> develop an outline
  -> create and review the product
  -> backfill reusable concepts and relationships
```

### Process AI output

AI-generated content always starts in `.kb/staging/`. Review it for accuracy, provenance, duplication, ownership, tone, and publication readiness before promoting it.

## Principles

1. Original material remains traceable.
2. Personal judgment and external claims remain distinguishable.
3. Published content is protected from vault automation.
4. AI output is isolated until reviewed.
5. Human-readable pages and machine-readable relationships have different jobs.
6. Binary media, secrets, and local machine details stay outside the repository.
7. The system is judged by reliable answers and reuse, not by note or link counts.

# Architecture

## Design goal

The project turns a file-based content collection into a traceable system that AI can ingest, query, compose from, and lint without overwriting originals or silently promoting generated text.

## Layers

| Layer | Purpose | Authoritative? | Typical writer |
|---|---|---:|---|
| `raw/` | Original personal input | Yes, for owner intent | Human |
| `sources/` | External evidence | Yes, for source claims | Import or human |
| `products/` | Reviewed output | Yes, for published expression | Human + reviewed AI |
| `wiki/` | Concepts, entities, maps, summaries | No; cites originals | Human-reviewed AI |
| `.kb/` | Graph, staging, reports, logs | Rebuildable | Scripts and AI |

## Why source-first

A wiki-first system encourages summaries to become detached from evidence. This design keeps original input, external evidence, and published expression as distinct authoritative roles. The wiki remains useful precisely because it does not pretend to replace them.

## Why sidecar relationships

Obsidian links are useful for navigation but do not reliably express relationship type, evidence, confidence, review state, or source version. YAML sidecars preserve those fields without modifying source and product files.

## Why staging is a first-class layer

AI output is cheap to generate and expensive to trust. A dedicated staging layer makes review status visible in the file system and prevents bulk generation from becoming accepted knowledge by accident.

## Core lifecycle

```text
capture/import
  -> classify provenance
  -> ingest or backfill
  -> generate staged candidates
  -> human review
  -> promote selected wiki pages and graph sidecars
  -> query and compose
  -> backfill reusable knowledge from reviewed products
```

## Object model

The minimal model has four reusable node classes:

- `source`: a citable external input;
- `concept`: a reusable idea;
- `entity`: a person, organization, product, tool, paper, or project;
- `product`: reviewed output owned by the repository owner.

Raw notes remain a distinct provenance role even when represented as graph nodes.

Start with the edge vocabulary in [GRAPH_SCHEMA.md](GRAPH_SCHEMA.md). Add more node or edge types only when real queries require them.

## Product isolation

Published repositories may have their own frontmatter, build tools, and directory conventions. Link them through sidecars or local mounts. Do not turn a publication repository into an Obsidian implementation detail.

## Rebuildability

Machine indexes and reports should be reproducible from original files and reviewed rules. Losing `.kb/` should be inconvenient, not catastrophic. Losing `raw/`, `sources/`, or `products/` is data loss.

## Scaling path

1. Validate the information roles with Markdown and YAML.
2. Add deterministic schema and path linting.
3. Add a file manifest with hashes and incremental updates.
4. Validate a small article and course backfill.
5. Add full-text or vector retrieval only for demonstrated query needs.
6. Consider a graph database only when YAML query and maintenance costs justify it.

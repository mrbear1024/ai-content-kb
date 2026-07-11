# Graph Sidecar Schema

The relationship graph lives in YAML files separate from source and product bodies.

## Minimal sidecar

```yaml
schema_version: 1
source: "sources/clips/example-source.md"
source_type: source
title: "Example Source"
review_status: reviewed
reviewed_at: "2026-01-01"
source_hash: "sha256:replace-with-the-reviewed-file-hash"
edges:
  - type: explains
    target: "wiki/concepts/Example Concept.md"
    target_type: concept
    confidence: high
    evidence: "The source defines the concept and discusses its main components."
```

## Required top-level fields

| Field | Meaning |
|---|---|
| `schema_version` | Schema version used by the file |
| `source` | Repository-relative path of the subject file |
| `source_type` | Raw note, source, product, concept, entity, or another declared type |
| `title` | Human-readable title |
| `review_status` | `draft` or `reviewed` |
| `edges` | List of explicit relationships |

Reviewed files should also contain `reviewed_at` and `source_hash`.

## Edge fields

| Field | Meaning |
|---|---|
| `type` | Controlled relationship type |
| `target` | Canonical repository path or identifier |
| `target_type` | Type of target node |
| `confidence` | `low`, `medium`, or `high` |
| `evidence` | Short explanation supporting the edge |

## Edge vocabulary

| Edge | Use when |
|---|---|
| `mentions` | The subject refers to a concept or entity without substantial explanation |
| `explains` | The subject systematically defines or teaches a concept |
| `uses` | A product applies a concept, method, or source |
| `derived_from` | The subject is translated, adapted, or composed from the target |
| `part_of` | The subject belongs to a course, collection, or larger product |
| `related_to` | A meaningful relationship exists but no more specific edge fits |
| `alias_of` | The subject name resolves to a canonical target |

## Conventions

- Prefer repository-relative paths over machine-specific absolute paths.
- Keep one canonical concept or entity page per identity.
- Use aliases for alternate names rather than duplicate pages.
- Evidence should explain why the relationship exists, not repeat the edge label.
- Keep draft sidecars under `.kb/staging/links/`.
- Promote reviewed sidecars to `.kb/links/` without changing their source meaning.
- Re-review a sidecar when its recorded source hash no longer matches.

# AI Content Creation Guide

Use `ai-content-kb` as a foundation for articles, courses, newsletters, social posts, and video scripts.

[简体中文](CONTENT_CREATION_GUIDE.zh-CN.md)

## Principle

Retrieve and verify originals before drafting. Keep generated work in staging until a human decides that it belongs in the active writing or publishing workflow.

```text
define the brief
  -> locate reviewed knowledge in wiki and graph
  -> read originals in raw, sources, and products
  -> separate owner judgment, external claims, prior expression, and inference
  -> create a source plan and outline
  -> write to .kb/staging/drafts/
  -> review facts, attribution, duplication, and style
  -> develop in raw/drafts/ or publish to products/
  -> backfill reusable knowledge
```

## 1. Define the brief

Provide the topic, audience, format, length, objective, tone, required owner viewpoints, and source constraints.

```text
Compose from the knowledge base:
Write a 2,500-word article for AI product managers about Context Engineering.
Explain how it differs from Prompt Engineering.
Prioritize my reviewed judgments and verified external sources.
Cite repository paths for important claims.
Create a source plan and outline before drafting.
```

## 2. Inventory evidence and viewpoints

Codex should use `wiki/` and `.kb/links/` for orientation, then verify context in the original files.

| Content role | Location | Use in creation |
|---|---|---|
| Owner judgment | `raw/` | Position, original ideas, and experience |
| External evidence | `sources/` | Facts, research, definitions, and third-party cases |
| Published expression | `products/` | Reusable frameworks, cases, and prior wording |
| Knowledge index | `wiki/` | Fast discovery of concepts and materials |

Ask for lists of usable claims, claims needing evidence, duplicated prior material, and opportunities for new ideas.

## 3. Outline before drafting

Map every section to its conclusion, examples, and source paths. After approval, write unreviewed prose to `.kb/staging/drafts/` with `Sources` and `Needs human review` sections.

## 4. Review the draft

Check:

1. factual support;
2. attribution of owner judgment, external claims, and inference;
3. duplication with existing products;
4. structure, tone, and fit for the publishing context.

```text
Review this staging draft. Report missing evidence, overreach, duplicated prior content,
unclear attribution, and decisions I need to make. Do not rewrite it yet.
```

## 5. Develop, publish, and backfill

Move accepted work to `raw/drafts/` for further development or to `products/` only when publication-ready. Keep unresolved drafts in staging. After publication, generate staged wiki and relationship candidates without modifying the published body.

The same lifecycle applies to course outlines, video scripts, newsletters, and social posts. Course control files belong in `.kb/staging/course-drafts/` before review.

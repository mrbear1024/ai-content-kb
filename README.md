<div align="center">
  <h1>AI Content Knowledge Base</h1>
  <p><strong>A review-first, AI-native knowledge base for people who research, write, teach, and publish.</strong></p>
  <p>Markdown + Codex + Obsidian + typed YAML relationships</p>
  <p>
    <a href="https://github.com/mrbear1024/ai-content-kb/stargazers"><img src="https://img.shields.io/github/stars/mrbear1024/ai-content-kb?style=flat" alt="GitHub stars"></a>
    <a href="LICENSE"><img src="https://img.shields.io/github/license/mrbear1024/ai-content-kb" alt="MIT license"></a>
    <a href="AGENTS.md"><img src="https://img.shields.io/badge/Codex-AGENTS.md-111111" alt="Codex AGENTS.md"></a>
    <a href="https://obsidian.md/"><img src="https://img.shields.io/badge/Obsidian-compatible-7C3AED?logo=obsidian" alt="Obsidian compatible"></a>
  </p>
  <p>English · <a href="README.zh-CN.md">简体中文</a></p>
</div>

---

Most knowledge bases mix personal thinking, external evidence, published work, and AI-generated text in the same pile. This template gives each a clear role—and gives AI agents rules for working without silently polluting originals or publication workflows.

> **Originals stay trustworthy. The wiki stays readable. The graph stays queryable. AI output stays reviewable.**

![Example of a mature Obsidian knowledge graph](docs/assets/obsidian-graph-view.png)

> The screenshot shows a mature vault using this architecture. A fresh clone starts with a small synthetic example and grows as you add reviewed notes, links, and tags.

## Why this project

| Need | How this project handles it |
|---|---|
| Keep personal ideas distinct from external claims | Separate `raw/` and `sources/` provenance layers |
| Reuse published articles, courses, and scripts | Treat reviewed output as a first-class `products/` layer |
| Let people browse without turning summaries into proof | Use `wiki/` as a cited human interface |
| Give agents precise relationships | Store typed, evidenced YAML sidecars in `.kb/links/` |
| Prevent generated text from becoming truth by accident | Route AI output through `.kb/staging/` and human review |
| Work naturally with Codex | Ship durable workflows in root [`AGENTS.md`](AGENTS.md) |
| Keep Obsidian optional | Store everything as ordinary Markdown, YAML, JSON, and folders |

## Quickstart

```bash
git clone https://github.com/mrbear1024/ai-content-kb.git
cd ai-content-kb
```

No application is required to read the vault. Use plain Markdown, open the root as an Obsidian vault, or use the intended Codex workflow below.

### Open in Codex

1. Open the Codex desktop app.
2. Select `+` in **Projects**.
3. Choose **Use an existing folder**.
4. Select the cloned repository root.
5. Start a new task and say:

```text
Inspect the project structure, read the knowledge-base rules,
make no changes, and list the available workflows.
```

Codex reads repository `AGENTS.md` guidance before starting work. The root instructions then route it to the rest of this project's rules. [See the official Codex documentation](https://learn.chatgpt.com/docs/agent-configuration/agents-md).

> The phrases below are natural-language workflows defined by this repository, not native slash commands. No plugin is required.

## Talk to your knowledge base

| Say this | Default result |
|---|---|
| `加入知识库：这是我的原创笔记` | Store owner-authored input under `raw/` and create staged index candidates |
| `add to knowledge base: this attachment is an external source` | Preserve provenance under `sources/` and create staged candidates |
| `增加 Wiki 索引：刚才的材料` | Draft cited wiki pages and typed relationships in staging |
| `query knowledge base: <question>` | Navigate the wiki and graph, return to originals, and cite paths |
| `compose from knowledge base: <brief>` | Build a source plan, outline, and sourced staging draft |
| `migrate legacy knowledge base: inventory <path>` | Produce a read-only inventory and mapping proposal |
| `review and publish index: <staging path>` | Validate and promote accepted wiki and graph candidates |
| `lint knowledge base` | Report broken paths, citations, aliases, hash changes, and privacy risks |

## How it works

```mermaid
flowchart LR
    R["raw/\nOwner input"]
    S["sources/\nExternal evidence"]
    P["products/\nReviewed output"]
    T[".kb/staging/\nAI candidates"]
    W["wiki/\nHuman interface"]
    G[".kb/links/\nMachine graph"]

    R -->|ingest| T
    S -->|ingest| T
    P -->|backfill| T
    T -->|human review| W
    T -->|human review| G
    W -->|compose| P
    G -->|retrieve evidence| P
```

| Path | Role | Source of truth? |
|---|---|---:|
| `raw/` | Original notes, judgments, voice transcripts, owned research, active drafts | Yes—owner intent |
| `sources/` | External clips, papers, books, reports, and media transcripts | Yes—external claims |
| `products/` | Reviewed articles, courses, scripts, and delivered work | Yes—published expression |
| `wiki/` | Concepts, entities, maps, and high-value source notes | No—must cite originals |
| `.kb/links/` | Reviewed typed relationships with evidence and hashes | Rebuildable |
| `.kb/staging/` | Unreviewed AI prose, wiki pages, mappings, and graph candidates | No |

Read the [architecture rationale](docs/ARCHITECTURE.md) and [graph schema](docs/GRAPH_SCHEMA.md) for details.

## Core workflows

### Capture and index

```text
add to knowledge base: this attachment is an external source.
Preserve provenance, check existing aliases, and keep generated wiki and graph data in staging.
```

Review `.kb/staging/wiki/` and `.kb/staging/links/` before promotion.

### Create with AI

```text
Compose from the knowledge base:
Write an article for AI product managers about Context Engineering.
Separate my judgments from external claims, cite repository paths,
and create a source plan and outline before drafting.
```

Unreviewed prose goes to `.kb/staging/drafts/`, accepted work moves to `raw/drafts/` for development, and only publication-ready work belongs in `products/`.

[Read the content creation guide](docs/CONTENT_CREATION_GUIDE.md).

### Migrate an existing vault

```text
Migrate legacy knowledge base: inventory the provided directory in read-only mode.
Do not move, copy, delete, or modify files.
Propose a 20–50 file pilot and stop for review.
```

Migration follows backup → inventory → mapping review → pilot → validation → reviewed batches → cutover.

[Read the migration guide](docs/MIGRATION_GUIDE.md).

### Explore in Obsidian

Open the repository root with **Open folder as vault**, then select **Graph view** in the left ribbon. The included `.obsidian/graph.json` colors nodes by `raw`, `sources`, `products`, and `wiki`.

Obsidian visualizes Markdown links and tags. Typed `.kb/links/*.yaml` relationships are a separate machine layer and do not automatically become Obsidian lines.

## Review-first lifecycle

| State | Destination |
|---|---|
| Unreviewed article or script | `.kb/staging/drafts/` |
| Unreviewed course outline | `.kb/staging/course-drafts/` |
| Unreviewed concept, entity, map, or source note | `.kb/staging/wiki/` |
| Unreviewed relationship sidecar | `.kb/staging/links/` |
| Draft selected for human development | `raw/drafts/` |
| Reviewed knowledge and relationships | `wiki/` and `.kb/links/` |
| Reviewed, publication-ready output | `products/` |

The deciding factors are **review status, intended use, human ownership, and publication readiness**—not whether AI helped write the text.

## Documentation

| Document | Purpose |
|---|---|
| [Start here](START_HERE.md) | Reading order and core boundaries |
| [Agent rules](AGENTS.md) | Durable Codex workflows and safety rules |
| [Knowledge base guide](KNOWLEDGE_BASE_GUIDE.md) | Human-facing operating principles |
| [Architecture](docs/ARCHITECTURE.md) | Layer model, lifecycle, and scaling path |
| [Graph schema](docs/GRAPH_SCHEMA.md) | YAML sidecar fields and edge vocabulary |
| [Content creation](docs/CONTENT_CREATION_GUIDE.md) | Research, outlining, drafting, review, and backfill |
| [Migration](docs/MIGRATION_GUIDE.md) | Safe migration from an existing vault |
| [Public release checklist](docs/PUBLIC_RELEASE_CHECKLIST.md) | Privacy, secrets, rights, and release checks |

## Project scope

This repository is a **reference architecture and working template**, not a hosted knowledge-management application. Automation is intentionally minimal: validate the content roles and review workflow with files first, then add manifests, search, embeddings, or a graph database only when real queries justify them.

Obsidian and Codex are optional interfaces. The durable system is the repository itself.

## Contributing and support

- Found a problem or have a proposal? [Open an issue](https://github.com/mrbear1024/ai-content-kb/issues).
- Pull requests improving the information model, examples, schemas, lint rules, and documentation are welcome.
- Do not submit private notes, copyrighted captures, credentials, absolute home paths, or generated databases containing personal metadata.
- Before publishing a fork, use the [public release checklist](docs/PUBLIC_RELEASE_CHECKLIST.md).

Maintained by [mrbear1024](https://github.com/mrbear1024) and contributors.

## License

[MIT](LICENSE). This project is not affiliated with or endorsed by Obsidian or OpenAI.

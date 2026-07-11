# Start Here

This repository is an AI-assisted content knowledge system built from ordinary files.

Before making changes, read these documents in order:

1. [CLAUDE.md](CLAUDE.md) — detailed agent operating rules.
2. [AGENTS.md](AGENTS.md) — portable rules for AI coding and content agents.
3. [KNOWLEDGE_BASE_GUIDE.md](KNOWLEDGE_BASE_GUIDE.md) — human-facing usage guide.
4. [Architecture](docs/ARCHITECTURE.md) — design rationale and data flow.
5. [Graph schema](docs/GRAPH_SCHEMA.md) — sidecar relationship format.

Core directory rule:

```text
raw/       original personal input
sources/   external source material
products/  reviewed published or delivered output
wiki/      human-readable index and synthesis
.kb/       machine-readable graph, staging, reports, and logs
```

AI content rule:

```text
unreviewed AI output  -> .kb/staging/
human-developed draft -> raw/drafts/
reviewed knowledge    -> wiki/ and .kb/links/
reviewed output       -> products/
```

Do not bulk-edit source or product content. Do not promote AI output without human review.

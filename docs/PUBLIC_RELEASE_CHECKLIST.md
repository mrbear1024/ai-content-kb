# Public Release Checklist

Use this checklist before publishing a vault, template, example, or generated report.

## Identity and local environment

- [ ] Remove names, email addresses, phone numbers, account handles, and private organization names.
- [ ] Remove home-directory paths and other absolute local paths.
- [ ] Remove local mount targets, private repository URLs, hostnames, and device names.
- [ ] Remove timestamps or metadata that reveal more than intended.

## Secrets

- [ ] Scan tracked and untracked files for API keys, tokens, cookies, passwords, and `.env` files.
- [ ] Inspect Git history, not just the current working tree.
- [ ] Revoke any credential that was ever committed.
- [ ] Exclude generated databases and logs that may retain deleted values.

## Content rights

- [ ] Remove copyrighted full-text captures that cannot be redistributed.
- [ ] Confirm licenses for images, transcripts, datasets, and code samples.
- [ ] Replace private source material with synthetic examples.
- [ ] Attribute third-party material as required by its license.

## Personal knowledge

- [ ] Remove private notes, health or financial information, client data, and confidential research.
- [ ] Remove graph edges and aliases that reveal private interests or relationships.
- [ ] Inspect AI-generated summaries for accidentally reproduced private context.
- [ ] Inspect Obsidian configuration, caches, backups, exports, and trash folders.

## Repository quality

- [ ] Verify every link from `README.md`, `AGENTS.md`, and `START_HERE.md`.
- [ ] Confirm examples use repository-relative paths.
- [ ] Confirm staged content is clearly labeled as unreviewed.
- [ ] Confirm the stated license applies to everything included.
- [ ] Clone the repository into a clean directory and follow the quick start.

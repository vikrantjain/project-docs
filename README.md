# project-docs

A Claude Code plugin that packages authoring and review guidelines for the documents a project
ships with: READMEs and runbooks.

## What it does

Two skills, loaded automatically when you ask Claude to create, update, or review the matching
document:

- **`readme`** — rules for what belongs in a README, what belongs elsewhere, and how far it goes
  before sending the reader on. Anchored on one test: a README is read, not followed.
- **`runbook`** — rules for what a runbook may contain, how each step is worded, and which
  sections it must carry. Anchored on the opposite test: a runbook is followed, not read.

The two skills cross-reference each other, so procedural content is kept out of READMEs and
design rationale is kept out of runbooks.

## Requirements

Claude Code. No other tools, languages, or services.

# CLAUDE.md

## What this repository is

A Claude Code plugin whose entire payload is prose. `.claude-plugin/plugin.json` is the manifest;
`skills/<name>/SKILL.md` is the deliverable. There is no build, no test suite and no lint step, so
no change here can be verified by running anything. To exercise one, install the plugin locally and
ask Claude to write or review the matching documentation.

## Editing a skill

Every rule in a `SKILL.md` must be derivable from that file's anchoring sentence, the bold sentence
directly under the title. A rule that is good advice but does not follow from the anchor belongs in
another skill, or nowhere.

A rule a reviewer could act on belongs in the review checklist that ends the file too. A rule with
no way to detect its violation is unfinished.

Widen the frontmatter `description` when a change widens what the skill covers. The description is
the only thing that loads the skill at the right moment, and a skill that fails to load fails
silently.

## Rule numbers are an API

Rules are cited by number from another skill, from inside their own list, and from the review
checklist below it. Inserting a rule mid-list renumbers everything after it and silently breaks
those citations. Before renumbering, grep every skill for `rule [0-9]` and fix what the shift
moved. Then bump the version in `plugin.json`, because a citation held outside this repo cannot be
grepped.

Each pair of skills partitions one decision, and each boundary is stated on both sides of itself:
what the reader executes goes to the runbook rather than the README, and what explains one block of
code goes beside that code rather than into either document. A change to where one of those lines
falls has to land in both files that state it, or they will contradict each other.

## Scope

The plugin covers READMEs, runbooks and the comments in the source, and `README.md` says so. A
fourth documentation type means a new skill directory, its own anchoring sentence, a decision about
which existing skill it takes content from, and an edit to `README.md`.

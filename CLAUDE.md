# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A Claude Code plugin whose entire payload is prose. There is no source code, no build, no test
suite and no lint step. `.claude-plugin/plugin.json` is the manifest; `skills/<name>/SKILL.md` is
the deliverable. Changing this repository means editing rules, so the quality bar is the wording
of a rule rather than anything a tool can check.

To exercise a change, install the plugin locally and ask Claude to write or review the matching
document. There is no faster loop than that.

## How a skill is built

Each skill is one `SKILL.md` with three parts, and they are load-bearing in this order:

1. **The anchoring sentence**, directly under the title. `readme` is anchored on "A README is
   read, not followed"; `runbook` is anchored on the opposite, "A runbook is followed, not read".
   The paragraph after it tells the reader to settle uncovered cases from that sentence alone.
   Every rule in the file must be derivable from it. A rule that is good advice but does not
   follow from the anchor belongs in the other skill, or nowhere.
2. **`## The rules`** — a numbered list. The lead sentence of each rule is bold and is the rule
   itself; what follows is the smallest justification that makes it actionable.
3. **`## Reviewing an existing <document>`** — a numbered checklist for auditing a document that
   already exists. It is not a restatement of the rules. It is the order in which to apply them,
   and it names what counts as the finding.

The frontmatter `description` is what makes the skill load at the right moment. It names the
artifact under each name a user might use for it ("a runbook, a setup guide, an operational
procedure") and the verbs that should trigger it (creating, updating, reviewing). Edit it when a
change widens what the skill covers.

## Rule numbers are an API

Rules are cited by number from outside the list they live in: `readme` rule 3 cites `runbook`
rule 7. Inserting a rule mid-list renumbers everything after it and silently breaks those
citations. Before renumbering, grep both skills for `rule [0-9]` and fix what the shift moved.
This has already broken once — `readme` cited runbook rule 6, the development-environment rule,
when it meant rule 7, the split-from-the-README rule.

Two more consequences of the same thing:

- A rule change that a reviewer could act on belongs in the review checklist too. A rule with no
  way to detect its violation is unfinished.
- The two skills are deliberately coupled. They partition one decision: what the reader executes
  goes to the runbook, what orients them stays in the README. A change to where that line falls
  has to land in both files or the two will contradict each other.

## Scope

The plugin covers READMEs and runbooks. `README.md` states that as the whole of it. Adding a
third document type means a new skill directory, a new anchoring sentence, and a decision about
which existing skill it takes content from.

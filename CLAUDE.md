# CLAUDE.md

## What this repository is

A Claude Code plugin whose entire payload is prose. There is no build, no test suite and no lint
step, so no change here can be verified by running anything. To exercise one, install the plugin
locally and ask Claude to write or review the matching documentation.

## Editing a skill

Every rule in a `SKILL.md` must be derivable from that file's anchoring sentence, the bold sentence
directly under the title. A rule that is good advice but does not follow from the anchor belongs in
another skill, or nowhere.

A rule a reviewer could act on belongs in the review checklist that ends the file too. A rule with
no way to detect its violation is unfinished.

A rule that is true of one tool and not of another does not go in the rule list. It goes in a
reference file beside the `SKILL.md`, named after the tool, which the skill tells the reader to
open when the project uses that tool. `skills/architecture-doc/plantuml.md` is the pattern.

A skill's reader is an agent working in another project. The plugin is installed in a cache the
next update overwrites, so no rule tells that reader to add a reference file or change the skill.
Adding one is a change to this repo. An example drawn from this repo's own vocabulary reads as
specific to this repo, so examples come from an ordinary codebase.

Each pair of skills partitions one decision. The README's paragraph on how the skills
cross-reference each other lists those boundaries. Each boundary is stated in both skills it
separates, so a change to where one falls has to land in both files, or they will contradict each
other.

Widen the frontmatter `description` when a change widens what the skill covers. The description is
the only thing that loads the skill at the right moment, and a skill that fails to load fails
silently.

## Rule numbers are an API

Rules are cited by number from another skill, from inside their own list, and from the review
checklist below it. Checklist items are cited by number too. Inserting a rule or an item mid-list
renumbers everything after it and silently breaks those citations. Before renumbering, run
`grep -rniE '(rules?|items?) [0-9]' skills` and fix what the shift moved. A narrower pattern such
as `rule [0-9]` misses `Rule 13`, `rules 4 and 7` and `items 1 to 10`. Then bump the version in
`plugin.json`, because a citation held outside this repo cannot be grepped.

## Scope

The plugin covers READMEs, runbooks, configuration references, architecture documents, the
comments in the source and commit messages. `README.md` says so, and so does the `description` in
both `plugin.json` and `marketplace.json`. A seventh documentation type means a new skill
directory, its own anchoring sentence, a decision about which existing skill it takes content from,
and an edit to all three of those.

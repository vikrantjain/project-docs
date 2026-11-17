# project-docs

A Claude Code plugin that packages authoring and review guidelines for the documentation a
project ships with: READMEs, runbooks, configuration references, architecture documents, the
comments in the source, and commit messages.

It is for people who want Claude held to a fixed standard on those six rather than to its
defaults. The drift it prevents is the usual one: procedures accumulating in READMEs, design
rationale accumulating in runbooks, work logs accumulating in comments and design documents, and
every one of them filling up with text nobody goes looking for.

## What it does

Six skills, loaded automatically when you ask Claude to create, update, or review the
matching documentation. Each also loads when Claude writes or reviews a code change that touches
that documentation:

- **`readme`** — rules for what belongs in a README, what belongs elsewhere, and how far it goes
  before sending the reader on. Anchored on one test: a README is read, not followed.
- **`runbook`** — rules for what a runbook may contain, how each step is worded, and which
  sections it must carry. Anchored on the opposite test: a runbook is followed, not read.
- **`config-doc`** — rules for the table of settings, ports and defaults an operator
  looks up, where it lives, and how each row is checked against the running system. Anchored
  on a third test: a configuration reference is looked up, not read through.
- **`code-comments`** — rules for what a comment, docstring or module header carries, and what
  git, a test or the architecture document carries instead. Anchored on a fourth test: code
  documentation says what the code cannot. It loads on every code change and every code
  review, because nearly every change touches a comment.
- **`architecture-doc`** — rules for what an architecture, design or HLD document may contain,
  what it sends to git, the runbook or a comment instead, and how its diagrams are drawn in
  whichever tool the project uses.
  Anchored on a fifth test: it describes the system as designed, not the work that produced it.
- **`commit-message`** — rules for what a commit's subject and body say, and for the history
  and deliberation it records that the other five send away. It loads on every commit and every
  review of a branch or pull request. Anchored on a sixth test: a commit message explains the
  change, not the code.

Each skill ends with a review checklist covering every one of its rules, so asking for a review
applies the same standard that writing does.

The six skills cross-reference each other, so procedural content is kept out of READMEs,
design rationale is kept out of runbooks and lands in the architecture document, a table of
settings lands in the configuration reference rather than in a runbook, and an explanation of one
block of code lands beside that block rather than in any of them. The account of how the work
got there lands in the commit message, the one record that carries its date. The project README is the
entry point: the runbook, the configuration reference and the architecture document are each one
link away from it.

## Requirements

Claude Code. No other tools, languages, or services.

## Install

```
/plugin marketplace add vikrantjain/project-docs
/plugin install project-docs@project-docs
```

The repo is its own plugin marketplace, so nothing else needs to be set up. In
`project-docs@project-docs`, the first name is the plugin and the second is the marketplace.

To confirm it is working, ask for a review in any project:

```
> review the README in this repo
```

Claude loads the `readme` skill and reports findings against its checklist, numbered in the order
the checklist applies them.

## Support

File bugs and questions at
[github.com/vikrantjain/project-docs/issues](https://github.com/vikrantjain/project-docs/issues).

## License

[MIT](LICENSE)

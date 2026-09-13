# project-docs

A Claude Code plugin that packages authoring and review guidelines for the documents a project
ships with: READMEs and runbooks.

It is for people who want Claude held to a fixed standard on those two documents rather than to
its defaults. The drift it prevents is the usual one: procedures accumulating in READMEs, design
rationale accumulating in runbooks, and both filling with sections nobody goes looking for.

## What it does

Two skills, loaded automatically when you ask Claude to create, update, or review the matching
document:

- **`readme`** — rules for what belongs in a README, what belongs elsewhere, and how far it goes
  before sending the reader on. Anchored on one test: a README is read, not followed.
- **`runbook`** — rules for what a runbook may contain, how each step is worded, and which
  sections it must carry. Anchored on the opposite test: a runbook is followed, not read.

Each skill ends with a review checklist covering every one of its rules, so asking for a review
applies the same standard that writing does.

The two skills cross-reference each other, so procedural content is kept out of READMEs and
design rationale is kept out of runbooks.

## Requirements

Claude Code. No other tools, languages, or services.

## Install

```
/plugin marketplace add vikrantjain/my-claude-plugins
/plugin install project-docs@my-claude-plugins
```

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

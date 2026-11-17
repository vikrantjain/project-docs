# Conventional Commits

Read this file when the project uses Conventional Commits. A `commitlint.config.*` or
`.commitlintrc*` file, a `semantic-release` or `release-please` configuration, or recent subjects
of the form `type(scope): description` each show that it does. What follows holds for this
convention only. The rules in the skill hold as well.

## The subject

The subject is `type(scope): description`. The scope is optional, and a `!` before the colon
marks a breaking change. The type is one of those the project's commitlint configuration allows.
Without a configuration, use the types the project's recent commits use. Where those are too few
to tell, use the common set: `feat`, `fix`, `docs`, `refactor`, `perf`, `test`, `build`, `ci`,
`chore`, `style` and `revert`.

The type is read by tools, not only by people. A release tool usually turns `feat` into a minor
version and `fix` into a patch. Choosing `chore` for a change users will notice leaves it out of
the release notes.

The description after the colon is still imperative, as rule 1 asks. The common commitlint
configuration rejects a description that starts with a capital letter, so it starts in lower
case. The length limit applies to the whole subject, prefix included. Use the
`header-max-length` the project configures, or 72 characters without one.

## Breaking changes

A `!` in the subject or a `BREAKING CHANGE:` footer is what the release tool reads to raise the
major version. Either one satisfies the tool. Neither satisfies rule 6 on its own: the body
still says what anyone depending on the old behaviour must now do.

## Reverts

A revert uses the `revert` type, and its body names the hash of the commit it reverts. Rule 2
still applies: the body says why the change is being reverted.

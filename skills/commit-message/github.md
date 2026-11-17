# GitHub

Read this file when the project's pull requests are merged on GitHub. What follows holds for
GitHub only. The rules in the skill hold as well.

## Whether the project squash-merges

The repository's settings say which merge methods are allowed:

```
gh api 'repos/{owner}/{repo}' --jq '{squash: .allow_squash_merge, merge: .allow_merge_commit, rebase: .allow_rebase_merge, squash_title: .squash_merge_commit_title, squash_message: .squash_merge_commit_message, merge_title: .merge_commit_title, merge_message: .merge_commit_message}'
```

The fields are missing where the token cannot see the repository's settings. Where more than one
method is allowed, the settings also do not say which one the team uses. The default branch's
history says both:

```
git log --first-parent --format='%p | %s' -20 origin/<default-branch>
```

A squash commit has one parent, and its subject usually ends in the pull request's number, as
in "Add retry on conflict (#123)". A merge commit has two parents, and its subject starts with
"Merge pull request #".

## Where the squash message comes from

`squash_merge_commit_title` sets the subject:

- `PR_TITLE`: the pull request's title is the subject, so rule 1 applies to the title.
- `COMMIT_OR_PR_TITLE`: a branch with one commit gives that commit's subject. A branch with more
  gives the pull request's title. Rule 1 applies to both.

`squash_merge_commit_message` sets the body:

- `PR_BODY`: the description is the body. Rule 11 applies to it.
- `COMMIT_MESSAGES`: the branch's commit messages are joined into the body. Each one is part of
  the final message, so rules 1 to 10 apply to each, and rule 12 applies to the branch.
- `BLANK`: the body starts empty, and the merger writes it at merge time. The rules apply to
  what they write.

## Where the merge commit's message comes from

A merge commit keeps every commit on the branch, so rules 1 to 10 and rule 12 apply to them as
they stand. The merge commit adds one message of its own.

`merge_commit_title` sets its subject:

- `MERGE_MESSAGE`: the classic "Merge pull request #123 from branch-name". No rule applies to it.
- `PR_TITLE`: the pull request's title is the subject, so rule 1 applies to the title.

`merge_commit_message` sets its body:

- `PR_BODY`: the description is the body. Rule 11 applies to it.
- `PR_TITLE`: the body repeats the title, so rule 1 applies to the title.
- `BLANK`: the body starts empty, and the merger writes it at merge time. The rules apply to
  what they write.

The merger can edit the message in the merge dialog whatever the setting says. Rule 11's
"Review notes" section is deleted there.

---
name: commit-message
description: Guidelines for writing and reviewing a commit message: what its subject and body say, what it records that no document or comment may, and what it sends to a comment or the architecture document instead. Use before committing, amending, squashing or rewording a commit, and before writing a pull request description or merging a pull request. Also use before reviewing a branch, a pull request or a commit history, even when the task is about the code.
---

# Commit messages

A commit message **explains the change, not the code**. Its reader runs `git log` or
`git blame` long after the work. They hold the diff, and nothing that explained it. The plan the
change came from, the conversation that shaped it and the ticket it closed are gone or changed by
then. The message is the one record that stays attached to the change and carries its date.
Tools read it too: a changelog generator or a release script that cannot parse it drops the
change. Every rule below serves that one sentence.

The reader is a person or an agent deciding what to do next with code that already exists. Both
need the same thing, so no rule below distinguishes them.

When a case here isn't covered, decide it from that sentence: does this explain the change, or
does it describe the code as it now is?

## The rules

1. **The subject says what the change does.** Write it in the imperative mood, as a command the
   commit carries out: "Retry the upload on a duplicate-key conflict", not "Retried the upload"
   and not "Fixes upload". It is one line, short enough that `git log --oneline` shows it whole.
   Rule 9 sets its length and punctuation.

2. **The body says why.** Give the problem the change answers, as someone would have met it: the
   failure, the request or the limit that was hit. Then say how this change answers it, at the
   level of the approach rather than the edits; see rule 5. Removing code is a change like any
   other, and the body says why it went. The `code-comments` skill, rule 6, sends removed code
   here rather than leaving it commented out. The `runbook` skill, rule 5, and the
   `architecture-doc` skill, rule 2, send here the failure that a step or a design change
   answered. Leave the body out only when the subject already
   carries the whole reason, as in a typo fix.

3. **The message stands alone.** No "as planned", no "step 3", no "per the discussion", and no
   "see the ticket" standing where a reason should be. Each points at something the reader
   cannot open. A ticket or issue link may follow the reason. It never replaces it.

4. **Record the deliberation the documents may not hold.** What was tried and reverted, which
   alternative was rejected and why, and the measurement that settled a choice are each true of
   one moment. The commit is the only record that carries that moment's date. The
   `code-comments` skill, rule 3, the `architecture-doc` skill, rule 3, and the `readme` skill,
   rule 7, send this material here. Keep what would change a later decision, not every turn the
   work took.

5. **Never narrate the diff.** A list of the files touched, or an account of each edit in turn,
   repeats what `git show` prints. Delete the sentence and read the diff: if nothing was lost,
   it was narration.

6. **Name the consequences the diff hides.** A changed default, a removed option, a broken
   interface, a renamed endpoint and a migration someone must run are each easy to miss in a
   diff. Say each one in the body, and say what anyone depending on the old behaviour must now
   do.

7. **A fact about the code as it now is goes where the code's reader looks.** An editor about to
   break a constraint reads the code, not `git blame`, because nothing has made them suspicious
   yet. The message may say how the constraint arose. The constraint itself sits in a comment
   beside the code it constrains; see the `code-comments` skill, rule 3. Where the project has
   an architecture document, a change to how the system is divided updates it in the same
   commit. The message then says why the design changed; see the `architecture-doc` skill,
   rule 3.

8. **The message accounts for the whole diff.** `git blame` sends a reader here from any line
   the commit touched, so the message gives the reason for every part of it. Changes that serve
   one reason read as one change: a renamed function, its callers and the changelog entry the
   rename forced. Changes with unrelated reasons each get their own reason in the body. Where
   they have not yet been committed, separate commits serve the reader better. Two kinds of
   change go in a commit of their own anyway. A moved file is one: git recognises a move only
   while the file stays at least half the same, so a move and a rewrite in one commit end the
   file's history for `git log --follow`. A reformat, or an identifier renamed across many
   files, is the other: in a commit of its own it can be listed in `.git-blame-ignore-revs`, and
   blame then skips it.

9. **Follow the project's format, not the author's.** Subject prefixes, subject length, trailers
   such as `Signed-off-by` or `Change-Id`, issue-closing keywords and line wrapping are the
   project's to decide. Its `CLAUDE.md` or contributing guide names its convention. Where
   neither does, follow the format of its recent commits. Take their format, not their content:
   a history without bodies sets no standard for the body. Where the recent commits agree on
   nothing, keep the subject to about 50 characters and never more than 72, end it without a
   full stop, and wrap the body at 72 columns. A trailer the user's own instructions require,
   such as `Co-Authored-By` on an agent's commit, is added as well. Some conventions have a
   reference file beside this skill, such as `conventional-commits.md`, holding what applies
   only to that convention. Read it before writing. A convention with no file here is followed
   from the project's guide and its recent commits.

10. **The body is as long as the reason.** The test is not the length. It is whether deleting any
    one sentence costs the reader something the diff and the subject do not give them. A reason
    that takes three paragraphs gets three. A sentence restating the subject goes.

11. **Where a pull request becomes the message, write it as one.** A project that squash-merges
    may take the squash commit's subject from the pull request's title and its body from the
    description. Whether it does depends on the host's settings, and `github.md` beside this
    skill says how to tell on GitHub. Where it does, the title follows rule 1. The description
    opens with the part that becomes the message, written by these rules. What only the reviewer
    needs follows under a heading of its own, such as "Review notes": how the change was tested,
    screenshots, and open questions. The merger deletes that section from the message at merge
    time.

12. **Fold fix-ups in before merging.** Where every commit on the branch reaches the history,
    merged as it is or joined into a squash message, a commit such as "Fix lint" or "Address
    review comments" explains nothing to a later reader. Fold it into the commit it fixes before
    the merge. `git commit --fixup=<hash>` marks it, and
    `GIT_SEQUENCE_EDITOR=: git rebase -i --autosquash <base>` folds it in without opening an
    editor. A branch others have already pulled is not rewritten. Its fix-ups instead get
    messages that stand alone.

## Reviewing commit messages

A finding on a commit already pushed to a shared branch is reported, not fixed. Rewording it
rewrites history that others have pulled. Fix findings by rewording only on a branch nobody else
has, and only when asked.

Review the commits the branch adds, `<base>..HEAD`. Where the pull request description becomes
the squash message, review its opening part as that message instead. Apply items 1 to 10 to it,
against the diff of the whole branch.

1. Run `git log --format='%h %s' <base>..HEAD` and read each subject with its body and diff
   hidden. Say what each commit does. A subject that does not let you is the finding. So is a
   subject not in the imperative mood, and one that breaks the length or punctuation rule 9
   sets.
2. Read each body and say why the change was made. A body that leaves you unable to is the
   finding. A missing body is the finding wherever the subject does not carry the whole reason.
3. Find every reference the reader cannot open: "the plan", "as discussed", a step number, or a
   bare link standing where a reason should be. Each one is the finding.
4. For each commit that changes a design choice or reverts an earlier attempt, check that the
   message says what was rejected and why. Where that account sits in a comment or a document
   instead, the finding belongs to the `code-comments` or `architecture-doc` review.
5. Find every sentence that narrates the diff: a list of files, or an edit-by-edit account. Each
   one is the finding.
6. Read each diff for a changed default, a removed option, a broken interface, a moved name or a
   migration. One the message does not name is the finding.
7. For each constraint a message states, check that the code carries it in a comment where an
   editor will see it. A constraint recorded only in the message is a finding against the code,
   not the message. Where the project has an architecture document, check that each design
   change updated it in the same commit.
8. Check each part of the diff against the message. A part whose reason the message does not
   give is the finding. A moved file that the same commit also rewrote is the finding. So is a
   reformat or a renamed identifier mixed into another change. Where a commit joins unrelated
   reasons, suggesting a split is a note, not a finding, and only for commits not yet pushed.
9. Check each message against the project's convention, and against that convention's reference
   file beside this skill where one exists. A trailer the project requires, and which is
   missing, is the finding. On the commits you wrote, so is a missing trailer that the user's
   instructions require.
10. Check each body sentence by sentence: delete the sentence, and ask what the reader lost. A
    sentence whose deletion costs nothing is the finding, whatever the body's length.
11. Where the description becomes the squash message, check that it opens with a part that would
    stand as that message. Reviewer notes mixed into that part are the finding. Notes under a
    heading of their own after it are not.
12. Where every commit on the branch reaches the history, find each commit that only fixes
    another commit on the same branch: "fix lint", "address review", "wip". One not folded in
    before the merge is the finding.
13. Settle anything the steps above do not cover against the opening sentence: does this explain
    the change, or does it describe the code as it now is?

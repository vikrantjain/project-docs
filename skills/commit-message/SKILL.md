---
name: commit-message
description: Guidelines for writing and reviewing a commit message: what its subject and body say, what it records that no document or comment may, and what it sends to a comment or the architecture document instead. Use before committing, amending, squashing or rewording a commit, and before merging a pull request whose description becomes the squash commit. Also use before reviewing a branch, a pull request or a commit history, even when the task is about the code.
---

# Commit messages

A commit message **explains the change, not the code**. Its reader runs `git log` or
`git blame` long after the work, holding the diff and nothing else. The plan the change came
from, the conversation that shaped it and the ticket it closed are gone or changed by then. The
message is the one record that stays attached to the change and carries its date. Tools read it
too: a changelog generator or a release script that cannot parse it drops the change. Every rule
below serves that one sentence.

The reader is a person or an agent deciding what to do next with code that already exists. Both
need the same thing, so no rule below distinguishes them.

When a case here isn't covered, decide it from that sentence: does this say why the code
changed, or does it describe the code as it now is?

## The rules

1. **The subject says what the change does.** Write it in the imperative mood, as a command the
   commit carries out: "Retry the upload on a duplicate-key conflict", not "Retried the upload"
   and not "Fixes upload". Keep it short enough that `git log --oneline` shows it whole: about
   50 characters, and never more than 72. It ends without a full stop.

2. **The body says why.** Give the problem the change answers, as someone would have met it: the
   failure, the request or the limit that was hit. Then say why this change answers it, wherever
   a reader could ask why not something simpler. Leave the body out only when the subject
   already carries the whole reason, as in a typo fix.

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
   interface, a renumbered rule and a migration someone must run are each easy to miss in a
   diff. Say each one in the body, and say what anyone depending on the old behaviour must now
   do.

7. **A fact about the code as it now is goes in the code.** An editor about to break a
   constraint reads the code, not `git blame`, because nothing has made them suspicious yet. The
   message may say how the constraint arose. The constraint itself sits in a comment beside the
   code it constrains; see the `code-comments` skill, rule 3. A change to how the system is
   divided updates the architecture document in the same commit, and the message says why the
   design changed; see the `architecture-doc` skill, rule 8.

8. **The message accounts for the whole diff.** `git blame` sends a reader here from any line
   the commit touched, so the message gives the reason for every part of it. Changes that serve
   one reason read as one change: a rule, the citations it moved and the version bump it forced.
   Changes with unrelated reasons each get their own reason in the body. Where they have not yet
   been committed, separate commits serve the reader better. A reformat or a mass rename is
   the exception worth splitting out anyway. In a commit of its own it can be listed in
   `.git-blame-ignore-revs`, and blame then skips it.

9. **Follow the project's format, not the author's.** Subject prefixes, trailers such as
   `Signed-off-by`, `Co-Authored-By` or `Change-Id`, issue-closing keywords and line length are
   the project's to decide. Its `CLAUDE.md` or contributing guide names its convention. Where
   neither does, follow the format of its recent commits. Where those agree on nothing, write a
   plain imperative subject and wrap the body at 72 columns. Each convention has one reference
   file beside this skill, such as `conventional-commits.md`, holding what applies only to it.
   Read that file before writing. A convention with no file gets one before its first commit, not
   a rule in this list.

10. **The body is as long as the reason.** The test is not the length. It is whether deleting any
    one sentence costs the reader something the diff and the subject do not give them. A reason
    that takes three paragraphs gets three. A sentence restating the subject goes.

11. **A squash-merged pull request's description becomes the message.** Where the project
    squash-merges, the commit message is written at merge time, from the pull request, and these
    rules apply to it then. What only the reviewer needed is cut from it before the merge: how
    the change was tested, screenshots, and replies to review comments.

## Reviewing commit messages

A finding on a commit already pushed to a shared branch is reported, not fixed. Rewording it
rewrites history that others have pulled. Fix findings by rewording only on a branch nobody else
has, and only when asked.

1. Run `git log --format='%h %s' <range>` and read each subject with its body and diff hidden.
   Say what each commit does. A subject that does not let you is the finding. So is a subject in
   the past tense, one over 72 characters, and one ending in a full stop.
2. Read each body and say why the change was made. A body that leaves you unable to is the
   finding. A missing body is the finding wherever the subject does not carry the whole reason.
3. Find every reference the reader cannot open: "the plan", "as discussed", a step number, or a
   bare link standing where a reason should be.
4. For each commit that changes a design choice or reverts an earlier attempt, check that the
   message says what was rejected and why. Where that account sits in a comment or a document
   instead, the finding belongs to the `code-comments` or `architecture-doc` review.
5. Cut every sentence that narrates the diff: a list of files, or an edit-by-edit account.
6. Read each diff for a changed default, a removed option, a broken interface, a moved number or
   a migration. One the message does not name is the finding.
7. For each constraint a message states, check that the code carries it in a comment where an
   editor will see it. A constraint recorded only in the message is a finding against the code,
   not the message. For each design change, check that the architecture document changed in the
   same commit.
8. Check each part of the diff against the message. A part whose reason the message does not
   give is the finding. Where a commit joins unrelated reasons, suggesting a split is a note,
   not a finding, and only for commits not yet pushed. A reformat or mass rename mixed into
   another change is the finding.
9. Check each message against the project's convention and against that convention's reference
   file beside this skill. A trailer the project or the user's instructions require, and which
   is missing, is the finding.
10. Check each body sentence by sentence: delete the sentence, and ask what the reader lost. A
    sentence whose deletion costs nothing is the finding, whatever the body's length.
11. Where the project squash-merges, read the pull request description as the message it will
    become. Testing notes, screenshots and replies to review comments left in it are the finding.
12. Settle anything the steps above do not cover against the opening sentence: does this say why
    the code changed, or does it describe the code as it now is?

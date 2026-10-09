---
name: code-comments
description: Guidelines for writing and reviewing code documentation — the comments, docstrings and header comments that sit in the source: what belongs beside the code, what belongs in git, a test or the architecture document, and how much each comment may say. Use before writing, updating or reviewing comments, docstrings or API documentation, and on any request to document or explain a piece of code in place.
---

# Code documentation

Code documentation **says what the code cannot**. The logic is already on the screen, and the
reader can run it. What they cannot recover by reading it is the intent behind it, the constraints
it has to keep, and the contract a caller must honour without opening the body. Each of those is
a fact about the code, which is the only subject a comment has. Not one run of it, and not the
week it was written in. Every rule below serves that one sentence.

The reader is a person picking the file up cold or an agent building context for an edit. Both
need the same thing, so no rule below distinguishes them.

The rules hold in any language. Where one names a construct a language does not have, it applies
to that language's nearest equivalent.

When a case here isn't covered, decide it from that sentence: is this a fact about the code that
the reader could not have read off it, or is it a fact about something else?

## The rules

1. **Say why, where the code already says what.** The comment gives the reason the code is as it
   is: the intent it serves, or the thing it must not break. A comment a reader could have
   written themselves from the lines beneath it has told them nothing.

2. **Document the contract of anything called from outside.** This is the one place where stating
   what the code does is required, because the caller never opens the body. Give what the caller
   has to know and cannot see: what must be true before the call, how failure is reported, what
   the caller is left holding afterwards, and any restriction on when or from where it may be
   called. What it still leaves out is how the body does it.

3. **Record the constraint, never the deliberation.** What survives a design discussion is the
   rule the next editor has to keep: retry only on a duplicate-key conflict, because any other
   failure may mean the write already landed. What does not survive is the account of reaching
   it. What was tried, what was reverted, who argued for which option and when all belong to git,
   which can date them. See the `readme` skill, rule 7, which keeps the same material off the
   front page.

4. **Never narrate the logic.** A comment that restates the line beneath it, or spells out the
   name of the thing it sits on, carries nothing. Delete it and read the code again: if nothing
   was lost, it was narration.

5. **No results of execution.** A comment carries no sample output, no timing, no count, no
   captured error text. Each of those describes one run of one version in one environment, and a
   later reader cannot tell whether it still holds. Output that the code guarantees belongs in a
   test, which fails when it stops being true. Output a follower needs in order to operate the
   system belongs in a runbook step; see the `runbook` skill, rule 4.

6. **Nothing parked for later.** Commented-out code and an unowned TODO are the same thing: text
   kept in the file because nobody decided. Git holds what was removed and can say when it went,
   and the tracker holds work not yet done. A TODO stays only when it names an owner and the item
   that tracks it.

7. **No system-level design.** A comment explains the code it sits on. How the parts fit
   together, why the system is divided this way, and which alternatives the design rejected
   belong in the architecture document, named in one line where a reader of this file would go
   looking for it. See the `readme` skill, rule 6. A comment that would have to change when a
   different file changes is in the wrong file.

8. **Every sentence earns its line.** The test is not the comment's length. It is whether
   deleting any one sentence costs the reader something they could not have read off the code. A
   long comment stays when every sentence passes that test, and a short one goes when none does.

9. **Put each comment where its reader is.** A comment at the head of a file says what the unit
   is for and how it sits against the ones beside it. A comment attached to a callable carries
   rule 2's contract. A comment inside a body answers a question that arises at one exact line,
   and sits against that line. A constraint written in the wrong place is found by nobody.

10. **A comment that disagrees with the code is a bug.** It is worse than no comment, because the
    reader trusts it. Change the comment in the same edit as the code it describes, and treat a
    mismatch found later as a defect rather than a wording nit.

## Reviewing existing code documentation

1. Read the file's comments with the code hidden, then say what the unit is for and what an
   editor must not break. Whatever you cannot say from the comments alone is what they are
   missing.
2. For anything reachable from outside the unit, check that the contract is there: what must be
   true before the call, how failure is reported, what the caller is left holding, and any
   restriction on when it may be called. An absent one of those is a finding as much as a surplus
   comment is.
3. Read each comment beside its code and delete every one you could have written from that code.
   That is narration, and a comment that only spells out the name above it is the same finding.
4. Cut every account of how the code came to be: sequences of attempts, "originally", "we used
   to", a name attached to a decision. Keep the constraint the discussion produced, written as a
   rule about the code rather than a history of the argument. Where that constraint is recorded
   nowhere else, it is not cut until it has been written down.
5. Cut every result of a run: sample output, timings, counts, captured error text. Where that
   output is what the code guarantees, the finding is a missing test rather than a surplus
   comment.
6. Cut commented-out code, and every TODO naming neither an owner nor a tracking item.
7. Cut comments whose subject is the system rather than the code they sit on. Anything that would
   have to change when a different file changes is in the wrong file: relocate it to the
   architecture document and leave the one line that points there.
8. Check each surviving comment sentence by sentence: delete the sentence, and ask what the reader
   lost. A sentence whose deletion costs nothing is the finding, whatever the comment's length.
9. Check placement: a contract buried inside a body, a line-level caveat sitting in the file
   header, a comment drifted away from the line it explains.
10. Check every comment against the code it describes. A name, argument, option, default or branch
    that no longer matches is a bug report, not a nit.
11. Settle anything the steps above do not cover against the opening sentence: is this a fact about
    the code that the reader could not have read off it, or is it a fact about something else?

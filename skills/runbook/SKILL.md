---
name: runbook
description: Guidelines for writing a runbook — what it may contain, how each step is worded, and which sections it must carry. Use before creating, updating or reviewing a runbook, a setup guide, an operational procedure or a "how to run this" document.
---

# Runbooks

A runbook is **followed, not read**. Someone who does not know the system opens it, types what
it says, and reaches a working end state. Every rule below serves that one sentence.

When a case here isn't covered, decide it from that sentence: does the follower need this in
order to do the next thing, or is it there because the author knew it?

## The rules

1. **Open with who and what.** Two or three sentences: what this runbook gets you, and who
   runs it. Where a run is triggered by something — an incident, a release, a rotation — say
   what.

2. **State what it was last verified against** — the date, and the versions or environment it
   was run on. It is the cheapest signal a reader has for whether to trust the rest.

3. **List prerequisites before the first step.** Access and credentials, installed tools with
   their versions, and every placeholder value the reader must supply. The reader gathers them
   once, at the top, rather than discovering a missing one at step 14.

4. **Setup starts from a fresh target environment and ends in a verified state.** The last step is a
   command whose expected output is shown. Any earlier step that can fail silently shows its
   expected result too, so the follower can tell success from failure before moving on. "It
   should work now" is not an end state.

5. **Only what the follower must do.** No architecture, no rationale for the design, no
   narration of what was built or what went wrong while building it. Two things this rule
   cuts: a paragraph explaining how the component works, and a note saying a step was added
   because of a bug last month. Link the architecture doc in one line instead.

6. **Not the development environment.** A runbook is for operating the system, not for preparing
   a machine to develop it. Installing a language toolchain, an IDE, editor settings or commit
   hooks belongs in a contributing guide. The prerequisites name the tool and the version the
   follower needs and link to its install instructions; they do not teach the install.

7. **Split from the README once the procedure outgrows a skim.** A small project's README
   quickstart is its setup runbook; a second copy is the one that goes stale. Move the steps out
   when they are longer than what someone reads to decide whether to care, and leave a link
   behind. What stays in the README is orientation — what the thing is, how it fits, where the
   docs are; see the `readme` skill. What moves is anything the reader executes.

8. **One line per step, saying what it does.** Add prose only where the step is non-obvious or
   destructive. A runbook where every step carries a paragraph stops being followable.

9. **One block per path, pasted whole.** A command block is copy-pastable as it stands: no prose
   interleaved, no line the reader must edit or uncomment mid-paste. Two ways to run a step are
   two blocks, each under a heading saying when to use it, and so is an optional-flag variant.
   Placeholders use one fixed convention — `<PROJECT_ID>` — and every placeholder appears in
   the prerequisites. Where a step cannot be a command (a click path, a console field), name
   the exact screen, the exact control and the exact value.

10. **Reference a script that already exists; never reproduce it.** Where the repository or the
    machine already carries the script, the step gives its path, the command that invokes it,
    the arguments to pass and the output to expect. A copy of the body in the runbook is a
    second version that drifts from the one that runs. Inline a block only for commands that
    exist nowhere else, and for those consider whether they should become a script.

11. **Mark destructive and irreversible steps, and say what they destroy.** This applies inside
    teardown as much as anywhere else.

12. **Never carry state in the reader's head.** A step that produces a value the follower needs
    later gives that value a name, and the later step refers to it by that name.

13. **Separate the execution flows.** If the system can be driven from a UI, from a terminal
    and from an automated pipeline, each gets its own section. Do not braid them into one
    sequence with conditional asides.

14. **Say which sections cannot be re-run.** The follower's default assumption is that repeating
    a section is safe. Where the underlying operation makes that false, the section says so.

15. **Say how to recover from a failure mid-run.** The follower who is stranded halfway has
    resources half-created, and needs to know whether to fix and resume, or tear down and start
    again. Name the resume point per section, or point at teardown.

16. **Troubleshooting is a section, not a scattering.** One at the end, or one per section when
    the runbook is long. Each entry is a symptom the follower can observe, then the fix.

17. **End with teardown.** Everything the runbook created, removed in an order that works,
    including anything created only on the failure paths.

## Reviewing an existing runbook

1. Read it as the follower: assume no knowledge of the system, and stop at the first step you
   could not execute from what is written above it. That step is the finding.
2. Check each command block for an unlisted placeholder and each step for a result the reader
   cannot verify.
3. Cut anything explaining the system rather than instructing the reader — the scope rule. It
   is not relocated unless it exists nowhere else.
4. Cut developer environment setup, and cut any block that reproduces a script the project
   already ships. Replace the reproduced block with the path and the invocation.
5. Confirm teardown covers what setup created, and refresh the verified-against line.

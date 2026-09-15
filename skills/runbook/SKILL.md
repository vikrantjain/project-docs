---
name: runbook
description: Guidelines for writing and reviewing a runbook — what it may contain, how it is organised into flows and files, how each step is worded, and which sections it must carry. Use before creating, updating or reviewing a runbook, a setup guide, an operational procedure or a "how to run this" document.
---

# Runbooks

A runbook is **followed, not read**. Someone who does not know the system opens it, types what
it says, and reaches a working end state. The system already exists, and what the follower does
is operate it. Every rule below serves that one sentence.

When a case here isn't covered, decide it from that sentence: does the follower need this in
order to operate the system, or is it there because the author knew it?

## The rules

Rules 14 to 18 govern the shape of the document: the flows it divides into, how each flow
section opens and what paths it holds, the setup they share, and the files it splits across.
The rest govern what goes inside a section. When starting a runbook from nothing, settle the
shape first.

1. **Open with who and what.** Two or three sentences: what this runbook gets you, and who
   runs it. Where a run is triggered by something, say what: an incident, a release, a
   rotation.

2. **State what it was last verified against.** Give the date, and the versions or environment
   it was run on. A follower who can see when it last worked knows how much of it to trust.

3. **List prerequisites before the first step.** Access and credentials, installed tools with
   their versions, and every placeholder value the reader must supply. The reader gathers them
   once, at the top, rather than discovering a missing one at step 14.

4. **Every section the follower runs ends in a verified state.** Its last step is a command
   whose expected output is shown. Any earlier step that can fail silently shows its expected
   result too, so the follower can tell success from failure before moving on. "It should work
   now" is not an end state.

5. **Only what the follower must do.** No architecture, no rationale for the design, no
   narration of what was built or what went wrong while building it. Two things this rule
   cuts: a paragraph explaining how the component works, and a note saying a step was added
   because of a bug last month. Link the architecture doc in one line instead.

6. **Not the development environment.** A runbook is for operating the system, not for preparing
   a machine to develop it. Installing a language toolchain, an IDE, editor settings or commit
   hooks belongs in a contributing guide. The prerequisites name the tool and the version the
   follower needs and link to its install instructions; they do not teach the install.

7. **Split from the README once the procedure outgrows a skim.** A small project's README
   quickstart is its setup runbook. A second copy is the one that goes stale. Move the steps out
   when they are longer than what someone reads to decide whether to care, and leave a link
   behind. What moves is anything the reader executes beyond the quickstart. What stays in the
   README is orientation: what the thing is, how it fits, where the docs are. See the `readme`
   skill.

8. **One line per step, naming what it achieves.** The line says what the follower gets and the
   command says how: "Restart the gateway so it picks up the new trust list", not "Run
   `docker compose restart gateway`". A follower who can see only the mechanism cannot tell
   whether the step has already run, what its failure costs, or whether it can be skipped. Where
   a flow is long enough that its steps are grouped under headings, each heading names the
   outcome of its group by the same test. Add prose only where the step is non-obvious or
   destructive. A runbook where every step carries a paragraph stops being followable.

9. **One block per path, pasted whole.** A command block is copy-pastable as it stands: no prose
   interleaved, no line the reader must edit or uncomment mid-paste. Two ways to run a step are
   two blocks, each under a heading saying when to use it. An optional-flag variant is a second
   block too. Where a step cannot be a command (a click path, a console field), name the exact
   screen, the exact control and the exact value.

10. **One placeholder convention, fixed across the runbook.** Write every placeholder
    `<PROJECT_ID>`. Every placeholder the follower supplies appears in the prerequisites under
    the same name it carries in the block.

11. **Reference a script that already exists; never reproduce it.** Where the repository or the
    machine already carries the script, the step gives its path, the command that invokes it,
    the arguments to pass and the output to expect. A copy of the body in the runbook is a
    second version that drifts from the one that runs. Inline a block only for commands that
    exist nowhere else.

12. **Mark destructive and irreversible steps, and say what they destroy.** This applies inside
    teardown as much as anywhere else.

13. **Never carry state in the reader's head.** A step that produces a value the follower needs
    later gives that value a name in the placeholder convention, and the later step refers to it
    by that name. A produced value is the one placeholder that does not belong in the
    prerequisites. The follower cannot supply it before the run, so it is introduced at the step
    that produces it.

14. **The top level is the list of flows the follower might come to run.** A follower arrives
    with a goal: issue a credential to a wallet and present it to a verifier, or rotate a
    signing key. Each goal is one section, named after what it achieves. Name it as the outcome
    the follower reaches, not as the subject the section covers: "Issue a credential to a wallet",
    not "Credential issuance". A section named after machinery, such as "Start the services" or
    "Call the token endpoint", is a step inside a flow rather than a top-level section. Where
    two flows drive the same services in different sequences, they are two sections, and the
    runbook says which one a first-time follower should run.

15. **Open each flow section with what the follower needs before starting it.** Name what the
    flow achieves, what must already have run, and the observable end state that proves it
    worked. Say whether the section can be re-run, and where to resume if it fails partway.
    Rules 19 and 20 say when each of those two is needed. Link the setup section for the
    preconditions instead of repeating its steps. A follower must be able to pick their section
    from these lines without reading the steps under any of them.

16. **Two ways to run one flow are two paths inside that flow's section.** A browser path, a
    terminal path and a pipeline invocation that reach the same end state go under headings
    saying when to use each, within the section for the flow they all perform. Splitting them
    at the top level makes the follower reconcile two sections to answer one question. Within a
    path, do not braid the alternatives back together with conditional asides.

17. **Write the shared setup once, ahead of the flows.** One section before them carries what
    every flow needs: the services running, the seeded data, the trust configuration. It starts
    from a target environment carrying nothing but the prerequisites, so a follower who has met
    those and done nothing else can run it start to finish. Setup that only one flow needs
    belongs in that flow's section, not in the shared one.

18. **Keep the runbook in one file unless its owner decides to split it.** One document needs no
    index and no cross-file links, so it is the default however many flows it holds. Where the
    flows have stopped fitting one read-through, propose the split and name the files it would
    produce. The split is proposed, never assumed.

    A split runbook lives in one `runbook/` folder, and no part of it lives outside: one file per
    flow that can be run on its own, one for the shared setup, one for teardown, and `index.md`
    as the only entry point. The index gives each flow its one-line statement of what it achieves
    and when to run it, and links it. Nothing the follower executes lives in the index. Each flow
    file links its prerequisites rather than copying them. Do not split finer than a flow. A file
    per service or per step scatters one procedure across the tree and puts the follower back to
    guessing which file to open.

19. **Say which sections cannot be re-run.** The follower's default assumption is that repeating
    a section is safe. Where the underlying operation makes that false, the section's opening
    lines say so.

20. **Say how to recover from a failure mid-run.** The follower who is stranded halfway has
    resources half-created, and needs to know whether to fix and resume, or tear down and start
    again. Name the resume point in the section's opening lines, or point at teardown.

21. **Troubleshooting is a section, not a scattering.** One at the end, or one per section when
    the runbook is long. Each entry is a symptom the follower can observe, then the fix.

22. **End with teardown.** Everything the runbook created, removed in an order that works,
    including anything created only on the failure paths.

23. **Say how long a step takes when it is long enough to look stuck.** A follower who does not
    know the system cannot tell a twelve-minute deploy from a hung one. Give the rough duration,
    and where the wait has an observable end, the thing to watch for.

## Reviewing an existing runbook

1. Read only the opening lines and the section headings. From those alone, say what the runbook
   gets you and which section you would open for each thing the system can do. A heading that
   names machinery rather than an outcome is the finding, and so are two headings that reach the
   same outcome by different means. A heading naming a topic rather than an outcome is the same
   finding: "Credential issuance" names a subject, "Issue a credential to a wallet" names what
   the follower gets.
2. Read each flow section's opening lines. They name what the flow achieves, what must already
   have run, and the end state that proves it worked; a missing one of those is the finding.
   Where the section cannot be re-run, or where a failure partway needs a named resume point,
   they say that too.
3. Name what is missing at the document level, not only what is surplus: who runs this and what
   triggers a run, the date and versions it was last verified against, and a prerequisites list
   naming access, credentials, tool versions and every placeholder the follower supplies. An
   absent one of those is a finding as much as a surplus section is.
4. Read it as the follower: assume no knowledge of the system, and stop at the first step you
   could not execute from what is written above it. That step is the finding.
5. Check each command block for a placeholder that is neither in the prerequisites nor produced
   by an earlier step, for prose interleaved between its lines, and for any line the reader must
   edit or uncomment before pasting. Check that every placeholder is written in one convention
   throughout. Where a step is not a command, check that it names the exact screen, the exact
   control and the exact value.
6. Check that two ways to reach one end state are two blocks or two paths, each under a heading
   saying when to use it, and that neither is braided back into the other by a conditional aside.
7. Check each step for a result the reader cannot verify, and for a wait long enough to look
   stuck that does not say how long it takes. Check that each value a step produces has a name,
   and that the later step refers to it by that name.
8. Read each step's line with its command hidden. A line naming only the mechanism, from which
   you cannot say what the follower gets, is the finding. So is a grouping heading inside a flow
   that names machinery. Then check each step is one line: a step carrying a paragraph where
   nothing is non-obvious or destructive is the finding.
9. Check that every destructive or irreversible step is marked and says what it destroys.
   Teardown is included.
10. Cut anything explaining the system rather than instructing the reader. That is rule 5, the
    scope rule. It is not relocated unless it exists nowhere else.
11. Cut developer environment setup, and cut any block that reproduces a script the project
    already ships. Replace the reproduced block with the path and the invocation.
12. Check that setup every flow needs sits in one section ahead of the flows, that it starts from
    an environment carrying nothing but the prerequisites, and that setup only one flow needs
    sits inside that flow.
13. Check that troubleshooting is a section rather than notes scattered through the steps. Each
    entry names a symptom the follower can observe, then the fix.
14. Check whether it should be one file at all. A runbook split across files whose owner did not
    ask for the split is the finding, and so is any part of a split runbook living outside the
    `runbook/` folder.
15. Where the runbook is split, check that `index.md` reaches every flow, that nothing the
    follower executes lives in it, and that no flow file repeats setup it could link to instead.
16. Check that the README links to this runbook rather than carrying a second copy of its steps.
17. Confirm teardown covers what setup created, and refresh the verified-against line.
18. Settle anything the steps above do not cover against the opening sentence: does the follower
    need this in order to operate the system, or is it there because the author knew it?

---
name: architecture-doc
description: Guidelines for writing and reviewing an architecture, design or HLD document — what it may contain, what it sends to a commit message, the runbook or a comment instead, and how its diagrams are drawn in whichever tool the project uses. Use before creating, updating or reviewing such a document, before adding a diagram to one in PlantUML, Mermaid or another tool, and when design rationale found in a README, a runbook or a comment needs a home. Also use before writing or reviewing a code change, diff or pull request that touches an architecture or design document, even when the task is about the code.
---

# Architecture documents

An architecture document **describes the system as designed, not the work that produced it**.
It reads the same whether it was written on the project's first day or its last. Every rule
below serves that one sentence. The diagram rules serve it too. A diagram that does not render
describes nothing. A second copy of an idea, whether a rendered image, a source file beside the
document or a second diagram, goes on showing the design as it was when the copy was made. A
diagram that needs caveats shows a system other than the one designed.

When a case here isn't covered, decide it from that sentence: is this the design as it stands,
or is it the story of how it got there?

## The rules

1. **Not a log of work done.** No narration of what was built, no status, no "done" markers,
   no progress report.

2. **Not a record of gotchas, failures or issues** hit while writing the document or while
   writing, building or deploying the code. One that a follower of the runbook can still meet
   goes in its troubleshooting section, as a symptom and its fix; see the `runbook` skill,
   rule 19. One still open goes in the issue tracker of the component it belongs to. The
   account of meeting it belongs in the message of the commit that dealt with it; see the
   `commit-message` skill, rule 2.

3. **Not a history.** Git holds that, and the message of each commit that changed the design
   says why it changed. See the `commit-message` skill, rules 4 and 7. The commit that changes
   how the system is divided updates this document too, so the document never describes a
   design the code has left. Describe only the current design: no "previously X, then we
   changed to Y", no revision log. A rejected alternative appears only as rationale inside a
   decision, never as chronology. The `readme` skill, rule 7, and the `code-comments` skill,
   rule 3, keep the same material off the front page and out of the source.

4. **No dates.** No planned dates and no release dates. A date ties a sentence to the day it
   was written, and the document reads the same on any day of the project.

5. **No plan for the work.** No implementation plan, project plan, timeline, estimate,
   milestone, phase, sprint or work breakdown. A design describes the system. A schedule is a
   commitment about the work, reviewed on different terms in a different document.

6. **Write in the present tense.** "The gateway validates the token", not "we added token
   validation". `we will`, `we did`, `next`, and `as of <date>` are the tells that the
   document has slipped into log mode.

7. **Explain, don't just present.** Where a part of the design exists to handle a problem,
   name the problem before the mechanism that handles it. State it as a problem the system
   faces now: "two workers can claim the same job", not "jobs were being run twice". Never put
   a problem and its mechanism side by side and leave the reader to work out the link. A flow
   that answers no problem is described as it runs, with no problem invented for it. State
   what the system does, not how sure the author is of it. A hedge leaves the reader unable to
   tell what the design is. Keep a metaphor only once it is this document set's own
   vocabulary. Until then it describes something other than the system.

8. **The subject is the whole system.** How the parts fit together, why the system is divided
   this way, and the alternatives the division rejected belong here and nowhere else. Three
   things that do not: anything the reader executes, which is the runbook's (see the `runbook`
   skill, rule 5), the table of settings an operator looks up, which is the configuration
   reference's (see the `config-doc` skill, rule 11), and the reason one block of code
   is as it is, which sits beside that code (see the `code-comments` skill, rule 7). The
   README names this document in one line rather than restating it; see the `readme` skill,
   rule 6.

9. **Never convey the same idea twice in different forms.** One interaction gets one diagram,
   not a sequence diagram and a flow diagram of the same thing. A passage that restates a
   diagram box by box is a second copy too.

10. **Draw in the project's tool, not the author's.** Use the diagram tool the project's
    `CLAUDE.md` names, since a diagram in the wrong one renders as an empty space with no error.
    Where it names none, use the tool the existing documents already use. Where there are no
    diagrams yet, ask rather than default. PlantUML and Mermaid each have a reference file
    beside this skill, `plantuml.md` and `mermaid.md`, holding what applies only to that tool.
    Read the one for the project's tool before drawing. A tool with no file here is drawn from
    its own documentation, and rule 13's render is the check.

11. **Embed every diagram in the markdown** as a fenced block whose language is the tool's
    name. No sibling diagram source files, no rendered PNG or SVG. One idea per diagram. A
    diagram that needs a paragraph of caveats is really two diagrams.

12. **Use only constructs the reader's renderer has.** Every renderer lags its tool's
    documentation, so a construct documented online may not exist where the reader looks, and
    one unknown construct fails the whole diagram rather than degrading. Prefer the oldest
    construct that expresses the thing. The tool's reference file names the ones known to
    fail.

13. **Render every diagram before it reaches the reader.** A markdown preview shows an empty space
    rather than an error anyone can read, so an unrendered diagram is an unverified one.
    Render against the same renderer the reader's preview uses, because that is what decides
    which constructs exist.

## Reviewing an existing architecture document

1. Read it asking on which day of the project it was written. If any sentence answers, that
   sentence is the finding: a status, a "done" marker, a date, a planned release, a milestone
   or a phase.
2. Cut every account of how the design came to be: "previously", "we changed to", a revision
   log, an attempt that was reverted. Keep a rejected alternative only where it sits inside
   the decision that rejected it, as the reason.
3. Cut every gotcha, failure and issue. Relocate one a follower can still meet to the runbook's
   troubleshooting section, as a symptom and its fix. Relocate one still open to the issue
   tracker. Relocate each only if it exists nowhere else.
4. Check each sentence for past or future tense about the project's own work. `we will`,
   `we did`, `next` and `as of` are the finding.
5. For each mechanism that exists to handle a problem, check that the problem is named before
   it, as one the system faces now. A problem told as something the project once had is the
   finding. So is a mechanism whose purpose the reader must work out from two things placed
   side by side. Then find every hedge about what the system does, and every metaphor the
   document set does not already use as a name. Each one is the finding.
6. Cut anything the reader executes, and relocate it to the runbook. Cut the rationale for one
   block of code, and relocate it to a comment beside that block. Cut a table of settings, and
   relocate it to the configuration reference, or to the README where the project has none.
   None of them is cut until it exists at its destination.
7. Name what is missing: how the parts fit together, why the system is divided this way, and
   what the division rejected. An absent one of those is a finding as much as a surplus
   section is. Where the review covers a change to how the system is divided, a document that
   the same commit left unchanged is the finding.
8. For each diagram, check that it carries one idea and needs no paragraph of caveats. Check
   that no other diagram or passage says the same thing.
9. Check that every diagram is in the tool the project's `CLAUDE.md` or its other documents
   name, as a fenced block in that tool's language, with no sibling file or rendered image.
   Then check it against that tool's reference file beside this skill. A construct that file
   names as failing is the finding.
10. Render every diagram against the reader's renderer. One that fails to render is the
    finding, whatever the markdown preview shows.
11. Settle anything the steps above do not cover against the opening sentence: is this the
    design as it stands, or is it the story of how it got there?

---
name: architecture-doc
description: Guidelines for writing and reviewing an architecture, design or HLD document — what it may contain, what it sends to git, the runbook or a comment instead, and how its diagrams are drawn. Use before creating, updating or reviewing such a document, before adding a diagram to one, and when design rationale found in a README, a runbook or a comment needs a home.
---

# Architecture documents

An architecture document **describes the system as designed, not the work that produced it**.
It reads the same whether it was written on the project's first day or its last. Every rule
below serves that one sentence. The diagram rules serve it too: a diagram that does not render
describes nothing.

When a case here isn't covered, decide it from that sentence: is this the design as it stands,
or is it the story of how it got there?

## The rules

1. **Not a log of work done.** No narration of what was built, no status, no "done" markers,
   no progress report.

2. **Not a record of gotchas, failures or issues** hit while writing the document or while
   writing, building or deploying the code. Those belong in the issue tracker, the runbook's
   troubleshooting section, or the component's own repo. See the `runbook` skill, rule 21.

3. **Not a history.** Git holds that. Describe only the current design: no "previously X,
   then we changed to Y", no revision log. A rejected alternative appears only as rationale
   inside a decision, never as chronology. The `readme` skill, rule 7, and the `code-comments`
   skill, rule 3, keep the same material off the front page and out of the source.

4. **No dates and no implementation plan.** No planned dates, no release dates, no project
   plan.

5. **No timelines, estimates, milestones, phases, sprints or work breakdown.** A design is a
   proposal; a schedule is a commitment. They are reviewed on different terms and belong in
   different documents.

6. **Write in the present tense.** "The gateway validates the token", not "we added token
   validation". `we will`, `we did`, `next`, and `as of <date>` are the tells that the
   document has slipped into log mode.

7. **Explain, don't just present.** Use plain language, but plain does not mean long. Lead
   with the flow, then say why it is a problem, then give the solution. Never put a flow and
   its solution side by side and leave the reader to work out what was wrong. Padding,
   restatement and hedging make a document boring long before they make it clear. Keep a
   metaphor only once it is this document set's own vocabulary.

8. **The subject is the whole system.** How the parts fit together, why the system is divided
   this way, and the alternatives the division rejected belong here and nowhere else. Two
   things that do not: anything the reader executes, which is the runbook's (see the `runbook`
   skill, rule 5), and the reason one block of code is as it is, which sits beside that code
   (see the `code-comments` skill, rule 7). The README names this document in one line rather
   than restating it; see the `readme` skill, rule 6.

9. **Prefer a diagram to a long passage** where it carries the same thing faster. Put a
   short summary under each one: two or three lines saying what to take from it, not a
   re-reading of every box. Never convey the same idea twice in different forms. One
   interaction gets one diagram, not a sequence diagram and a flow diagram of the same thing.

10. **Draw structure with PlantUML C4**, included from the bundled standard library in
    exactly this form:

    ```
    !include <C4/C4_Context>     <C4/C4_Container>  <C4/C4_Component>
    !include <C4/C4_Dynamic>     <C4/C4_Deployment>
    ```

    **Never a URL, and never a bare filename**, so not `!include C4_Container.puml`. An
    include resolves where the *renderer* runs, not where the markdown lives. A renderer
    configured as a server (VS Code's `plantuml.render: PlantUMLServer`, the common setup)
    looks a bare filename up on the server's own filesystem, finds nothing, and answers HTTP
    400 for the whole diagram. The `<C4/…>` form ships inside PlantUML itself, so it resolves
    in a local jar and in a server alike, offline, with no library path to configure.

11. **Sequence, flow and activity diagrams are not part of C4.** Use a C4 dynamic diagram
    (`<C4/C4_Dynamic>`) when the interaction is between elements the document has already
    introduced. Otherwise write a plain PlantUML sequence or activity diagram coloured to
    match the C4 palette, so it sits alongside the C4 views: person and system `#08427B`,
    container `#438DD5`, component `#85BBF0`, external `#999999`, white text on the dark fills.

12. **Embed every diagram in the markdown** as a fenced ```plantuml block. No sibling `.puml`
    files, no rendered PNG or SVG. One idea per diagram. A diagram that needs a paragraph of
    caveats is really two diagrams.

13. **Use only macros the renderer actually has.** The C4 bundled with PlantUML lags
    C4-PlantUML's master branch, so a macro documented online may simply not exist locally,
    and a diagram that is valid against the docs then fails as a whole. `RelIndex` is one such
    macro. A C4 dynamic diagram numbers plain `Rel()` calls by itself and needs nothing else.
    Prefer the oldest macro that expresses the thing.

14. **Render every diagram before committing it.** A bad include or an unknown macro fails
    the entire diagram rather than degrading. A markdown preview shows an empty space rather
    than an error anyone can read, so an unrendered diagram is an unverified one. Render
    against the same renderer the reader's preview uses, because that is what decides which
    macros exist.

## Reviewing an existing architecture document

1. Read it asking on which day of the project it was written. If any sentence answers, that
   sentence is the finding: a status, a "done" marker, a date, a planned release, a milestone
   or a phase.
2. Cut every account of how the design came to be: "previously", "we changed to", a revision
   log, an attempt that was reverted. Keep a rejected alternative only where it sits inside
   the decision that rejected it, as the reason.
3. Cut every gotcha, failure and issue. Relocate each to the issue tracker or the runbook's
   troubleshooting section, and only if it exists nowhere else.
4. Check each sentence for past or future tense about the project's own work. `we will`,
   `we did`, `next` and `as of` are the finding.
5. For each flow, check that the reader is told what was wrong before being shown the
   solution, rather than left to work it out from two things placed side by side.
6. Cut anything the reader executes, and relocate it to the runbook. Cut the rationale for one
   block of code, and relocate it to a comment beside that block. Neither is cut until it
   exists at its destination.
7. Name what is missing: how the parts fit together, why the system is divided this way, and
   what the division rejected. An absent one of those is a finding as much as a surplus
   section is.
8. For each diagram, check that it carries one idea, that a two- or three-line summary sits
   under it, and that no other diagram or passage says the same thing.
9. Check every `!include` is of the `<C4/…>` form, every non-C4 diagram uses the C4 palette,
   and every diagram is a fenced `plantuml` block with no sibling file or rendered image.
10. Render every diagram against the reader's renderer. One that fails to render is the
    finding, whatever the markdown preview shows.
11. Settle anything the steps above do not cover against the opening sentence: is this the
    design as it stands, or is it the story of how it got there?

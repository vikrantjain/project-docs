---
name: readme
description: Guidelines for writing a README — what belongs in it, what belongs somewhere else, and how far it goes before the reader is sent elsewhere. Use before creating, updating or reviewing a README or a project's front-page documentation.
---

# READMEs

A README is **read, not followed**. Someone who has just landed on the repo skims it to answer
three questions: what is this, is it the thing I need, and where do I go next. Every rule below
serves that one sentence.

When a case here isn't covered, decide it from that sentence: does this help the reader decide
and route, or does it belong to whoever has already decided?

## The rules

1. **Open with what it is, in one or two sentences.** A plain description of the thing, in the
   words someone searching for it would use. Not a tagline, not a claim about how good it is.
   The reader must be able to reject the project from these sentences alone.

2. **Say who it is for and what problem it solves.** Where the project only makes sense inside
   a larger system, say which system and what part it plays. One paragraph.

3. **Orientation, not instruction.** The README routes the reader; it does not walk them
   through work. Anything the reader executes at length belongs in a runbook — see the
   `runbook` skill, rule 6. What stays here is what the thing is, how it fits, and where the
   docs are.

4. **A quickstart earns its place only while it stays short.** Install and one command that
   proves the thing runs, with its expected output. Once it grows past what someone reads to
   decide whether to care, move it to a runbook and leave a link.

5. **Requirements before the quickstart.** Language and runtime versions, the services it needs
   running, the platforms it is known to work on. A reader who cannot meet them should find that
   out before typing anything.

6. **Link the architecture, do not restate it.** One line pointing at the design document. A
   diagram belongs here only when the reader cannot place the project without it, and then it
   is the smallest one that does the job.

7. **No history and no changelog.** Git holds one and `CHANGELOG.md` holds the other. No "now
   rewritten in", no migration notes, no record of what a past version did.

8. **No status theatre.** No roadmap, no progress bars, no "coming soon" lists, no badge wall.
   A badge stays only when its state changes a reader's decision — build status, published
   version. Maturity is stated in a sentence if it is unusual: pre-release, unmaintained,
   internal-only.

9. **Every section is one the reader would go looking for.** A section added because other
   READMEs have it, and which says nothing specific to this project, is deleted rather than
   filled.

10. **Say where support goes.** Where to file a bug, where to ask a question, and who owns the
    project when that is not obvious from the repo.

11. **Contribution and licence are links, not chapters.** `CONTRIBUTING.md` and `LICENSE` carry
    the text; the README names them.

12. **Every link resolves and every command runs.** A broken link on the front page is the
    cheapest signal a reader has that the rest is stale.

## Reviewing an existing README

1. Read only the first screen, then say what the project is and whether you would use it. If
   you cannot, the opening is the finding.
2. Cut anything the reader executes beyond the quickstart, and anything that explains the
   design — relocate to the runbook or the architecture document, leaving a link. It is not
   relocated unless it exists nowhere else.
3. Delete every section that would read identically in another project.
4. Follow each link and run the quickstart on a fresh environment.

---
name: config-doc
description: Guidelines for writing and reviewing a configuration reference, the table an operator looks up to configure a system: its environment variables, configuration properties, command-line flags, mounted files, ports and their defaults. Use before creating, updating or reviewing such a table, a configuration or settings document, or the "Configuration" section of a README. Also use before writing or reviewing a code change, diff or pull request that adds, renames or removes a setting or changes a default, even when the task is about the code.
---

# Configuration references

A configuration reference is **looked up, not read through**. An operator arrives with one
setting's name or one symptom and needs one row. Every row is a promise about what the running
system does, so every row is taken from the system, not from memory. Every rule below serves that
one sentence.

When a case here isn't covered, decide it from that sentence: can an operator who arrives with
one name or one message find its row, and can they trust what the row says?

## The rules

1. **Every setting the system reads has a row, and every row is a setting it reads.** Build the
   list from the system: the code that reads the environment, the configuration files it loads,
   the entrypoint of its image and the flags of its command. A setting missing from the table
   leaves the operator who needs it with nothing to find. A row for a setting the system no
   longer reads has the operator change a value that does nothing. The ports the system listens
   on and the paths it answers health checks on get rows too, even where they cannot be changed.

2. **Generate the table from the system where the project can.** A schema, the framework's
   configuration metadata or the command's `--help` output can each produce it. A hand-written
   table drifts from the system with every change to the code. A generated table says how to
   regenerate it, and the rules below then apply to the source it is generated from. Where that
   source is a comment or a docstring, these rules govern it, not the `code-comments` skill's.
   Its reader is the operator, so it gives the message the system prints word for word, as
   rule 4 asks.

3. **Each row gives the name, what the setting does, its default and whether it must be set.**
   Write the name exactly as the operator types it. Say what happens when the setting is unset
   and when it is set to empty, because the two often differ. Give the valid values or the
   range, and the unit. Where the setting names a file, say who must be able to read it. Group
   the rows by what they configure, and sort them by name within a group.

4. **Say what happens when a setting is missing or wrong.** Give the message the system prints,
   word for word. An operator who arrives with a symptom searches for that message, and the
   search has to land on the row. This matters most where the message does not name the
   setting.

5. **Mark every setting that holds a secret, and every one that makes the system write a secret
   down.** A key, a password or a token is the first kind. A switch that turns on heap dumps, a
   debug log or a trace that captures one is the second kind. No row shows a usable example
   value for either.

6. **Separate the settings the system owns from those it only passes on.** A base image, a
   framework and a runtime each read settings of their own. Those go in a table of their own,
   which names whose they are. Their names and defaults follow that component's version, not
   the project's. Where the project promises not to rename its settings, say which table the
   promise covers.

7. **Say what else can set the same value, and which source wins.** A configuration file, a
   command-line flag, a profile, or a second name the framework derives can each set what the
   documented name sets. Where another source wins, an operator who sets the documented name
   sees no error and no effect. Where a setting guards a check, name any other setting that
   switches the check off.

8. **Test every general claim against the system before writing it.** "Any other value",
   "always", "cannot" and a list worded as complete each claim every case. One case the system
   does not keep makes the row false. A list that is not complete says so.

9. **State what it was last verified against.** Give the date, the version of the system, and
   the version of each component whose settings it lists. A row is a promise only about the
   version it was checked on.

10. **Keep the table where the project already keeps it, and propose a file of its own before
    creating one.** Where the project already has a document for its settings, that document is
    its configuration reference, whatever it is called. Where it has none, the table goes in the
    README's "Configuration" section. Once that section is longer than what someone
    reads to decide whether to care, propose moving it to a file of its own, and name the file.
    The file is proposed, never assumed. Until the project's owner agrees, the table stays in
    the README. See the `readme` skill, rule 3. A runbook never carries the table. Its
    prerequisites link the table wherever it lives. See the `runbook` skill, rule 5.

11. **Rows, not procedures and not design.** A sequence of steps the operator runs belongs in
    the runbook, and the row links it. Why the system has a setting, beyond the one sentence
    saying what it does, belongs in the architecture document. See the `architecture-doc`
    skill, rule 8.

## Reviewing an existing configuration reference

1. Pick three settings the system reads, and find each one's row by its name alone. A row you
   cannot find without reading the document through is the finding, and so is a name written
   differently from how the operator types it.
2. List what the system reads, from the system: the code that reads the environment, the
   configuration files, the image's entrypoint and the command's flags. Lay the list beside the
   table. A setting with no row is the finding, and so is a row for a setting nothing reads. A
   port or a health path with no row is the same finding.
3. Where the table is generated, regenerate it and compare. Where it is hand-written and the
   project has a schema, configuration metadata or `--help` output that could produce it, that
   is the finding. Where it is generated from comments in the source, review those comments
   with this checklist.
4. Check every row for what the setting does, its default, whether it must be set, its valid
   values and its unit. Check each default against the code or the entrypoint for both the
   unset case and the empty case. Where you can run the system, set the variable to empty and
   leave it unset, and compare both with the row. Check that a row naming a file says who must
   be able to read it. Check that the rows are grouped by what they configure, and sorted by
   name within each group.
5. For each setting, find the message the system prints when the setting is missing and when
   its value is invalid. Where you can start the system, start it each way and compare. A row
   that does not give each message word for word is the finding.
6. Check that every setting holding a secret is marked, and every setting that makes the system
   write one down. A usable example value for either is the finding.
7. Check that settings read by a base image, a framework or a runtime sit in a table of their
   own that names whose they are. Where the project promises stable names, check that the
   reference says which table the promise covers.
8. For each setting, look for another source that sets the same value: a configuration file, a
   flag, a profile, a derived name. One the reference does not name, or whose precedence it
   does not state, is the finding. For each setting that guards a check, look for a setting
   that switches the check off.
9. Find every general claim and every list worded as complete, and look for a case the system
   does not keep. One case is the finding.
10. Check that the reference states the date and the versions it was last verified against, and
    that those versions are the ones in use.
11. Check where the table lives. A full table inside a runbook is the finding. A table in the
    README longer than a skim is the finding too, and its fix is a proposal to move it, not the
    move. A configuration reference file whose owner did not ask for it is the finding. Check
    that the README and the runbook's prerequisites link the reference.
12. Cut every procedure and every explanation of the design. Relocate a procedure to the runbook
    and an explanation to the architecture document, and leave a link in the row. Neither is
    cut until it exists at its destination.
13. Settle anything the steps above do not cover against the opening sentence: can an operator
    who arrives with one name or one message find its row, and can they trust what the row
    says?

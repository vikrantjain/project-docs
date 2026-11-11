# PlantUML

Read this before drawing a diagram in a project whose tool is PlantUML. The rules in
`SKILL.md` still apply; this file holds only what is true of PlantUML and of nothing else.

## Structure is drawn with C4, included from the bundled standard library

Use exactly this include form:

```
!include <C4/C4_Context>     <C4/C4_Container>  <C4/C4_Component>
!include <C4/C4_Dynamic>     <C4/C4_Deployment>
```

**Never a URL, and never a bare filename**, so not `!include C4_Container.puml`. An include
resolves where the *renderer* runs, not where the markdown lives. A renderer configured as a
server (VS Code's `plantuml.render: PlantUMLServer`, the common setup) looks a bare filename up
on the server's own filesystem, finds nothing, and answers HTTP 400 for the whole diagram. The
`<C4/…>` form ships inside PlantUML itself, so it resolves in a local jar and in a server alike,
offline, with no library path to configure.

## Sequence, flow and activity diagrams are not part of C4

Use a C4 dynamic diagram (`<C4/C4_Dynamic>`) when the interaction is between elements the
document has already introduced. Otherwise write a plain PlantUML sequence or activity diagram
coloured to match the C4 palette, so it sits alongside the C4 views: person and system
`#08427B`, container `#438DD5`, component `#85BBF0`, external `#999999`, white text on the dark
fills.

## Rendering from a terminal

Extract the fenced block to a `.puml` file and run `plantuml -tsvg <file>`, or
`java -jar plantuml.jar -tsvg <file>` where only the jar is installed. A diagram that fails
prints the line and the error. This checks against the local jar, so where the reader's preview
is a server, the server's PlantUML version is the one that decides. Rule 13 still asks for a
look at that preview before committing.

## Constructs known to fail

The C4 bundled with PlantUML lags C4-PlantUML's master branch, so a macro documented online may
not exist locally.

- `RelIndex` does not exist in the bundled C4. A C4 dynamic diagram numbers plain `Rel()` calls
  by itself and needs nothing else.

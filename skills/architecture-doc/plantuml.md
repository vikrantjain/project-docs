# PlantUML

Read this before drawing a diagram in a project whose tool is PlantUML. The rules in
`SKILL.md` still apply; this file holds only what is true of PlantUML and of nothing else.

## Structure is drawn with C4, included from the bundled standard library

Include one library per `!include` line, in this form:

```
!include <C4/C4_Container>
```

The libraries are `<C4/C4_Context>`, `<C4/C4_Container>`, `<C4/C4_Component>`,
`<C4/C4_Dynamic>` and `<C4/C4_Deployment>`. Each one includes the libraries it builds on, so
`<C4/C4_Container>` alone also defines `Person` and `System`. A line naming several libraries
fails the whole diagram. PlantUML reads the rest of the line as one path and cannot include it.

**Never a URL, and never a bare filename**, so not `!include C4_Container.puml`. An include
resolves where the *renderer* runs, not where the markdown lives. A renderer configured as a
server (VS Code's `plantuml.render: PlantUMLServer`, the common setup) looks a bare filename up
on the server's own filesystem, finds nothing, and answers HTTP 400 for the whole diagram. The
`<C4/…>` form ships inside PlantUML itself, so it resolves in a local jar and in a server alike,
offline, with no library path to configure.

## Sequence, flow and activity diagrams are not part of C4

Use a C4 dynamic diagram (`<C4/C4_Dynamic>`) when the interaction is between elements the
document has already introduced. Otherwise write a plain PlantUML sequence or activity diagram
coloured to match the C4 palette, so it sits alongside the C4 views. The fills are:

- person `#08427B`
- system `#1168BD`
- container `#438DD5`
- component `#85BBF0`
- external system `#999999`
- external person `#686868`

Text is white on every fill except the component's, where it is black.

## Rendering from a terminal

Render against the renderer the reader's preview uses. Where that preview is a PlantUML server,
the editor's settings name it. In VS Code it is `plantuml.server`, in the user or the workspace
`settings.json`. Extract the fenced block to a `.puml` file, set `PLANTUML_SERVER` to that URL,
and send the file to the server's `/svg` endpoint:

```
curl -s -o diagram.svg -w '%{http_code}\n' -H 'Content-Type: text/plain' --data-binary @diagram.puml "$PLANTUML_SERVER/svg"
```

`200` means the diagram rendered. A diagram that fails answers `400`, and the SVG it returns
shows the error and the line it is on. The SVG names the server's PlantUML version in a
`<?plantuml …?>` instruction.

Where the preview uses no server, run `plantuml -tsvg diagram.puml`, or
`java -jar plantuml.jar -tsvg diagram.puml` where only the jar is installed. A diagram that
fails prints the line and the error. This checks against the local jar's version only, and
the preview's may differ. Rule 13 still asks for a look at that preview before committing.

## Constructs known to fail

The C4 bundled with PlantUML lags C4-PlantUML's master branch, so a macro documented online may
not exist in the reader's version. Each entry names the versions it was checked on.

- `RelIndex` is missing from the C4 bundled with older PlantUML releases, and renders on
  PlantUML 1.2026.6. A C4 dynamic diagram numbers plain `Rel()` calls by itself, so `Rel()`
  works on every version and `RelIndex` is never needed.

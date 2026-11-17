# Mermaid

Read this before drawing a diagram in a project whose tool is Mermaid. The rules in `SKILL.md`
still apply; this file holds only what is true of Mermaid and of nothing else.

## Where it renders

GitHub renders a fenced `mermaid` block in markdown without any setup, which is the usual reason
a project chooses it. A local preview needs an extension, and the Mermaid version it bundles
differs from the one GitHub runs. Rule 13 of the skill applies: render where the reader will
read, which for a GitHub-hosted document means pushing to a branch and looking at it.

## Rendering from a terminal

Extract the fenced block to a `.mmd` file and run
`npx -y @mermaid-js/mermaid-cli -i <file> -o <file>.svg`. A diagram that fails prints the parse
error. This checks against the CLI's bundled Mermaid, not GitHub's, so a diagram that passes here
can still fail on GitHub where the versions differ. Rule 13 still asks for a look at the pushed
page.

## Which diagram carries which view

Mermaid has no C4 that renders everywhere, so each architecture view maps to one of its stable
diagram types. Use the type in the table and no other for that view.

| View | Diagram | How |
|---|---|---|
| System context: the system, its users and the external systems it talks to | `flowchart` | One node per system or actor, the system under design as a `subgraph`, labelled edges for each dependency |
| Containers: the deployable units inside the system | `flowchart` | One `subgraph` for the system boundary, one node per container, edges labelled with the protocol |
| Components: the parts inside one container | `flowchart` | One `subgraph` for the container, one node per component. Draw only the container the document is about |
| Deployment: what runs where | `flowchart` | Nested `subgraph` blocks for environment, host and runtime, containers as the leaves |
| One interaction between parts already introduced | `sequenceDiagram` | Participants named exactly as the flowchart nodes that introduced them |
| Lifecycle of one thing with distinct states | `stateDiagram-v2` | Only where the states are the design, not an implementation detail |
| Data a system owns, at the level of entities and relations | `erDiagram` | Entities and relations only, no column lists |

Direction is `LR` for context and container views, `TB` for deployment. Keep one direction per
diagram.

## Carrying the palette

Where the project's other documents use the C4 colours, apply them with one `classDef` per
element kind at the top of the diagram and assign nodes with `:::kind`, so one definition
carries the whole diagram and a per-node `style` line never appears:

```
classDef person fill:#08427B,color:#fff
classDef system fill:#1168BD,color:#fff
classDef container fill:#438DD5,color:#fff
classDef component fill:#85BBF0,color:#000
classDef external fill:#999999,color:#fff
```

## Constructs not used

- `C4Context`, `C4Container`, `C4Component`, `C4Dynamic` and `C4Deployment`: marked
  experimental in Mermaid's own documentation and absent from some renderers. The table above
  gives the stable type for each.
- `architecture-beta` and `block-beta`: beta, and absent from older renderers for the same
  reason.

## Constructs known to fail

- A node id of `end` in lowercase breaks a `flowchart`, because it is the keyword that closes a
  `subgraph`. Capitalise it or choose another id.
- A node label containing `(`, `[`, `{` or `"` must be quoted: `A["read(path)"]`. Unquoted, the
  bracket is parsed as a shape and the diagram fails.

Add a construct here the first time it fails on the reader's renderer, with the renderer it
failed on.

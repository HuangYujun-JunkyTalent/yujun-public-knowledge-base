# TikZ Editor opens a figure only when its styles are in the file

TikZ Editor, the desktop application and the web application at https://tikz.dev/editor/web, renders TikZ with its own interpreter and resolves no `\usepackage`, `\input` or `\include`.
The web application has no file access, and the desktop application opens and saves only the one file that is picked, as its source in `apps/desktop/src/platform/desktop-platform.ts` shows.
A figure that takes its styles from a package therefore shows every style as an unsupported option, every text macro as undefined, and every element in black.

The remedy is a style block at the top of every figure, between the marker lines `%% >>> <name> styles` and `%% <<< <name> styles`, such as `mynotation` for a figure in a notation of its own.
The block holds the colour definitions, the text macros as `\providecommand`, and the `\tikzset` of the package.
LaTeX is unaffected when the paper also loads the package, because `\providecommand` and an identical `\tikzset` change nothing, and `\usetikzlibrary` stays in the preamble where it belongs.
The script `apply-styles.py` of the `tikz-figures` skill of the ai-workspace writes the block from the file that defines the styles, such as `mynotation.sty`, into a figure or into the template of a notation.
The block belongs to the figure and may differ from a later version of that file, because the figure needs only the styles it was drawn with.
The script names the block after that file unless `--name <block name>` names it otherwise, so a beamer slide whose styles sit in a theme file carries the block the same way.
A slide that carries the block still compiles inside its deck, because the deck loads the theme first and a repeated `\providecommand` and an identical `\tikzset` change nothing.

The round trip was verified on 2026-09-14 with version 0.5.2.
Two figures of 108 and 120 objects opened on their own with all styles resolved and no warning.
Dragging a box rewrote only its `at (x,y)`, the wires followed through their calc ports, and saving kept the style block and the comments intact.
After a save, rendering the figure again gives the new PDF and SVG.

To edit every picture of a whole paper, open the `.tex` in the editor after inlining the package and the `\input` files into one file, which `inline-tex.py` of the `tikz-figures` skill writes, or put the `\tikzset` block in the paper's preamble.

Known gaps of version 0.5.2 are partial support of `decorations`, `graphs` and `plots`, no `let` operations, and no interface for relative positioning, which is why figures place nodes at absolute coordinates.

The editor rejects a perpendicular coordinate whose operand is a bare pair.
LaTeX places `(ctl.east |- 0,2.0)` at the x of the anchor and the y of the pair, and the editor reports `Coordinate evaluation issue: invalid-cartesian-coordinate:(ctl.east |- 0,2.0)` and drops the wire.
The parser of the editor splits a coordinate at a top-level comma before it looks for `|-` or `-|`, in `packages/core/src/domains/coordinates/parse.ts` of the source at version 0.6.0, so the coordinate becomes a cartesian pair whose first component `ctl.east |- 0` is not a length.
The perpendicular evaluator in `packages/core/src/semantic/coords/evaluate.ts` runs only on a coordinate without a top-level comma.
The core package of version 0.6.0, run on 2026-10-05, accepts four ways of writing the same port, and LaTeX accepts all four.

| Form | Example | What follows the box when the box is dragged |
|---|---|---|
| calc fraction along an edge | `($(ctl.north east)!0.2222!(ctl.south east)$)` | the port, and it keeps its place on the edge when the box is resized |
| anchor with a shift | `([yshift=5mm]ctl.east)` | the port |
| perpendicular with a named coordinate | `(ctl.east |- p)` after `\coordinate (p) at (0,2.0);` | the x of the port only, the y stays at the named coordinate |
| perpendicular with the pair in braces | `(ctl.east |- {0,2.0})` | the x of the port only, the y stays at the pair |

The parser of the editor also reads the name `edge` inside a coordinate as the path operation `edge`, so a coordinate named `edge` gives a parse error, while LaTeX accepts the name.
A coordinate for the border of a figure is named `rim` instead.

The figures of the ai-workspace use the calc fraction, as [the item on placement](box-and-wire-placement.md) says, and `convert-perpendicular-ports.py` of the `tikz-figures` skill rewrites the bare pairs of a figure into it.
The script `check-tikz-editor-diagnostics.mjs` of the `tikz-figures` skill of the ai-workspace runs the core package of a local clone of the editor on a figure and lists the diagnostics, so that a figure is checked without opening the editor.
Run on its own, the core package of version 0.6.0 applies a `\tikzset` inside the picture and not the style block before it, and the tool reports the style names it did not resolve on one line.
The application resolved the block before the picture in the round trip of 2026-09-14 with version 0.5.2.

---

Source: the section "GUI editing with TikZ Editor" of the conventions for TikZ figures of a private repository of presentations, read on 2026-09-21.
Related: [Place boxes at absolute coordinates and attach wires to calc ports](box-and-wire-placement.md).

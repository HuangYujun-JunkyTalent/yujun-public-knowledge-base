# Place boxes at absolute coordinates and attach wires to calc ports so that a GUI editor can move them

Place every box as a named node at absolute coordinates:

```latex
\node[draw, minimum width=2.2cm, minimum height=1.2cm] (task) at (2.2,3.36) {Task};
```

A GUI editor that drags the box rewrites only the `at (x,y)` part, and the rest of the line survives.
Relative placement such as `right=of task` has no counterpart in the editor's interface, so a figure meant for GUI editing does not use it.

Attach a wire to a port that is a `calc` expression on the border of the box:

```latex
\draw[->] (-1,1.2) -- ($(batt.north west)!0.2!(batt.south west)$);
```

The port is 20 percent of the way down the left edge, so the wire follows the box when the box moves.
A perpendicular coordinate with a bare pair, such as `(batt.west |- 0,0.3)`, places the same port in LaTeX, but [TikZ Editor reads it as a cartesian coordinate and rejects it](tikz-editor-styles.md), so a figure meant for the editor uses the calc form.
The script `convert-perpendicular-ports.py` of the `tikz-figures` skill of the ai-workspace rewrites one form into the other.

A wire stays horizontal and vertical when a box is moved or resized if every point on it is derived from named anchors, so that no bend is a pair of numbers.
Three forms do that, and TikZ Editor accepts all three because none of them has a top-level comma.

- A port defined by the other end of the wire: `(a.east |- n)` is the point on the east edge of `a` at the height of `n`, so `\draw (a.east |- n) -- (n);` is horizontal whatever the heights of `a` and `n`, and `\draw (a.south -| m.north) -- (m.north);` is the vertical counterpart.
- An L drawn by a path operator: `\draw (b.east) -| (c.north);` goes horizontal from `b` and then vertical into `c`, and `|-` goes vertical first.
- A lane named as a coordinate: `\coordinate (lane) at (3.6,0);` and then `\draw (P) -| (lane |- m) -- (m.west);` runs from the port `P` to the lane, down to the height of `m`, and into `m`, with the lane as the one point a person drags to move the whole bend.

A calc fraction gives a port a place on the edge that scales with the box, and the forms above give a port the place the other end of the wire asks for.
Use the fraction when the port must keep its order among the ports of one edge, and the perpendicular form when the wire must stay straight into its target.
The five wires below use the three forms, and the evaluator of the editor and LaTeX both accept them, checked on 2026-10-05.

```latex
\coordinate (lane) at (3.6,0);
\draw[dashed] (a.east |- n) -- (n);
\draw[->] (n) -- (b.west |- n);
\draw[dashed] ($(a.north east)!0.8!(a.south east)$) -| (lane |- m) -- (m.west);
\draw[dashed] (b.east) -| (c.north);
\draw[->] (a.south -| m.north) -- (m.north);
```

Emit a figure as `name.tikz`, the `tikzpicture` alone, and `name.tex`, its standalone wrapper.
A paper writes `\input{name.tikz}` after loading the packages of its styles, and slides use the PDF or the SVG.

---

Source: the conventions for TikZ figures of a private repository of presentations, read on 2026-09-21.
Related: [TikZ Editor opens a figure only when its styles are in the file](tikz-editor-styles.md).

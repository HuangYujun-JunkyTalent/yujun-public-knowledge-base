# Add an example, a figure or a table where the prose alone does not let the reader see the subject

The prose alone is not enough when the reader would have to imagine an instance, a picture or a comparison to follow the text.
The table gives what the text gets in each case.

| The prose states | Add |
|---|---|
| a definition or a rule in general terms | one instance with real names and values |
| parts and the connections between them | a diagram |
| where things sit relative to each other | a figure |
| options compared on several properties | a table, with one row per option and one column per property |
| a quantity that changes with another | a plot |
| a command | a code block with the command and its output |
| a process or a dependence that the reader would explore by changing its inputs | an interactive component, such as a slider, in an HTML page |

> Rejected: *A tip touches the end of its path, so a wire that ends on the border of a box gets a dot outside the box, tangent to its edge.*
> Replacement: the same sentence, followed by the figure below.

![A box with two wires that end in a dot touching its left edge from outside, and two dashed wires that start with a dot touching its right edge from outside.](examples-and-figures/wire-dots.svg)

The figure is rendered from `examples-and-figures/wire-dots.tikz` by `render-tikz.sh` of the `tikz-figures` skill of the ai-workspace, and it needs only the `tikz` package.

An example shows a statement that the prose makes in full.
A reader who skips every example still has every rule and every fact of the text.
A statement that already names concrete things, such as a version number, a path or a setting with its value, gets no example.
An example that restates its sentence in other words makes the text longer and adds no information.

> Rejected: *The option `--write-tree` exists in git 2.38 and later, and an older git has no dry-run merge. For example, git 2.37 cannot run `git merge-tree --write-tree`.*
> Replacement: the second sentence is deleted, because the first already names the version.

The form has to show where the text is read.
A reply read in a terminal shows tables and code blocks and no images.
A figure in such a reply is a text diagram inside a code block, or a file rendered beside the reply and named by its path.
A Markdown file shows an SVG image.
GitHub and the VS Code preview also draw a `mermaid` code block as a diagram, as knowledge items `public-knowledge-base/markup-formats/github.md` and `public-knowledge-base/markup-formats/vscode-preview.md` say.
A LaTeX document draws a figure with TikZ.
The same TikZ source renders to an SVG for a Markdown file, as knowledge item `public-knowledge-base/tikz/rendering-to-pdf-and-svg.md` says.
An HTML page shows everything that a Markdown file shows, and also interactive components, animations, and links to any page or file.
A PDF keeps its layout fixed on every screen and on paper, and shows figures, tables and links.
A browser opens both, and a person reads them without a tool of the ai-workspace.

---

Source: Yujun Huang's request of 2026-10-03, Yujun Huang's decision of 2026-10-04 to accept the plan that answered it, Yujun Huang's request of 2026-10-10 that an example carry no essential information, and Yujun Huang's request of 2026-10-22 on rich formats for content that agents rarely read.

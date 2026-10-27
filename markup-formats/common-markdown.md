# GitHub, the VS Code preview and pandoc render dollar mathematics, pipe tables, strikethrough and links to headings alike

The forms below render alike in four Markdown implementations.
The four are GitHub in the `.md` files of a repository, the Markdown preview of VS Code 1.133.0 with its default settings, and pandoc 3.5 with the flavour `markdown` and with the flavour `gfm`.
The constructs of the [CommonMark core](commonmark-markdown-core.md) render alike as well, apart from the differences that the items on [GitHub](github.md), [the VS Code preview](vscode-preview.md) and [pandoc](pandoc.md) list.
The table gives, for each form, the way of writing it that all four render.
In the column Source, "Docs" links the GitHub Docs page that states the form, and "measured" marks a form whose rendering was measured in the VS Code preview engine and in pandoc with both flavours.

| Form | Way of writing it that all four render | Source |
|---|---|---|
| inline mathematics | `$x^2$`. The opening `$` follows a space, the start of a line or a punctuation character other than `$`, `_` and `\`, and a character other than a space follows it. The closing `$` follows a character other than a space, and a space, the end of a line or a punctuation character other than `$` and `_` follows it. So `($x^2$)` and `$x$:` render as mathematics, and `a$x$b` and `costs $5 and $6` do not meet the rule. The expression holds no characters that overlap with Markdown syntax, such as `*`, `_` or a backslash before punctuation, because GitHub Docs give another form for such an expression. | [Docs][math], measured |
| display mathematics | `$$` before and after the expression, either on one line as `$$x^2$$` or with each `$$` on a line of its own. A blank line comes before the first `$$` and after the last, and no blank line comes inside the expression. | [Docs][math], measured |
| dollar sign beside mathematics | `<span>$</span>` outside mathematics on a line that holds mathematics, as in `To split <span>$</span>100 in half, we calculate $100/2$`. | [Docs][math], measured |
| table | A header row, a delimiter row and data rows, with a pipe between two cells, and a blank line before and after the table. The pipes at the two ends of a row may be left out, and the cells need not line up. Each cell of the delimiter row holds at least three `-`, and a `:` on the left, on the right or on both sides of the hyphens aligns the column left, right or centre. A cell holds inline content, such as a link, a code span, emphasis or strikethrough. A pipe in the text of a cell, outside a code span, is written with a backslash before it. Without the blank line after the table, GitHub and the VS Code preview take a line of text right after the last row as another row, and pandoc takes it as a paragraph. | [Docs][tables], [GFM, example 202][tableend], measured |
| strikethrough | `~~text~~`, which may hold emphasis. | [Docs][styling], measured |
| link to a heading | `[text](#anchor)`. The anchor is the heading text in lower case, with each space replaced by a hyphen, punctuation removed, markup removed and its text kept, and `-1`, `-2` and so on added to a repeated anchor. So `## Sample Section` has the anchor `sample-section`. The anchors agree for a heading of level one to five whose text starts with a letter and holds words of ASCII letters and digits separated by single spaces, emphasis, code spans, and the characters `,`, `:`, `?`, `!`, `'`, `"`, `(` and `)` next to a word. | [Docs][sectionlinks], measured |

[math]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions
[tables]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/organizing-information-with-tables
[tableend]: https://github.github.com/gfm/#example-202
[styling]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#styling-text
[sectionlinks]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#section-links

Source: the GitHub Docs pages linked above, read on 2026-09-30, for GitHub, and measurements on the same day for the VS Code preview and pandoc.
The VS Code preview was measured by running the markdown-it engine of its bundled extensions `markdown-language-features`, `markdown-math` and `mermaid-markdown-features` with node 23.11.0.
Pandoc 3.5 was measured by converting test files with `-f markdown` and `-f gfm` to HTML, with and without `--mathjax`, and to LaTeX.
The end of a table was read on 2026-10-10 in example 202 of the GFM specification for GitHub, and in the table rule of the markdown-it engine bundled with VS Code 1.140.0 for the VS Code preview.
The end of a table was measured on the same day in pandoc 3.5 with both flavours.

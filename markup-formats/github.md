# GitHub renders Markdown files as CommonMark with tables, task lists, mathematics, footnotes, alerts and diagrams added

GitHub renders the README and every other `.md` file of a repository as GitHub Flavored Markdown.
The [GFM specification][gfm-intro] defines GitHub Flavored Markdown as a strict superset of CommonMark.
The specification marks five extensions: tables, task list items, strikethrough, extended autolinks and the filtering of nine HTML tags.
The specification is version 0.29-gfm of 2019-04-06 and is based on CommonMark 0.29.
After the conversion to HTML, GitHub sanitises the HTML and passes it through further filters, as the README of [github/markup][markup] says.
The GFM specification does not describe mathematics, footnotes, alerts, diagrams, emoji codes or heading anchors, and GitHub Docs describes them.

## GitHub renders most CommonMark constructs as CommonMark does

In a `.md` file, GitHub renders the following constructs with the forms of [CommonMark](commonmark-markdown-core.md): emphasis, strong emphasis, monospace or inline code, section heading, bulleted list, numbered list, block quotation, verbatim or code block, hyperlink, image or figure, comment, line break, non-breaking space, dashes and typographic quotes, and the backslash escape.
The GFM specification holds the CommonMark sections on these constructs.
A line break may also be written `<br/>`, as [GitHub Docs, basic writing and formatting syntax][basic] says.

## GitHub changes raw HTML and relative links and highlights code blocks

| Construct | What GitHub does | Source |
|---|---|---|
| raw HTML | The nine tags `<title>`, `<textarea>`, `<style>`, `<xmp>`, `<iframe>`, `<noembed>`, `<noframes>`, `<script>` and `<plaintext>` are shown as text, because their leading `<` becomes `&lt;`. The sanitiser then removes `script` tags, inline styles and `class` and `id` attributes. | [GFM specification, disallowed raw HTML][tagfilter], [github/markup][markup] |
| hyperlink and image or figure | A relative destination is rewritten for the branch being viewed. A destination that starts with `/` is relative to the root of the repository. | [Docs, relative links][relative] |
| verbatim or code block | An info string that is a language keyword of Linguist selects syntax highlighting for that language. | [Docs, code blocks][codeblocks] |

## GitHub adds features beyond CommonMark

The features below render in `.md` files of a repository.
The third column gives the rules that the documentation states.

Text that a CommonMark renderer shows as typed may also turn into a feature of the table.
Examples are `~word~`, `www.example.org`, `$x$`, `[ ]` at the start of a list item, a row of pipes, `:+1:` and `[^1]`.

| Feature | Form | Rules stated | Source |
|---|---|---|---|
| table | a header row, a delimiter row of `---` cells and data rows, with a pipe between two cells | A blank line precedes the table. The table ends at a blank line or at the start of another block, so a line of text right after the last row becomes another row, with its missing cells empty. A colon beside the hyphens aligns the column left, right or centre. A pipe inside a cell, also inside a code span, is written with a backslash before it. Cells hold inline content. | [GFM specification, tables][tables], [GFM specification, example 202][tableend], [Docs, tables][tablesdocs] |
| task list | `- [ ]` and `- [x]` | An item text that starts with a parenthesis is written `\(`. | [GFM specification, task list items][tasklist], [Docs, task lists][tasklists] |
| strikethrough | `~~text~~` or `~text~` | The same number of tildes, one or two, stands on each side. | [GFM specification, strikethrough][strike] |
| extended autolinks | `www.` followed by a domain, `http://` or `https://` followed by a domain, an email address, `mailto:` and `xmpp:` | The link ends before a trailing `?`, `!`, `.`, `,`, `:`, `*`, `_` or `~`, and before an unmatched trailing `)`. | [GFM specification, autolinks][autolinks] |
| inline mathematics | `$x^2$`, or ``$`x^2`$`` when the expression holds characters of Markdown syntax | GitHub renders mathematics with MathJax. | [Docs, mathematics][math] |
| display mathematics | `$$` before and after the expression, starting on a new line, or a code block with the info string `math` | In a `.md` file, a line of text right before `$$` ends with a hard line break. | [Docs, mathematics][math] |
| dollar sign beside mathematics | `\$` inside a mathematical expression, as in ``$`\sqrt{\$4}`$``, and `<span>$</span>` outside mathematics on the same line | | [Docs, mathematics][math] |
| footnote | `[^1]` in the text and `[^1]: text` anywhere | The notes render at the bottom of the document. | [Docs, footnotes][footnotes] |
| alerts | `> [!NOTE]` followed by `>` lines, for the five types | An alert stands on its own, outside any other element. | [Docs, alerts][alerts] |
| diagrams in code blocks | a code block with the info string `mermaid`, `geojson`, `topojson` or `stl` | A `mermaid` block holding `info` shows the Mermaid version in use. | [Docs, diagrams][diagrams] |
| emoji codes | `:+1:`, `:shipit:` and the other codes of the emoji cheat sheet | | [Docs, emoji][emoji] |
| heading anchors | generated from the heading text | Letters become lower case, spaces become hyphens, other punctuation is removed, markup is removed and its text kept, and a repeated anchor gets `-1`, `-2` and so on. A file with two or more headings gets an outline menu in its header. | [Docs, section links][sectionlinks], [Docs, headings][headings] |
| cross-reference to a section | `[text](#anchor)`, or `<a name="anchor"></a>` for a place without a heading | The outline lists the headings and leaves out the custom anchors. | [Docs, section links][sectionlinks], [Docs, custom anchors][anchors] |
| collapsed sections | `<details>` with a `<summary>`, and `<details open>` to show it open | | [Docs, collapsed sections][details] |
| subscript, superscript and underline | `<sub>`, `<sup>` and `<ins>` | | [Docs, styling text][styling] |
| the picture element | `<picture>` | | [Docs, the picture element][picture] |

[gfm-intro]: https://github.github.com/gfm/#what-is-github-flavored-markdown-
[tables]: https://github.github.com/gfm/#tables-extension-
[tableend]: https://github.github.com/gfm/#example-202
[tasklist]: https://github.github.com/gfm/#task-list-items-extension-
[strike]: https://github.github.com/gfm/#strikethrough-extension-
[autolinks]: https://github.github.com/gfm/#autolinks-extension-
[tagfilter]: https://github.github.com/gfm/#disallowed-raw-html-extension-
[markup]: https://github.com/github/markup#readme
[basic]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
[headings]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#headings
[styling]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#styling-text
[sectionlinks]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#section-links
[relative]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#relative-links
[anchors]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#custom-anchors
[picture]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#the-picture-element
[emoji]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#using-emojis
[footnotes]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#footnotes
[alerts]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts
[tablesdocs]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/organizing-information-with-tables
[details]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/organizing-information-with-collapsed-sections
[codeblocks]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks
[diagrams]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams
[math]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions
[tasklists]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/about-tasklists

Source: the GFM specification and the GitHub Docs pages linked above, and the README of github/markup, read on 2026-09-30, and example 202 of the GFM specification, read on 2026-10-10.
Related: [CommonMark marks up emphasis, code, headings, thematic breaks, lists, quotations, links, images, line breaks and raw HTML](commonmark-markdown-core.md), [The VS Code Markdown preview renders CommonMark with tables, strikethrough, KaTeX mathematics, Mermaid diagrams and front matter](vscode-preview.md), [Pandoc Markdown is a dialect with a parser of its own that adds mathematics, raw LaTeX and citations and writes each construct in its LaTeX form](pandoc.md), [GitHub, the VS Code preview and pandoc render dollar mathematics, pipe tables, strikethrough and links to headings alike](common-markdown.md).

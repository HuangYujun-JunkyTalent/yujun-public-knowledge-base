# The VS Code Markdown preview renders CommonMark with tables, strikethrough, KaTeX mathematics, Mermaid diagrams and front matter

The built-in Markdown preview of VS Code 1.133.0 renders a `.md` file with markdown-it 14, which follows CommonMark.
Three bundled extensions, each at version 10.0.0, decide what the preview renders beyond CommonMark.

- `markdown-language-features` runs markdown-it with raw HTML allowed, with the tables and the strikethrough of the default preset of markdown-it, and adds bare links, front matter and heading identifiers.
- `markdown-math` adds mathematics, typeset by KaTeX 0.16.21.
- `mermaid-markdown-features` adds diagrams, which Mermaid 11.15.0 draws inside the preview.

The lists below hold for the default settings.
A setting that changes a construct is named beside it.

## The preview renders these CommonMark constructs as CommonMark does

- Emphasis, strong emphasis, monospace or inline code, section heading, thematic break, bulleted list, numbered list, block quotation, comment, non-breaking space and the backslash escape.
- Verbatim or code block.
  A fenced block with a language name is coloured by highlight.js, and the text stays as typed.
- Line break.
  The setting `markdown.preview.breaks` turns every newline inside a paragraph into a line break.
- Dashes and typographic quotes, which stay as typed.
  The setting `markdown.preview.typographer` turns `--` into an en dash, `---` into an em dash, straight quotes into curly quotes and `(c)` into ©.

## The preview renders these CommonMark constructs in its own way

- Hyperlink, from `markdown-language-features`.
  A destination with the scheme `javascript:`, `vbscript:` or `file:`, or with `data:` not followed by `image/` and a `;`, renders as the source text of the link.
  A `vscode:` link opens VS Code, and a link to another Markdown file opens that file in the preview.
  The setting `markdown.preview.openMarkdownLinks` set to `inEditor` opens the file in the editor.
- Image or figure, from `markdown-language-features`.
  The default security level, Strict, loads images over `https:`, as `data:` and from local files inside the workspace folders, or inside the folder of the Markdown file when the file lies outside every workspace folder, and blocks images over `http:`.
  A path that starts with `/` is taken from the root of the workspace folder.
- Raw HTML, from `markdown-language-features`.
  Every tag reaches the preview unchanged, including `<script>`, `<style>` and `style` attributes.
  Strict runs only the scripts of the preview and its extensions, so a script in the file stays inert, and a `<style>` element applies.
  The level "Allow insecure content" also loads content over `http:`, and the level "Disable" sets no content security policy, which the documentation describes as allowing script execution.
- A `---` block on the first line, from `markdown-language-features`.
  The preview reads the block as YAML front matter and shows its keys as a table, where CommonMark reads a thematic break and a heading.
  The setting `markdown.preview.frontMatter` takes `table`, `codeBlock` or `hide`.

## The preview adds these constructs to CommonMark

- Inline mathematics, from `markdown-math`, written `$x^2$` or ``$`x^2`$``.
  The opening `$` must not follow an ASCII letter, a digit, an underscore, a `$` or a backslash, and the closing `$` must not precede an ASCII letter, a digit, an underscore or a `$`.
  `a$b$` and `costs $5 and $6` therefore render as text.
  A formula that holds `\begin{equation}`, `align`, `gather`, `alignat` or `CD` is set in display style.
  `\$` renders as a dollar sign.
  The setting `markdown.math.enabled` turns all mathematics off.
- Display mathematics, from `markdown-math`, written `$$...$$` on one line or across several lines, or as a fenced code block with the language `math`.
  `$$x$$` inside a paragraph renders as a display block that splits the paragraph.
- Numbered equation, from `markdown-math`.
  The environments `equation`, `align`, `gather` and `alignat` are numbered `(1)`, `(2)` and so on, and `\tag{3}` sets a number by hand.
  The numbers come from a CSS counter of KaTeX that runs through the whole preview.
- Macro definition, from `markdown-math`.
  `\newcommand` or `\def` in one formula defines the macro for every later formula of the same file.
  The setting `markdown.math.macros` defines macros for every file.
- Table, from `markdown-language-features`, in the pipe syntax of GitHub, with colons for the alignment of a column and `\|` for a pipe inside a cell.
- Strikethrough, from `markdown-language-features`, written `~~text~~`.
- Bare links, from `markdown-language-features`.
  A URL that starts with a scheme such as `https://` and an e-mail address become links.
  The setting `markdown.preview.linkify` turns bare links off.
- Cross-reference to a section, from `markdown-language-features`, written `[text](#section-one)` or `[text](other.md#section-one)`.
  Every heading gets an identifier in lowercase, with punctuation removed and spaces replaced by hyphens.
  A repeated heading gets `-1`, `-2` and so on, and mathematics in a heading is left out of its identifier.
- Diagrams, from `mermaid-markdown-features`, written as a fenced code block with the language `mermaid` or as a `::: mermaid` container.
  The setting `markdown-mermaid.languages` names further languages that are drawn as Mermaid.

## The preview renders these GitHub features

- Tables and strikethrough with two tildes.
- Mathematics in the four forms of GitHub: `$...$`, ``$`...`$``, `$$...$$` and the `math` code block.
- Mermaid diagrams in a `mermaid` code block.
- Heading anchors, with identifiers formed by the rule that [GitHub](github.md) gives and `-1` for a repeated heading.
- Bare URLs with a scheme and bare e-mail addresses become links, as GitHub's extended autolinks do, and a trailing `_`, `~` or `:` stays part of the link.

Source: [VS Code documentation, Markdown and Visual Studio Code](https://code.visualstudio.com/docs/languages/markdown), read 2026-09-30, for CommonMark as the target, KaTeX, Mermaid and the security levels. The versions, the settings and the rules were read in the bundled extensions `markdown-language-features`, `markdown-math` and `mermaid-markdown-features` of VS Code 1.133.0 on 2026-09-30. Every rendering above other than the drawing of diagrams, the security levels, the opening of links and the resolution of a path that starts with `/` was measured on the same day by running the markdown-it engine of these extensions with node 23.11.0, and the CommonMark reading of the `---` block with pandoc 3.5.
Related: [CommonMark marks up emphasis, code, headings, thematic breaks, lists, quotations, links, images, line breaks and raw HTML](commonmark-markdown-core.md), [GitHub renders Markdown files as CommonMark with tables, task lists, mathematics, footnotes, alerts and diagrams added](github.md), [Pandoc Markdown is a dialect with a parser of its own that adds mathematics, raw LaTeX and citations and writes each construct in its LaTeX form](pandoc.md), [GitHub, the VS Code preview and pandoc render dollar mathematics, pipe tables, strikethrough and links to headings alike](common-markdown.md).

# Pandoc Markdown is a dialect with a parser of its own that adds mathematics, raw LaTeX and citations and writes each construct in its LaTeX form

Pandoc reads Markdown in several flavours, and the option `-f` chooses the flavour.
The flavour `markdown` is pandoc's own Markdown, and pandoc reads a `.md` file in this flavour when no `-f` is given.
The flavour `commonmark` is CommonMark, and the flavour `gfm` is CommonMark with the extensions of GitHub.
Each flavour is a set of extensions, which `+name` switches on and `-name` switches off after the name of the flavour, as in `-f markdown+tex_math_single_backslash`.
The command `pandoc --list-extensions=<flavour>` lists the extensions that a flavour supports, and marks with `+` the extensions that are on by default.
The flavour `markdown` has a parser of its own, so it differs from CommonMark in some constructs that both have.
The two lists below compare the flavour `markdown` with CommonMark, and the table gives the LaTeX that pandoc writes for each construct.

## CommonMark constructs that pandoc Markdown renders as CommonMark does

- Emphasis and strong emphasis with `*` or `_`, and monospace or inline code between backticks.
- Fenced and indented code blocks, block quotations, and numbered lists with `1.` or `1)`.
- Inline links, reference links, autolinks between `<` and `>`, and images.
- ATX headings with a space after the `#` characters, and setext headings.
- A line break made by two trailing spaces or by a backslash at the end of the line.
- Entities such as `&nbsp;`, and inline raw HTML.

## CommonMark constructs that pandoc Markdown renders otherwise

- A heading, a block quotation, a list and a thematic break each need a blank line before them.
  A line `# Heading`, `> quote`, `- item` or `***` directly after the text of a paragraph joins the paragraph, where CommonMark starts a new block.
- A change of the bullet character between `-`, `+` and `*` continues the list, so `- a` and `+ b` on consecutive lines give one list of two items.
- A backslash escapes every punctuation character, every symbol such as `€` and the space, and a backslash before a space gives a non-breaking space.
- A backslash before a letter starts a raw LaTeX command, such as `\a` or `\foo{bar}`.
  The LaTeX output keeps the command unchanged, and the HTML output drops the command.
- Markdown between HTML block tags is parsed, so a line `*a*` between `<div>` and `</div>` gives emphasis inside the `div`.
- The source `**foo*` stays text in pandoc Markdown, and gives an asterisk followed by emphasis in CommonMark.
- Every heading gets an identifier made from its text, which becomes the `id` in HTML and, for the levels one to five, the `\label` in LaTeX.
  Pandoc Markdown also parses headings deeper than six levels, as [Markdown headings stop at six levels](markdown-heading-depth.md) says.
- An image with a non-empty alternative text alone in a paragraph becomes a figure, with the alternative text as its caption.
- Straight quotes, `--`, `---` and `...` become typographic quotes, an en dash, an em dash and an ellipsis.

## The LaTeX that pandoc writes for each construct of pandoc Markdown

The table gives, for each construct, the form in the flavour `markdown` and the LaTeX that `pandoc -t latex` writes.

| Construct | Form in `markdown` | LaTeX that pandoc writes |
|---|---|---|
| emphasis | `*a*` or `_a_` | `\emph{a}` |
| strong emphasis | `**a**` or `__a__` | `\textbf{a}` |
| monospace or inline code | `` `a` `` | `\texttt{a}` |
| inline mathematics | `$a$` | `\(a\)` |
| display mathematics | `$$a$$` | `\[a\]` |
| numbered equation with label and reference | raw LaTeX `\begin{equation}\label{eq:a}` to `\end{equation}`, with `\eqref{eq:a}` | the raw LaTeX unchanged |
| section heading | `# A {#sec-a}` | `\section{A}\label{sec-a}`, then `\subsection` down to `\subparagraph` for levels two to five, and a paragraph without label for level six |
| bulleted list | `- a` | `itemize` with `\tightlist` |
| numbered list | `1. a` | `enumerate` with `\labelenumi` set to the style |
| description list | a line `Term` and a line `: Definition` | `description` with `\item[Term]` |
| block quotation | `> a` after a blank line | `quote` |
| verbatim or code block | a fenced block with a language, or a fenced or indented block without | `Shaded` and `Highlighting` with a language, and `verbatim` without |
| hyperlink | `[t](u)` or `<u>` | `\href{u}{t}` or `\url{u}` |
| image or figure | `![c](f.png){#fig:a width=50%}` alone in a paragraph | `figure` with `\includegraphics[width=0.5\linewidth,...]{f.png}`, `\caption{c}` and `\label{fig:a}` |
| table | a pipe, simple, multiline or grid table | `longtable` with the rules of `booktabs` |
| footnote | `a[^1]` with a line `[^1]: note`, or `a^[note]` | `\footnote{note}` |
| citation | `[@key, p. 3]`, `@key` and `[-@key]` | `\citep[p.~3]{key}`, `\citet{key}` and `\citeyearpar{key}` with `--natbib`. `\autocite[3]{key}`, `\textcite{key}` and `\autocite*{key}` with `--biblatex`. Formatted text and a bibliography with `--citeproc`. The source as typed without these options. |
| cross-reference to a section | `[A]` or `[t](#sec-a)`, and raw LaTeX `\ref{sec-a}` | `\hyperref[sec-a]{A}`, and the raw LaTeX unchanged |
| comment | `<!-- c -->`, or `% c` in a fenced block with the attribute `{=latex}` | the HTML comment dropped, and `% c` for the raw block |
| macro definition | `\newcommand{\R}{\mathbb{R}}` on a line of its own | the definition unchanged, and `\mathbb{R}` for every `\R` in mathematics and in raw LaTeX |
| line break | two trailing spaces, or `\` at the end of the line | `\\` |
| non-breaking space | `\ ` or `&nbsp;` | `~` |
| dashes and typographic quotes | `--`, `---`, `"a"`, `'a'` and `...` | `--`, `---`, ` ``a'' `, `` `a' `` and `\ldots{}` |
| strikethrough | `~~a~~` | `\st{a}` |

The flavour `markdown` adds constructs beyond those of the table as well.
A task list item `- [ ] a` or `- [x] a` becomes `\item[$\square$]` or `\item[$\boxtimes$]`.
The source `H~2~O` becomes `H\textsubscript{2}O`, and `x^2^` becomes `x\textsuperscript{2}`.
A span `[a]{.smallcaps}` becomes `\textsc{a}`.
A fenced div such as `::: note` writes only its content to LaTeX.

The standalone template of pandoc, used with `-s`, loads the packages that these forms need.
The measured document loaded `amsmath` and `amssymb` for mathematics, `longtable` and `booktabs` for the table, `graphicx` for the image, `soul` or `lua-ul` for `\st`, `fancyvrb` and `color` for `Shaded`, and `bookmark` and `xurl` for the links.
Without `-s`, pandoc writes the body only, and the preamble of the including document has to load these packages and define the commands of the template that the body uses, such as `\tightlist`, `\pandocbounded`, the environments `Shaded` and `Highlighting`, and `\NormalTok`.

The HTML output drops raw LaTeX.
With `--mathjax`, the HTML output keeps a raw `equation` environment and a raw `\eqref` as mathematics for MathJax.

## Constructs of the table that the flavours `gfm` and `commonmark` read

The flavour `commonmark` reads the forms of the table for emphasis, strong emphasis, monospace or inline code, bulleted list, numbered list, block quotation, verbatim or code block, hyperlink, line break and the entity `&nbsp;`, and pandoc writes the same LaTeX for them.
A heading `# A` becomes `\section{A}`, and an image becomes `\pandocbounded{\includegraphics[keepaspectratio]{f.png}}`.

The flavour `gfm` reads all these forms, and in addition inline and display mathematics, a pipe table, a footnote `[^1]`, strikethrough and a task list, and pandoc writes the same LaTeX for them as for `markdown`.
A heading `# A` becomes `\section{A}\label{a}`, with the label made by the GitHub rule, and a link `[t](#a)` to that label becomes `\hyperref[a]{t}`.
A bare URL `u` becomes `\url{u}`.
An emoji code `:smile:` becomes the Unicode character.
An alert `> [!NOTE]` becomes a `div` with the title Note in HTML, and a paragraph Note before the content in LaTeX.

With `+smart`, both flavours write dashes and typographic quotes as `markdown` does.

## Mathematics in the flavours `markdown`, `gfm` and `commonmark`

| Extension | Forms read as mathematics | Flavours that support the extension | Flavours with the extension on by default |
|---|---|---|---|
| `tex_math_dollars` | `$a$` and `$$a$$` | `markdown`, `gfm` and `commonmark` | `markdown` and `gfm` |
| `tex_math_gfm` | a code block with the language `math`, and ``$`a`$`` together with `tex_math_dollars` | `gfm` and `commonmark` | `gfm` |
| `tex_math_single_backslash` | `\(a\)` and `\[a\]` | `markdown` | off by default |
| `tex_math_double_backslash` | `\\(a\\)` and `\\[a\\]` | `markdown` | off by default |

The two extensions for the LaTeX delimiters are switched on with `-f markdown+tex_math_single_backslash` and `-f markdown+tex_math_double_backslash`.
The extension `tex_math_single_backslash` takes away the escape of `(` and `[`.
Without the extension, a backslash before `(` or `[` is an escape, so `markdown` and `gfm` render `$x^2$` as mathematics and `\(y^2\)` as the text `(y^2)`.
The flavour `commonmark` reads mathematics once `+tex_math_dollars` or `+tex_math_gfm` switches it on.
In `markdown`, ``$`a`$`` is inline mathematics that holds the two backticks.

A `$` opens inline mathematics only when a non-space character follows it.
A `$` closes inline mathematics only when a non-space character precedes it, and in `markdown` only when no digit follows it as well.
The source `$5 and $10` therefore stays text in every flavour that reads `$`.
The LaTeX output writes inline mathematics as `\(...\)` and display mathematics as `\[...\]`, whatever delimiters the source used.

In `markdown`, an environment such as `\begin{equation}` is raw LaTeX, and pandoc writes the environment unchanged to LaTeX with its `\label`.

The extension `latex_macros`, on by default in `markdown` and supported by `markdown` only of the three flavours, applies each `\newcommand` of the source to every piece of mathematics and raw LaTeX.
The HTML output of `$\R$` after the definition in the table is then the character ℝ.
The definition itself still passes to the LaTeX output.

Source: [Pandoc User's Guide](https://pandoc.org/MANUAL.html), sections Extensions, Pandoc's Markdown and Markdown variants, read 2026-09-30, for the rule of `$`, the lost escape of `(` and `[` under `tex_math_single_backslash`, and the passing of macro definitions under `latex_macros`.
Every other claim was measured with pandoc 3.5 on 2026-09-30, by converting test files from `markdown`, `gfm` and `commonmark` to `latex` and `html` with the default extensions that `pandoc --list-extensions` lists.
Related: [CommonMark marks up emphasis, code, headings, thematic breaks, lists, quotations, links, images, line breaks and raw HTML](commonmark-markdown-core.md), [GitHub renders Markdown files as CommonMark with tables, task lists, mathematics, footnotes, alerts and diagrams added](github.md), [The VS Code Markdown preview renders CommonMark with tables, strikethrough, KaTeX mathematics, Mermaid diagrams and front matter](vscode-preview.md), [GitHub, the VS Code preview and pandoc render dollar mathematics, pipe tables, strikethrough and links to headings alike](common-markdown.md).

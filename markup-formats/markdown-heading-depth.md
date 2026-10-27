# Markdown headings stop at six levels

CommonMark and GitHub-flavoured Markdown recognise one to six `#` characters as a heading.
A seventh `#` is not a heading, and the line renders as literal text.

The cap follows HTML, which defines `h1` to `h6` and nothing below.
Renderers differ on what they do past the cap.

- CommonMark and GitHub-flavoured Markdown: the line stays text.
- Pandoc's own Markdown flavour: the heading is parsed to any depth and written as `<p class="heading">` with its anchor kept.
- The VS Code outline pane: the heading is ignored, because the shipped Markdown extension matches `^#{1,6}`.

An outline deeper than six levels therefore needs another format, or headings replaced by nested lists, which nest without a limit and carry no anchor.

Source: [CommonMark specification, ATX headings](https://spec.commonmark.org/0.31.2/#atx-headings), read 2026-09-25. The regular expression was read in the installed `markdown-language-features` extension on 2026-09-25, and the renderer behaviour was measured with pandoc 3.5 on the same day.
Related: [reStructuredText section depth is unlimited](restructuredtext-section-depth.md), [Org-mode exports three heading levels by default](org-mode-heading-depth.md).

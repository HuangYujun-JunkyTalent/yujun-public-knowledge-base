# Org-mode exports three heading levels by default

An org-mode file nests headlines without a limit, one more star per level.
The exporter does not carry that depth across.
`org-export-headline-levels` defaults to three, and every headline below the third level becomes a list item instead of a heading.

The default is silent: the file keeps its structure, and only the exported document is flattened.
A converted ten-level file came back as three headings followed by nested ordered lists.
Adding `#+OPTIONS: H:10` to the file restored all ten as headings, numbered to ten levels with section numbering switched on.

For an editor other than Emacs the format loses most of its value.
The main VS Code extension registers no document symbol provider, so the outline pane stays empty, and live numbering through `org-num-mode` is an Emacs feature.

Source: [Org manual, export settings](https://orgmode.org/manual/Export-Settings.html), read 2026-09-25. The flattening and the fix were measured with pandoc 3.5 on the same day.
Related: [Markdown headings stop at six levels](markdown-heading-depth.md), [reStructuredText section depth is unlimited](restructuredtext-section-depth.md).

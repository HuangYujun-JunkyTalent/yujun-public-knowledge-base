# reStructuredText section depth is unlimited

A section in reStructuredText is a title with a line of punctuation under it, and the character chosen fixes the level.
The hierarchy is the order in which the characters first appear, so the depth is bounded only by the 32 punctuation characters, and doubled again by adding an overline.
Sixty-four levels parse without a warning.

Depth beyond six survives into HTML, because the renderer emits nested `section` elements and marks the heading with `aria-level` where no tag exists.
Nesting therefore gives indentation for free, and `.. sectnum::` numbers the sections to any depth.
The VS Code outline pane follows about fourteen levels with the `simple-rst` extension, which recognises fourteen adornment characters.

Two costs come with the format.
A title's underline must be at least as long as the title, so renaming a section means retyping its underline.
The inline syntax differs from Markdown, so links, code and emphasis are rewritten rather than carried over.
A mechanical conversion of Markdown bodies into a reStructuredText skeleton produced four indentation errors on the first attempt, all from list and continuation lines the format reads differently.

Source: [Docutils, reStructuredText sections](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#sections), read 2026-09-25. The depth, the numbering and the conversion errors were measured with docutils 0.18.1 and pandoc 3.5 on the same day.
Related: [Markdown headings stop at six levels](markdown-heading-depth.md).

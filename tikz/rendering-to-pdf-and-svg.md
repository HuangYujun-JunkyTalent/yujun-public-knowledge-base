# Render a TikZ picture to PDF and SVG through a standalone wrapper

Keep the picture alone in a `.tikz` file and wrap it for compilation:

```latex
\documentclass[border=3pt]{standalone}
\usepackage[notextcomp]{stix2}   % the text font of the document
\usepackage{tikz}                % and the libraries the picture needs
\begin{document}
\input{figure.tikz}
\end{document}
```

Compile the wrapper with `pdflatex` or `latexmk -pdf` for the PDF.
Do not pass the `tikz` class option to `standalone` when the file wraps the picture in anything, such as a `\scalebox`, because the option expects a bare `tikzpicture` and fails with "Improper \prevdepth".
The `border` option alone crops any content.

Two routes give the SVG.

- `pdftocairo -svg figure.pdf figure.svg` converts the PDF.
  The PDF route needs poppler only, and it turns text into paths, so the text is not searchable.
- `latex` on a wrapper with the class option `dvisvgm`, then `dvisvgm --font-format=woff -o figure.svg figure.dvi`, keeps text as text with the fonts embedded.
  The DVI route needs no Ghostscript, whereas `dvisvgm --pdf` on a PDF does.

On the machine of the test, the DVI route took 0.2 seconds per figure and the PDF route 0.7 seconds, measured on 2026-09-19.

The `tikz-figures` skill of the ai-workspace holds `render-tikz.sh`, which writes the wrapper, runs one of the two routes and puts the PDF and the SVG beside the source.

---

Source: the Makefile of a folder of figures in a private repository of presentations, and tests run on Yujun Huang's requests of 2026-09-19 and 2026-09-21.

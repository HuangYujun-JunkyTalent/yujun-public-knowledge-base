# arxiv_latex_cleaner strips the comments of `.tex` files and drops the files of externalized TikZ figures

`arxiv_latex_cleaner` of Google copies a LaTeX project into a folder `<project>_arXiv` beside it, for an upload to arXiv.
It runs without an installation through uv:

```sh
uvx --from arxiv-latex-cleaner==1.0.11 arxiv_latex_cleaner <project> --commands_to_delete todo
```

The cleaner removes the comments of the `.tex` and `.tikz` files and leaves the comments of a `.sty` file in place.
A `%` at the end of a line stays, and so does `\%`.
`--commands_to_delete <name>` removes each use of `\<name>` with its optional argument and its first braced argument, nested braces included.
A use of a command with three arguments therefore leaves its second and third argument as text.
A definition such as `\newcommand{\commentAuthor}[1]{...}` survives, because no brace follows the name.

The cleaner deletes every `.md5`, `.dpth`, `.dep`, `.svg`, `.sh` and `.aux` file, every `.bib` file unless `--keep_bib` is given, and every PDF or image whose path no `.tex` file names.
The PDFs of figures externalized with `\tikzexternalize` are among them, and so are their `.md5` and `.dpth` files, which an arXiv build needs to use the PDFs.
The option `--use_external_tikz <folder>` keeps such a figure only when `\tikzsetnextfilename{<name>}` precedes its picture, and then replaces the picture with `\includegraphics`.
A project that names its figures through the job name has to copy the dropped files back after the cleaner has run.

---

Source: the behaviour of version 1.0.11 on a test file and on a copy of a paper repository, and the lists of patterns in `run_arxiv_cleaner` of its source `arxiv_latex_cleaner.py`, read on 2026-10-15.
Related: [An externalized TikZ figure is found only under the job name of the build that made it](../tikz/externalized-figure-names.md), [The recorder file of a build lists every file that the build read](files-read-by-a-build.md).

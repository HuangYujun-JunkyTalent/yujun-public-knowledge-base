# The recorder file of a build lists every file that the build read

`latexmk -recorder` has TeX write a file `<job name>.fls` beside the PDF.
Each file that TeX opened for reading appears there on a line `INPUT <path>`, and each file it wrote on a line `OUTPUT <path>`.
The list holds what the build opened, whatever command in the source opened the file.
Examples are a file of `\input`, a figure of `\includegraphics`, a CSV table that pgfplots reads and the `.pdf`, `.md5` and `.dpth` files of an externalized TikZ picture.
A path that starts with `/` belongs to the TeX installation, and a relative path belongs to the project.

The relative `INPUT` paths of a full build are therefore the files that the document needs:

```sh
latexmk -pdf -recorder main.tex
grep '^INPUT ' main.fls | sed 's/^INPUT //' | grep -v '^/' | sed 's|^\./||' | sort -u
```

The list also holds the `.aux`, `.bbl` and `.out` files that the build wrote and read back.
A file that the build reads only on some runs, such as a picture under a switch that is off, is missing from the list of a run that skipped it.

---

Source: measured on 2026-10-14 on a copy of a paper repository, with TeX Live 2022 and latexmk, where the list held 112 of the 290 files of the repository.
Related: [An externalized TikZ figure is found only under the job name of the build that made it](../tikz/externalized-figure-names.md).

# An externalized TikZ figure is found only under the job name of the build that made it, so the files that Overleaf writes fail on arXiv

`\tikzexternalize[prefix=figures/external/]` names the file of each picture `<prefix><job name>-figure<N>`, with the files `.pdf`, `.md5` and `.dpth` beside each other.
Overleaf compiles with the job name `output`, so the files it writes are `figures/external/output-figure0.pdf` and so on.
arXiv compiles `main.tex` with the job name `main` and without shell escape.
The build then looks for `main-figure<N>.pdf`, cannot make the file, and stops at each picture with the error

```
! Package tikz Error: Sorry, the system call 'pdflatex -halt-on-error -interaction=batchmode -jobname "figures/external/main-figure11" ...' did NOT result in a usable output file
```

After each error the build typesets the picture inline.
A paper with twelve pgfplots figures drawn from CSV tables did not finish its first pass in 25 minutes.

Renaming the three files of each picture to the job name of the target build repairs the build:

```sh
cd figures/external
for f in output-figure*; do mv "$f" "main-${f#output-}"; done
```

`pdflatex -no-shell-escape main.tex` then reads the stored PDFs, and two passes give the same 18 pages as the build with shell escape.

## The `.md5` file holds a checksum of the code of the picture, and the `.dpth` file holds its depth

The `.md5` file holds one line `\def \tikzexternallastkey {<checksum>}%`, the MD5 sum of the code of the picture.
A build compares the sum with the code of the picture, and makes the figure again when the two differ.
The checksum covers the code and none of the files that the code reads, so a changed CSV table leaves the figure as it is.
The `.dpth` file holds the depth of the picture below the baseline, and is empty for a picture that sits on the baseline.

A build without shell escape needs the PDF of each picture, and did without the other two files in a test.
With all `.md5` files removed, two passes wrote them again with the same sums, ran no system call and gave the same pages.
With all `.dpth` files removed, the pages were the same as well, because every one of those files was empty.

## A table read outside the picture is read by every build

A `\pgfplotstableread` placed before `\begin{tikzpicture}`, such as at the top of a figure file, runs whether or not the picture is externalized.
The CSV file then stays needed in a build that takes the picture from its PDF, and pdflatex stops with `Could not read table file` when the file is missing.

---

Source: measured on 2026-10-14 on a copy of a paper repository, with TeX Live 2022 and pdfTeX 1.40.23.
Related: [Render a TikZ picture to PDF and SVG through a standalone wrapper](rendering-to-pdf-and-svg.md), [The recorder file of a build lists every file that the build read](../latex/files-read-by-a-build.md).

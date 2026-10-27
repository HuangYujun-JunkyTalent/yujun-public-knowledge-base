# pdfTeX writes the same PDF byte for byte when `SOURCE_DATE_EPOCH` and `FORCE_SOURCE_DATE=1` fix its dates

A PDF of pdfTeX holds the time of the build in `/CreationDate` and `/ModDate`, and a trailer identifier `/ID` that pdfTeX derives from the time and the name of the file.
Two builds of the same source therefore differ in those bytes.
The environment variable `SOURCE_DATE_EPOCH`, a time in seconds since 1970, takes the place of the clock for the dates and the identifier.
`FORCE_SOURCE_DATE=1` makes `\today` and `\time` take the same time as well.

```sh
export SOURCE_DATE_EPOCH=$(git log -1 --format=%ct) FORCE_SOURCE_DATE=1
pdflatex main.tex
```

The time of the last commit makes the PDF of one commit the same on every build.
Two builds in different folders with these variables gave identical PDFs of 682911 bytes, so a byte comparison with `cmp` shows whether two sources typeset the same.
The variables reach pdflatex through latexmk, which passes its environment on.

---

Source: measured on 2026-10-15 with TeX Live 2021 and pdfTeX 1.40.23, on a paper repository, where the build of the original source and the build of its copy without comments gave identical PDFs.
Related: [arxiv_latex_cleaner strips the comments of `.tex` files](arxiv-latex-cleaner.md), [The recorder file of a build lists every file that the build read](files-read-by-a-build.md).

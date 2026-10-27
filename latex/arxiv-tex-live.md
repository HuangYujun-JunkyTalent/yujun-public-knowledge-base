# arXiv compiles a submission with its own TeX Live, TeX Live 2025 by default

arXiv typesets the source of a submission itself, so the PDF on arXiv comes from the TeX Live of arXiv and not from the TeX Live of the author.
Since September 2025, arXiv offers TeX Live 2023 and TeX Live 2025, with TeX Live 2025 in its state of 2025-08-03 as the default.
arXiv intends to offer the two most recent annual releases from then on.

Overleaf compiles a project with the TeX Live of the day the project was created, and the menu of the project sets another release.
Overleaf added TeX Live 2026 on 2026-08-18.

A different release of TeX Live changes line breaks, spacing and float placement, because the LaTeX kernel, the packages and the hyphenation patterns change between releases.
A local build checks the source for an arXiv upload only as far as its TeX Live matches the one of arXiv.
The Docker image `texlive/texlive:TL2025-historic` holds TeX Live 2025, for `linux/amd64` only, and is 2.6 GB compressed.

---

Source: arXiv blog, "Attention Authors: TeX Live 2025 now available on arXiv", 2025-09-10, https://blog.arxiv.org/2025/09/10/tex-live-2025-on-arxiv, and Overleaf blog, "TeX Live 2026 is now available", https://www.overleaf.com/blog/tex-live-2026-is-now-available, both as summarized by a web search on 2026-10-15; the image read with `docker manifest inspect` on 2026-10-15.
Related: [An externalized TikZ figure is found only under the job name of the build that made it](../tikz/externalized-figure-names.md), [pdfTeX writes the same PDF byte for byte](reproducible-pdf.md).

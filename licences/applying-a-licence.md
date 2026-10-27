# A repository carries its licence in a licence file at its root, and a file under another licence names it in an SPDX header

A licence applies to a repository through the full text of the licence in a file `LICENSE` at the root.
GitHub reads that file and shows the licence on the page of the repository.
The licence names the copyright holder and the year, as in `Copyright (c) 2026 Firstname Lastname` at the top of an `MIT` licence.

A file may name its licence and its copyright holder in a header of two lines, in the form of the SPDX specification:

```python
# SPDX-FileCopyrightText: 2026 Firstname Lastname <name@example.com>
# SPDX-License-Identifier: MIT
```

An expression may join identifiers, as in `GPL-3.0-or-later OR Apache-2.0` for a file that the user may take under either licence.

The REUSE specification of the Free Software Foundation Europe gives every file of a repository its licence in a form that a tool checks.

- The full text of each licence used sits in `LICENSES/<SPDX identifier>.txt`, such as `LICENSES/CC-BY-4.0.txt`.
- A file that holds comments carries the SPDX header.
- A file that cannot hold a comment, such as an image, gets a file beside it with the suffix `.license`, such as `figure.svg.license`, that holds the header.
- A file `REUSE.toml` at the root may give the licence of every file that matches a pattern:

```toml
version = 1

[[annotations]]
path = "docs/**"
SPDX-FileCopyrightText = "2026 Firstname Lastname"
SPDX-License-Identifier = "CC-BY-4.0"
```

The command `reuse lint` of the tool `reuse` reports every file without a licence.

A repository that holds code and text gives each its own licence, because a licence of software and a Creative Commons licence suit different works, as knowledge item `public-knowledge-base/licences/creative-commons.md` says.
A common pair is `MIT` for the code and `CC-BY-4.0` for the documents and the figures.
The `LICENSE` file or the README then says which paths take which licence.

On GitHub, a contribution to a repository with a licence is licensed under that licence, unless a separate agreement with the contributor says otherwise.
The owner of a repository changes its licence alone only for the code that the owner holds the copyright of.

---

Source: the REUSE Specification version 3.3 (https://reuse.software/spec-3.3/) and section D.6 of the GitHub Terms of Service (https://docs.github.com/en/site-policy/github-terms/github-terms-of-service), read on 2026-10-15.
Related: knowledge item `public-knowledge-base/licences/software-licences.md`, knowledge item `public-knowledge-base/licences/no-licence.md`.

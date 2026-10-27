# Licences of software range from network copyleft through file copyleft to permissive licences and public domain dedications

A copyleft licence requires that a modified or combined work is shared under the same licence, with its source.
A permissive licence requires only that the copyright notice and the licence text travel with the code.
A public domain dedication, and a licence such as `0BSD` that drops even the notice, let the user do anything with the code.
The table lists the common licences from the most restrictive to the one that allows the most, by their SPDX identifiers.

| SPDX identifier | Kind | What a user who distributes the code must do | Express patent grant |
|---|---|---|---|
| `AGPL-3.0-or-later` | network copyleft | release the source of the whole program under the AGPL, also when users only reach it over a network | yes |
| `GPL-3.0-or-later`, `GPL-2.0-only` | strong copyleft | release the source of the whole program that includes the code under the GPL | version 3 yes, version 2 no |
| `LGPL-3.0-or-later` | library copyleft | release changes to the library under the LGPL, while a program that only links the library may keep any licence | yes |
| `MPL-2.0` | file copyleft | release changed files of the code under the MPL, while new files beside them may keep any licence | yes |
| `Apache-2.0` | permissive | keep the notice and the licence, state the files changed, and keep the `NOTICE` file | yes |
| `BSD-3-Clause`, `BSD-2-Clause`, `MIT` | permissive | keep the copyright notice and the licence text | no |
| `0BSD`, `Unlicense`, `CC0-1.0` | public domain dedication or licence without conditions | nothing | no, and CC0 states that it grants no patent right |

The suffix `-only` fixes the version of a GNU licence, and the suffix `-or-later` lets the user choose a later version as well.
The duties of the GPL arise when the program is distributed, so a program run and changed in private carries none.
The AGPL adds the same duties for a program that users reach over a network.

Every licence of the table disclaims warranty and liability.
None of them grants a right to the trademarks of the licensor.

A permissive licence suits code that a researcher wants others to reuse with the fewest conditions, such as the code of a paper.
`Apache-2.0` adds a patent grant to the terms of `MIT`, at the price of a longer text and the `NOTICE` file.
A copyleft licence suits code whose improvements the author wants back in the open.
Code under a copyleft licence cannot enter a project under a permissive licence, as knowledge item `public-knowledge-base/licences/licence-compatibility.md` says.

---

Source: the comparison of licences of choosealicense.com (https://choosealicense.com/appendix/), the list of licences of the Free Software Foundation (https://www.gnu.org/licenses/license-list.html) and the REUSE Specification version 3.3 on SPDX identifiers (https://reuse.software/spec-3.3/), read on 2026-10-15.
Related: knowledge item `public-knowledge-base/licences/no-licence.md`, knowledge item `public-knowledge-base/licences/licence-compatibility.md`, knowledge item `public-knowledge-base/licences/creative-commons.md`.

# Code under one licence may enter a work under another only when the conditions of both can hold at once

Two licences are compatible in one direction when code under the first may be combined into a work distributed under the second.
The combined work must meet the conditions of both licences.
A permissive licence asks only for its notice, so code under it enters a work under almost any licence.
A copyleft licence asks that the whole work carry it, so code under it enters only a work under the same licence or a stricter one that it names.

The table gives the directions that the Free Software Foundation states for the GNU General Public License.

| Code under | May enter a work under `GPL-2.0-only` | May enter a work under `GPL-3.0-or-later` |
|---|---|---|
| `MIT`, `BSD-2-Clause`, `BSD-3-Clause`, `0BSD`, `Unlicense`, `CC0-1.0` | yes | yes |
| `Apache-2.0` | no, because of its clauses on patent termination and indemnification | yes |
| `MPL-2.0` | yes, through section 3.3, unless the file says it is incompatible with secondary licences | yes, through section 3.3 |
| `LGPL-3.0-or-later` | no | yes |
| `CC BY 4.0` | yes | yes |
| `CC BY-SA 4.0` | no | yes, in this direction alone |
| `LPPL-1.3c` | no, because a modified version must carry or point to the unmodified version | no |

The reverse direction fails for every copyleft licence.
Code under the GPL never enters a project under `MIT` or `Apache-2.0` unless the whole project moves to the GPL.

---

Source: the list of licences of the Free Software Foundation (https://www.gnu.org/licenses/license-list.html) and the FAQ of Creative Commons on the compatibility of CC BY-SA 4.0 (https://creativecommons.org/faq/), read on 2026-10-15.
Related: knowledge item `public-knowledge-base/licences/software-licences.md`, knowledge item `public-knowledge-base/licences/creative-commons.md`, knowledge item `public-knowledge-base/licences/lppl.md`.

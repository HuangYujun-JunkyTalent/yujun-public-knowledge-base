# Syncthing never syncs the `.stignore` of a folder, so each device includes a synced file of shared patterns, and a pattern without a leading slash matches at any depth

The file `.stignore` at the root of a synced folder holds the ignore patterns of that folder on one device.
Syncthing never sends `.stignore` to the other devices.
A line `#include <file>` reads the patterns of another file, whose path is relative to the folder of the file that includes it.
An included file is synced like any other file, so patterns shared by every device go into such a file, and the `.stignore` of each device holds one line:

```
#include .stignore-shared
```

A missing included file and a file included twice are errors.
Lines that start with `//` are comments.

A pattern without a leading `/` matches a file or a folder of that name at any depth, and a leading `/` anchors the pattern to the root of the folder.
The pattern `.git` therefore ignores every `.git` folder of the folder, nested repositories included, and every `.git` file of a submodule.
Syncthing 2.1.7 expands the pattern `.git` into the four patterns below, which the REST endpoint `/rest/db/ignores?folder=<id>` prints under `expanded`:

```
(?i).git
(?i)**/.git
(?i).git/**
(?i)**/.git/**
```

The prefix `(?i)` makes a pattern match whatever the case, and Syncthing adds it to every pattern on macOS and on Windows.
The prefix `(?d)` lets Syncthing delete the ignored files that keep a folder from being deleted when the folder is deleted on another device.
It suits files that a system or a tool regenerates, such as `.DS_Store`, `node_modules` and `__pycache__`.

---

Source: the page "Ignoring Files" at https://docs.syncthing.net/users/ignoring.html, read on 2026-10-28, and the expanded patterns of a folder on Syncthing 2.1.7 on macOS on 2026-10-28.
Related: knowledge item `public-knowledge-base/syncthing/change-detection-and-conflicts.md`, knowledge item `public-knowledge-base/git/working-tree-synced-without-git-folder.md`.

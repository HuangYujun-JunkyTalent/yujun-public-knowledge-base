# A `.git` folder that a sync tool copies between machines needs `core.checkStat minimal` and `core.trustctime false`, or every first `git status` on a machine rewrites the index

The index `.git/index` records for each tracked file its modification time, its change time, its size, its inode, its device and its owner.
`git status` compares these with the file and rewrites the index when they differ.
A sync tool such as Syncthing keeps the modification time and the size of a file, while the inode and the change time differ on each machine.
With the default checks, the first `git status` on each machine therefore rewrites the copied index, and the sync carries the new index to the other machine, where the same happens.

Two settings limit the checks to the whole seconds of the modification time and the size:

```sh
git config core.checkStat minimal
git config core.trustctime false
```

The settings sit in `.git/config`, which the sync carries with the rest of `.git`, so one machine sets them for every machine.
With them, `git status` leaves the copied index as it is, and after a commit on one machine the other machine shows a clean `git status` as soon as the sync has copied the files and `.git`.

A sync of `.git` still breaks when two machines change the same repository before the sync has carried the first change, such as two commits, as knowledge item `public-knowledge-base/syncthing/change-detection-and-conflicts.md` says of a conflict.
The objects of git have names derived from their content and never conflict, while the index, the branch references and `HEAD` then get conflict copies.

---

Source: a test with git 2.42.0 on two repositories of a scratch folder on 2026-10-28, with `rsync -a --checksum` of the whole repository in the place of the sync tool, and the description of `core.checkStat` and `core.trustctime` in the manual page `git-config(1)`.
Related: knowledge item `public-knowledge-base/git/working-tree-synced-without-git-folder.md`.

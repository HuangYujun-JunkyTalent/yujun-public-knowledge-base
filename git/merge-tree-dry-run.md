# `git merge-tree --write-tree` does a dry-run merge of two commits from git 2.38

`git merge-tree --write-tree <branch> <other>` computes the merge of two commits without touching the working tree or the index.
The command writes the merged tree object and prints its id.
When the merge has conflicts, the exit status is 1 and the output lists the conflicting files, so the command answers whether a pull would conflict before anything is pulled.
The option `--write-tree` exists in git 2.38 and later, and an older git has no dry-run merge.

---

Source: the git documentation of `git-merge-tree`, as used in the inspection before a pull in the ai-workspace since 2026-09-18.

# In a working tree that a sync tool copies without its `.git` folder, `git pull` refuses the commit that another machine pushed, and `git fetch` followed by `git reset @{u}` takes it in

A sync tool can copy the files of a repository between machines and leave out `.git`, so that git history moves only by push and fetch.
After machine A commits and pushes, the sync has already given machine B the files of the new commit, while the `.git` of B still names the old commit.
`git status --short` on B then shows the commit as local changes:

```
 D d
 M f
?? n
```

`git pull --ff-only` on B fetches and then stops, and changes nothing, although the files already hold the content of the commit:

```
error: Your local changes to the following files would be overwritten by merge:
	f
error: The following untracked working tree files would be overwritten by merge:
	n
Aborting
```

`git switch <branch>` stops in the same way after A has switched branch.

`git fetch && git reset @{u}` moves the branch and the index to the pushed commit and leaves the files as they are, and `git status` is then clean.
For a branch that A switched to, `git branch <branch> origin/<branch>`, then `git symbolic-ref HEAD refs/heads/<branch>`, then `git reset @{u}` give the same result.
`git reset --hard` would instead overwrite every file that differs from the commit, such as uncommitted work of A or a file that the sync has not copied yet, and the sync would carry the overwritten file back to A.

A machine that receives the files of a repository for the first time has no `.git` for them.
A clone without checkout gives it one without touching the files:

```sh
git clone --no-checkout --branch <branch> <remote URL> <temporary folder>
mv <temporary folder>/.git <repository>/.git
rmdir <temporary folder>
git -C <repository> reset
```

`git status` then lists the uncommitted changes of the sending machine.
A `.git` of an older clone of the same repository serves as well, and keeps its stashes and its unpushed branches.

Three habits keep the history right:

- A pushes before work moves to B, because a commit that A has not pushed shows on B as local changes, and committing them on B records the change twice.
- B takes in the commit before committing, because a commit on the old commit holds the changes of A and diverges from the remote.
- One machine pulls the commits that come from elsewhere, such as from a collaborator, and the other machines take them in as above, because the files reach them through the sync.
  `git reset @{u}` on a machine whose files have not received a commit leaves those files old, and `git status` then shows changes that undo the commit.

---

Source: tests with git 2.42.0 on repositories of a scratch folder on 2026-10-28, with `rsync -a --delete --exclude .git` in the place of the sync tool.
Related: knowledge item `public-knowledge-base/syncthing/ignore-files.md`, knowledge item `public-knowledge-base/git/git-folder-in-a-synced-folder.md`.

# Syncthing sends a change about 10 seconds after it happens and a deletion a minute later, and keeps both versions of a file changed on two devices

Syncthing finds changes in two ways.
A watcher of the file system collects the changes of a folder and sends them after the delay `fsWatcherDelayS`, 10 seconds by default.
A deletion waits one more minute.
A full scan runs every `rescanIntervalS`, 3600 seconds by default, at a random time between three quarters and five quarters of that interval, and finds the changes that the watcher missed.
During a scan, Syncthing compares the modification time, the size and the permissions of each file with its database, and hashes the files that differ.
The first scan of a folder hashes every file.

A conflict arises when a file changes on two devices before they sync.
Syncthing keeps the version with the newer modification time under the name of the file.
The other version becomes `<name>.sync-conflict-<date>-<time>-<device>.<extension>`, and syncs to every device like any other file.
When the modification times are equal, the version of the device whose identifier is larger in its first 63 bits becomes the conflict copy.
When a change on one device and a deletion on another conflict and the deletion wins, the changed file is kept as a conflict copy.

File versioning keeps the versions that a change from another device replaces or deletes, in `.stversions` at the root of the folder.
A version that an edit on the same device replaces is not kept.
The type `staggered` keeps the oldest version of every 30 seconds for the first hour, of every hour for the first day, of every day for the first 30 days, and of every week up to a maximum age.
The command `syncthing cli config folders <id> versioning type set staggered` sets it on a running Syncthing 2.1.7.

---

Source: the page "Understanding Synchronization" at https://docs.syncthing.net/users/syncing.html and the page "File Versioning" at https://docs.syncthing.net/users/versioning.html, read on 2026-10-28, and the defaults that `syncthing cli config folders add --help` of Syncthing 2.1.7 prints.
Related: knowledge item `public-knowledge-base/syncthing/ignore-files.md`, knowledge item `public-knowledge-base/syncthing/private-network-only.md`.

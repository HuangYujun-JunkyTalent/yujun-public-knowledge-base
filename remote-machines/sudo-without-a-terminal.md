# sudo asks a command without a terminal for the password every time, unless the time stamp is global or the rule needs no password

sudo remembers a password for `timestamp_timeout` minutes, 15 by default, in a time stamp record.
The option `timestamp_type` of sudoers decides which commands share a record, and its default is `tty`.
Under `tty`, each terminal has its own record, and a command without a terminal falls back to `ppid`, a record per parent process.
A command that an agent sends as `ssh <host> '<command>'` has no terminal and a new parent process each time.
Such a command therefore asks for the password again, and it has no terminal on which to answer the prompt.
`sudo -n <command>` then fails at once with `sudo: a password is required` instead of waiting for a password.

Three settings in a file under `/etc/sudoers.d/` let such a command run as root.

| Line | When the password is asked | Risk |
|---|---|---|
| `Defaults:<user> timestamp_type=global` | once, in a session with a terminal such as `ssh -t <host> sudo -v`, and the record then holds for every session of the user until the timeout | every process of the user has root until the timeout |
| `<user> ALL=(ALL) NOPASSWD: ALL` | never | whoever logs in as the user has root at any time |
| `<user> ALL=(root) NOPASSWD: /usr/bin/apt-get, /usr/bin/systemctl` | for every command outside the list | `apt-get` and `systemctl` can start a root shell, so the list guards against mistakes and is no security boundary |

`sudo visudo -f /etc/sudoers.d/<name>` edits such a file and refuses to save a file whose syntax is wrong.

The sudo of Debian 13 is 1.9.16p2.
On a Debian 13 machine on 2026-10-13, `sudo -n true` over SSH printed `sudo: a password is required` for a user of the group `sudo`.

---

Source: the manual page `sudoers(5)` of sudo 1.9.16p2 on Debian 13, entries `timestamp_type` and `timestamp_timeout`, read on 2026-10-13, and a run over SSH on Yujun Huang's request of 2026-10-13.

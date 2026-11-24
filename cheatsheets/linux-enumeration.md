# Linux Enumeration Cheat Sheet

## Overview

Post-access Linux enumeration focuses on understanding the current user's privileges, group memberships, running processes, and system configuration to identify a path to privilege escalation.

## Common Syntax

```bash
<enumeration-command>
```

## Useful Commands

| Command | Description |
|---|---|
| `id` | Show current user's UID, GID, and group memberships |
| `whoami` | Show current username |
| `sudo -l` | List commands the current user can run with `sudo` |
| `uname -a` | Show kernel version and architecture |
| `cat /etc/os-release` | Show OS/distribution details |
| `ls -la` | List directory contents with permissions, including hidden files |
| `find / -perm -4000 2>/dev/null` | Find SUID binaries |
| `ip a` | List network interfaces and IP addresses |

## Important Options

| Option | Description |
|---|---|
| `-la` (with `ls`) | Long format, including hidden (dotfile) entries |
| `-perm -4000` (with `find`) | Match files with the SUID bit set |
| `2>/dev/null` | Suppress permission-denied errors during a system-wide search |

## Examples

Used in this lab:

```bash
id
```

```text
uid=1000(john) gid=1000(john) groups=1000(john),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
```

```bash
ls -la
```

```text
total 60
drwxr-xr-x 8 john john  4096 Jul 27  2020 .
...
-rw-rw-r-- 1 john john    33 Feb  5  2020 user.txt
```

## Common Mistakes

- Only checking `sudo -l` and stopping there — group memberships like `lxd`, `docker`, and `disk` can be just as significant as (or more significant than) sudo rights.
- Not checking symlinked files like `.bash_history -> /dev/null`, which can indicate deliberate anti-forensics configuration worth noting.

## References

- [LinEnum GitHub Repository](https://github.com/rebootuser/LinEnum)
- [linPEAS (PEASS-ng) GitHub Repository](https://github.com/peass-ng/PEASS-ng)

## Key Takeaways

- `id` is one of the fastest, highest-value commands to run immediately after gaining a shell — group memberships alone can reveal a full privilege escalation path.
- Manual enumeration and automated tooling such as LinEnum and linpeas complement each other; don't rely on only one approach.

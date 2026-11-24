# FTP Cheat Sheet

> [!NOTE]
> General-purpose reference material. No FTP service was encountered in this specific lab engagement — see [`docs/04-FTP`](../docs/04-FTP/README.md).

## Overview

FTP (File Transfer Protocol) is a plaintext protocol, typically running on TCP port 21, used for transferring files between a client and server.

## Common Syntax

```bash
ftp <target>
```

## Useful Commands

| Command | Description |
|---|---|
| `ftp <target>` | Connect to an FTP server interactively |
| `nmap -p 21 -sV --script ftp-anon <target>` | Check for anonymous FTP login via Nmap script |
| `ls` / `dir` (within FTP session) | List remote directory contents |
| `get <file>` | Download a file from the server |
| `put <file>` | Upload a file to the server |
| `binary` | Switch transfer mode to binary (important for non-text files) |

## Important Options

| Option | Description |
|---|---|
| `anonymous` login | Username `anonymous`, password often blank or an email address |
| `-sV` (Nmap) | Detect FTP server version for known-vulnerability lookup |
| `--script ftp-anon` (Nmap) | Nmap NSE script to test for anonymous access |

## Examples

General reference examples (not run in this lab):

```bash
# Check for anonymous access
nmap -p 21 --script ftp-anon 10.10.10.10

# Connect manually
ftp 10.10.10.10
Name: anonymous
Password: <blank or anything>
```

## Common Mistakes

- Forgetting to switch to `binary` mode before downloading non-text files, which can corrupt them.
- Overlooking anonymous login as a possibility before attempting credential brute-forcing.

## References

- [Nmap NSE `ftp-anon` script documentation](https://nmap.org/nsedoc/scripts/ftp-anon.html)

## Key Takeaways

- Always test for anonymous FTP access before attempting to brute-force credentials.
- This cheat sheet exists for future reference; no FTP-specific findings exist in this repository yet.

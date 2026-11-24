# 04 — FTP

## Overview

No FTP service was identified during this engagement. The reconnaissance scan (see [`docs/01-Recon`](../01-Recon/README.md)) confirmed that only ports **22 (SSH)** and **80 (HTTP)** were open on the target. This document describes the verification performed and links to general FTP methodology for future engagements where the service is present.

## Objectives

- Confirm whether an FTP service is present on the target.
- Record the methodology used to reach that conclusion.

## Important Terminology

| Term | Definition |
|---|---|
| FTP | File Transfer Protocol, a plaintext protocol for transferring files, typically on TCP port 21 |
| Anonymous login | An FTP configuration that allows login with the username `anonymous` and no real password |
| Passive/Active mode | The two ways FTP can negotiate its data connection channel |

## Concepts Explained

Confirming the absence of a service is as important as documenting its presence. A full-range port scan is the standard way to rule out FTP (or any service) rather than assuming it based on default or common port coverage alone.

## Commands and Syntax

```bash
sudo nmap -A -T5 10.128.133.75
```

## Command Breakdown

See [`docs/01-Recon`](../01-Recon/README.md#command-breakdown) for the full breakdown of this command. Its result is what ruled out FTP for this target.

## Examples

**Result relevant to this stage** (from the reconnaissance scan):

```text
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
```

No entry for port 21 (or any other FTP-associated port) appears in the scan output.

## Screenshots

No screenshots apply to this stage. Port 21 does not appear in the reconnaissance results; see [`docs/01-Recon`](../01-Recon/README.md) for the full scan screenshot.

## Observations

- The target exposes only SSH and HTTP; FTP is out of scope for this engagement.

## Common Mistakes

- Assuming a target has no FTP service without explicitly checking. Always confirm with a full port range scan (`-p-`) rather than relying on a default top-1000-ports scan alone.
- Overlooking non-standard FTP ports; some deployments run FTP on a port other than 21.

## Notes

This document will be updated with real findings, commands, and screenshots the next time an FTP service is encountered in a lab engagement.

## Additional Context

> [!NOTE]
> The following is general reference material and was not observed in this lab.

FTP services are commonly assessed for anonymous authentication, outdated server software with known CVEs, and writable directories that could be leveraged for further access if the FTP root overlaps with a web root.

## References

- [`cheatsheets/ftp.md`](../../cheatsheets/ftp.md)

## Key Takeaways

- Always verify service presence with a full port scan before ruling a protocol out of scope.
- Documenting a negative result is part of a complete methodology, not a gap in it.

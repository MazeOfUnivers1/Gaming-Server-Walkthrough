# Nmap Cheat Sheet

## Overview

Nmap ("Network Mapper") is a network scanning tool used for host discovery, port scanning, service/version detection, and OS fingerprinting.

## Common Syntax

```bash
nmap [scan type] [options] <target>
```

## Useful Commands

| Command | Description |
|---|---|
| `nmap <target>` | Basic scan of the top 1000 TCP ports |
| `nmap -p- <target>` | Scan all 65535 TCP ports |
| `nmap -sV <target>` | Service/version detection |
| `nmap -sC <target>` | Run default Nmap scripts |
| `nmap -A <target>` | OS detection + version detection + script scanning + traceroute |
| `nmap -O <target>` | OS detection only |
| `nmap -sU <target>` | UDP scan |
| `nmap -Pn <target>` | Skip host discovery (treat host as up) |
| `nmap -T<0-5> <target>` | Set timing template (0 = slowest/stealthiest, 5 = fastest) |

## Important Options

| Option | Description |
|---|---|
| `-A` | Aggressive scan (OS, version, scripts, traceroute) |
| `-Pn` | Skip host discovery |
| `-p-` | Scan all ports |
| `-sV` | Version detection |
| `-sC` | Default script scan |
| `-T5` | Insane timing (fastest, least stealthy) |
| `-oA <basename>` | Output in all formats (normal, XML, grepable) |

## Examples

Used in this lab:

```bash
sudo nmap -A -T5 10.128.133.75
```

General reference examples:

```bash
# Full port sweep before a targeted scan
nmap -p- -T4 10.10.10.10

# Follow-up detailed scan against discovered ports only
nmap -sC -sV -p 22,80 10.10.10.10
```

## Common Mistakes

- Only scanning the top 1000 ports and missing services on unusual high ports.
- Using `-T5` against production/real-world targets, risking dropped scan accuracy or triggering alerts.
- Forgetting `-Pn` against hosts that don't respond to ICMP ping, resulting in "host seems down" false negatives.

## References

- [Nmap Official Documentation](https://nmap.org/book/man.html)

## Key Takeaways

- `-A` is a fast way to get a broad picture of a target in one scan, ideal for lab environments.
- Always confirm findings with a full port range scan before concluding enumeration.

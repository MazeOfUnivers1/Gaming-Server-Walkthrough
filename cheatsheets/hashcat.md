# Hashcat Cheat Sheet

> [!NOTE]
> `hashcat` itself was not used in this specific lab (John the Ripper was used instead — see [`docs/05-Credentials`](../docs/05-Credentials/README.md)). This cheat sheet is provided as general reference material for the same class of task: offline hash/passphrase cracking.

## Overview

Hashcat is a GPU-accelerated password/hash cracking tool supporting a wide range of hash types (via numeric "mode" identifiers).

## Common Syntax

```bash
hashcat -m <mode> -a <attack mode> <hashfile> <wordlist>
```

## Useful Commands

| Command | Description |
|---|---|
| `hashcat -m 0 hash.txt rockyou.txt` | Crack MD5 hashes using a wordlist attack |
| `hashcat -m 1800 hash.txt rockyou.txt` | Crack sha512crypt hashes |
| `hashcat --example-hashes` | List all supported hash modes with examples |
| `hashcat -a 3 hash.txt ?a?a?a?a?a?a` | Brute-force (mask) attack |
| `hashcat --show hash.txt` | Show already-cracked results without re-running |

## Important Options

| Option | Description |
|---|---|
| `-m` | Hash mode (algorithm identifier) |
| `-a` | Attack mode (0 = wordlist, 1 = combinator, 3 = mask/brute-force, 6/7 = hybrid) |
| `-r` | Apply a rules file to mutate wordlist entries |
| `--show` | Display cracked hashes from a previous session |
| `-o` | Output cracked results to a file |

## Examples

General reference examples (not run in this lab):

```bash
# Crack an SSH key hash (produced via ssh2john) with a wordlist
hashcat -m 22921 hash.txt rockyou.txt

# Apply a rules file for mutated guesses
hashcat -m 0 -a 0 hash.txt rockyou.txt -r rules/best64.rule
```

## Common Mistakes

- Using the wrong `-m` mode for the hash type, resulting in no matches even with a correct wordlist.
- Forgetting `--show` after a session completes, and re-running the full attack unnecessarily.

## References

- [Hashcat Official Documentation](https://hashcat.net/wiki/)

## Key Takeaways

- Identify the correct hash mode (`-m`) before starting a cracking session; an incorrect mode guarantees failure.
- John the Ripper, not Hashcat, was the tool actually used in this lab (see [`docs/05-Credentials`](../docs/05-Credentials/README.md)); the two tools solve the same class of problem and are often interchangeable depending on hash type and available hardware.

# 05 — Credential & Key Discovery

## Overview

The two directories discovered during enumeration (`/uploads` and `/secret`) both had directory listing enabled, exposing their contents directly. This stage covers what was found in each, and how an encrypted SSH private key was cracked offline.

## Objectives

- Enumerate the contents of the discovered `/uploads` and `/secret` directories.
- Identify any files of security value (credentials, keys, wordlists).
- Recover the passphrase protecting an exposed encrypted SSH private key.

## Important Terminology

| Term | Definition |
|---|---|
| Directory listing | A web server feature (Apache: `Options +Indexes`) that displays file listings for a directory with no index file |
| Encrypted private key | An SSH private key whose contents are encrypted with a passphrase, requiring the passphrase to be decrypted before use |
| `ssh2john` | A John the Ripper helper script that converts an SSH private key into a crackable hash format |
| Offline cracking | Password/passphrase recovery performed against a captured hash, without interacting with the live target |

## Concepts Explained

An encrypted SSH private key file (`BEGIN RSA PRIVATE KEY` with a `Proc-Type: 4,ENCRYPTED` header) cannot be used for authentication until it is decrypted with its passphrase. If that passphrase is weak, it can be recovered offline using `ssh2john` to extract a crackable hash, followed by `john` running against a wordlist.

## Commands and Syntax

### Step 1 — Enumerate `/uploads`

Browsed directly in Firefox:

```text
http://10.128.133.75/uploads/
```

### Step 2 — Enumerate `/secret`

Browsed directly in Firefox:

```text
http://10.128.133.75/secret/
```

### Step 3 — Save and prepare the private key

```bash
touch id_rsa
vim id_rsa
chmod 600 id_rsa
```

### Step 4 — Convert the key to a crackable hash

```bash
ssh2john id_rsa > hash.txt
```

### Step 5 — Crack the passphrase

```bash
john --show hash.txt
```

## Command Breakdown

| Command | Purpose |
|---|---|
| `chmod 600 id_rsa` | SSH requires strict permissions on private key files (owner read/write only) before it will use them |
| `ssh2john id_rsa > hash.txt` | Extracts a crackable hash representation of the encrypted key into `hash.txt` |
| `john --show hash.txt` | Displays any already-cracked results for the given hash file (used here after John had already run against the hash and recovered the passphrase) |

## Examples

**`/uploads` directory listing:**

| File | Size | Modified |
|---|---|---|
| `dict.lst` | 2.0K | 2020-02-05 14:10 |
| `manifesto.txt` | 3.0K | 2020-02-05 13:05 |
| `meme.jpg` | 15K | 2020-02-05 13:32 |

`dict.lst` was fetched directly (`http://10.128.133.75/uploads/dict.lst`) and turned out to be a **custom password wordlist** — a mix of seasonal date-based passwords (`Spring2017`, `summer2016`, `Winter2013`, etc.) and generic/common passwords (`P@55w0rd`, `Welcome123`, `password1`, `security1`, `sqlserver`, `changeme`, and more).

**`/secret` directory listing:**

| File | Size | Modified |
|---|---|---|
| `secretKey` | 1.7K | 2020-02-05 13:41 |

`secretKey` (`http://10.128.133.75/secret/secretKey`) was an **encrypted RSA private key**:

```text
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,82823EE792E75948EE2DE731AF1A0547

T7+F+3ilm5FcFZx24mnrugMY455vI461ziMb4NYk9YJV5uwcrx4QflP2Q2Vk8phx
...
-----END RSA PRIVATE KEY-----
```

**Cracking the passphrase:**

```text
$ ssh2john id_rsa > hash.txt
$ john --show hash.txt
id_rsa:letmein

1 password hash cracked, 0 left
```

### Rationale

The custom wordlist (`dict.lst`) found in `/uploads` was a strong signal that credential material was somewhere on this host. The `/secret` directory, found via `ffuf`, delivered exactly that: an encrypted private key. `ssh2john` + `john` is the standard workflow for recovering a private key passphrase offline once the encrypted key file itself has been obtained.

## Screenshots

![Uploads directory listing](../../images/uploads-directory-listing.png)

![dict.lst wordlist contents](../../images/dict-wordlist-found.png)

![Secret directory listing](../../images/secret-directory-listing.png)

![secretKey encrypted RSA private key contents](../../images/secretkey-rsa-private-key.png)

![ssh2john and john cracking the key passphrase](../../images/ssh2john-john-crack-passphrase.png)

## Observations

- `/uploads/dict.lst` is a **custom, target-specific wordlist** rather than a well-known public list — this is a strong hint that it was intentionally placed for use against this specific target's credential material.
- `/secret/secretKey` is an **AES-128-CBC-encrypted RSA private key**, requiring a passphrase before it can be used for SSH authentication.
- The passphrase recovered by `john` was **`letmein`**, a simple, weak, dictionary-guessable value. It does not appear in the `dict.lst` excerpt above, suggesting John's default wordlist and rules cracked it rather than `dict.lst` itself.
- `manifesto.txt` and `meme.jpg` were also present in `/uploads` but their contents were not captured in these notes.

## Common Mistakes

- Forgetting to `chmod 600` a downloaded private key before use — SSH will refuse to use a key with overly permissive file permissions.
- Assuming an encrypted private key is unusable without its passphrase — always attempt offline cracking with `ssh2john` + `john` before discarding a key as a dead end.
- Overlooking companion files in a directory listing (like `manifesto.txt`, `meme.jpg`) that may contain additional context, even when one file (`secretKey`) is the obvious prize.

## Notes

The exact wordlist/ruleset `john` used to crack the passphrase was not captured in the terminal output — only the final `john --show hash.txt` confirmation was recorded.

## Additional Context

> [!NOTE]
> Supplementary information, not directly captured in this lab's terminal output.

`john`'s default behavior when run without an explicit `--wordlist` flag is to fall back to its "single crack" mode followed by its default wordlist (`password.lst`) and mangling rules. Given the simplicity of the cracked passphrase (`letmein`), it is consistent with a default wordlist + rules run rather than requiring the custom `dict.lst` at all.

## References

- [John the Ripper — Official Documentation](https://www.openwall.com/john/)

## Key Takeaways

- Two exposed, listable directories (`/uploads`, `/secret`) leaked both a custom password wordlist and an encrypted private key.
- `ssh2john` + `john` recovered the key's passphrase (`letmein`) offline, without any interaction with the live SSH service.
- This credential chain — web misconfiguration → key exposure → offline cracking — directly enabled the initial access documented in [`docs/06-Initial-Access`](../06-Initial-Access/README.md).

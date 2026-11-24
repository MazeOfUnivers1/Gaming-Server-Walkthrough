# FFUF Cheat Sheet

## Overview

`ffuf` (Fuzz Faster U Fool) is a fast web fuzzer used for directory/file brute-forcing, virtual host discovery, and parameter fuzzing.

## Common Syntax

```bash
ffuf -w <wordlist>:<KEYWORD> -u <url-with-KEYWORD>
```

## Useful Commands

| Command | Description |
|---|---|
| `ffuf -w wordlist.txt -u http://target/FUZZ` | Basic directory fuzzing |
| `ffuf -w wordlist.txt:fuzz -u http://target/fuzz` | Fuzzing with a named keyword |
| `ffuf -w wordlist.txt -u http://target/FUZZ -e .php,.txt,.bak` | Fuzz with common file extensions |
| `ffuf -w wordlist.txt -u http://target/FUZZ -recursion` | Recursive fuzzing into discovered directories |
| `ffuf -w wordlist.txt -H "Host: FUZZ.target.com" -u http://target/` | Virtual host fuzzing |
| `ffuf -w wordlist.txt -u http://target/FUZZ -fc 404` | Filter out a specific status code |

## Important Options

| Option | Description |
|---|---|
| `-w` | Wordlist to use (optionally `path:KEYWORD`) |
| `-u` | Target URL, must contain the fuzz keyword |
| `-e` | Extensions to append to each wordlist entry |
| `-fc` | Filter by HTTP status code (exclude) |
| `-mc` | Match by HTTP status code (include) |
| `-t` | Number of concurrent threads |
| `-recursion` | Recurse into discovered directories |
| `-o` | Output results to a file |

## Examples

Used in this lab:

```bash
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:fuzz -u http://10.128.133.75/fuzz
```

General reference examples:

```bash
# Fuzz for common web file extensions
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://target/FUZZ -e .php,.html,.bak

# Filter out noisy 404 responses explicitly
ffuf -w wordlist.txt -u http://target/FUZZ -fc 404
```

## Common Mistakes

- Not filtering/calibrating against "soft 404" responses (pages that return `200 OK` for non-existent paths).
- Using an overly broad wordlist for a quick scan when a smaller, targeted list would be faster and just as effective.
- Missing a `301` redirect result and not following up by browsing the discovered path directly.

## References

- [ffuf GitHub Repository](https://github.com/ffuf/ffuf)

## Key Takeaways

- `ffuf` is fast and flexible; always double-check discovered `301`/`302` results manually.
- Pairing directory brute-forcing with manual browsing of results (as in this lab) often reveals more than automation alone.

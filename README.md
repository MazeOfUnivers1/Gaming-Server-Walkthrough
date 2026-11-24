# Cybersecurity Lab Notes — "House of Danak" Engagement

> A personal, structured knowledge base documenting a hands-on penetration testing lab exercise, built from real commands, real terminal output, and real screenshots captured during the engagement.

![Status](https://img.shields.io/badge/status-complete-brightgreen)
![Type](https://img.shields.io/badge/type-lab%20notes-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)
![Made with](https://img.shields.io/badge/made%20with-Markdown-informational)

---

## About This Repository

This is **not** a copy-paste walkthrough. It is a personal cybersecurity knowledge base built from my own lab notes, terminal sessions, and screenshots taken while working through a vulnerable target machine on an isolated lab network (`10.128.133.75` / `10.130.138.103`, later re-provisioned).

The goal of this repository is to document, in a reusable and reference-friendly format, the **methodology** used at each stage of the assessment: reconnaissance, enumeration, web analysis, credential discovery, initial access, and privilege escalation — along with general-purpose cheat sheets for the tools used.

Every command, every piece of terminal output, and every screenshot in the `docs/` folders reflects something that was actually run or observed in this lab. Where extra background knowledge is included to help explain a concept, it is clearly separated into an **Additional Context** section.

---

## Table of Contents

- [About This Repository](#about-this-repository)
- [Repository Structure](#repository-structure)
- [Learning Objectives](#learning-objectives)
- [Topics Covered](#topics-covered)
- [Navigation](#navigation)
- [Tools Used](#tools-used)
- [Repository Statistics](#repository-statistics)
- [Future Improvements](#future-improvements)
- [Acknowledgements](#acknowledgements)
- [License](#license)

---

## Repository Structure

```
.
├── README.md                       # This file
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── 01-Recon/                   # Host discovery & Nmap scanning
│   ├── 02-Enumeration/             # Service & directory enumeration
│   ├── 03-Web/                     # Web application analysis
│   ├── 04-FTP/                     # FTP methodology reference
│   ├── 05-Credentials/             # Credential & key discovery, cracking
│   ├── 06-Initial-Access/          # SSH foothold
│   └── 07-Privilege-Escalation/    # LXD group privilege escalation
│
├── cheatsheets/
│   ├── nmap.md
│   ├── ffuf.md
│   ├── ftp.md
│   ├── hashcat.md
│   ├── netcat.md
│   ├── linux-enumeration.md
│   ├── privilege-escalation.md
│   └── reverse-shells.md
│
├── images/                         # Screenshots referenced throughout docs/
└── resources/                      # Supplementary reference material
```

---

## Learning Objectives

By working through this lab and documenting it, the objectives were to:

- Practice a structured, repeatable penetration testing methodology (Recon → Enumeration → Exploitation → Privilege Escalation).
- Get hands-on experience with `nmap`, `ffuf`, `john`, `ssh2john`, and `ssh`.
- Understand how misconfigured file upload/static directories can leak sensitive material (wordlists, private keys).
- Understand how encrypted SSH private keys can be cracked when a weak passphrase is used.
- Understand how Linux group membership (specifically the `lxd` group) can be abused for privilege escalation.

---

## Topics Covered

| Category | Description |
|---|---|
| Reconnaissance | Nmap service/version scanning of a target host |
| Enumeration | Directory and file discovery with `ffuf` |
| Web Analysis | Reviewing page source (`view-source:`) for developer comments / usernames |
| Credential Discovery | Locating an exposed encrypted SSH private key and a custom password wordlist |
| Password Cracking | Using `ssh2john` + `john` to recover a private key passphrase |
| Initial Access | Authenticating over SSH using a cracked private key |
| Privilege Escalation | Abusing `lxd` group membership to escalate to root via an LXC container |

---

## Navigation

| Stage | Folder |
|---|---|
| 1. Reconnaissance | [`docs/01-Recon`](docs/01-Recon/README.md) |
| 2. Enumeration | [`docs/02-Enumeration`](docs/02-Enumeration/README.md) |
| 3. Web Analysis | [`docs/03-Web`](docs/03-Web/README.md) |
| 4. FTP Reference | [`docs/04-FTP`](docs/04-FTP/README.md) |
| 5. Credentials | [`docs/05-Credentials`](docs/05-Credentials/README.md) |
| 6. Initial Access | [`docs/06-Initial-Access`](docs/06-Initial-Access/README.md) |
| 7. Privilege Escalation | [`docs/07-Privilege-Escalation`](docs/07-Privilege-Escalation/README.md) |
| Cheat Sheets | [`cheatsheets/`](cheatsheets/) |

---

## Tools Used

| Tool | Purpose |
|---|---|
| `nmap` | Host and service discovery |
| Firefox (`view-source:`) | Manual web application review |
| `ffuf` | Directory/file brute-forcing |
| `ssh2john` | Convert an encrypted SSH private key into a crackable hash format |
| `john` (John the Ripper) | Offline passphrase cracking |
| `ssh` | Remote authentication |
| `lxc` / `lxd` | Container management (abused for privilege escalation) |
| `python3 -m http.server` | Lightweight file transfer between attacker and target |

---

## Repository Statistics

| Metric | Count |
|---|---|
| Documentation stages | 7 |
| Cheat sheets | 8 |
| Screenshots | 15 |
| Machines documented | 1 |

---

## Future Improvements

- [ ] Add FTP findings if/when an FTP service is encountered in a future engagement.
- [ ] Expand the Linux enumeration cheat sheet with additional `linpeas.sh` output analysis.
- [ ] Add a dedicated Active Directory section if future labs include Windows domain targets.
- [ ] Add automated note-taking templates for future engagements.

---

## Acknowledgements

- Formatting and structure inspired by community projects such as **HackTricks**, **PayloadsAllTheThings**, and **OWASP** documentation.
- Target web application template originally sourced from `freewebsitetemplates.com` (identified via page source, see [`docs/03-Web`](docs/03-Web/README.md)).

---

## License

This repository is licensed under the [MIT License](LICENSE). Lab target details (IP addresses, keys, hashes) are specific to an isolated, private lab environment and hold no value outside of that context.

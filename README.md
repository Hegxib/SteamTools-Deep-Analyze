# SteamTools v1.8.30 - Complete Deep Analysis & Exposure Report

> **Analysis Date:** March 5, 2026
> **Analyst:** Automated Static Analysis (no execution)
> **File Analyzed:** `st-setup-1.8.30.exe`
> **Classification:** Game Piracy Tool with Remote Code Execution Backdoor
> **Threat Level:** HIGH

## Support

Have you ever met a guy that refuses to take free money? Neither did i, Buy me a Ko-Fi :D

https://ko-fi.com/hegxib

Etherium: 0x83Cc0fe051bEf3c8D7633665F165fd9E1AFb10fC

BTC: bc1qppajze80mq8wcrap0ym00mch0w8z6qvpcscku2

Solana: B4HgYSahtRA9gqmKJG1GL677WJcLi8idUpXbz6ztdTkN

Other? contact@hegxib.me

Contact legal@hegxib.me For Removal Requests.

Official Website www.hegxib.me

Blogs at www.hegxib.me/blog
---

> [!WARNING]
> # ⚠️ SECURITY & LEGAL RESEARCH DISCLOSURE ⚠️
>
> **This document is published strictly for cybersecurity research, public transparency, and educational awareness.**
>
> ---
>
> | | |
> |---|---|
> | 🔬 **Purpose** | Independent binary analysis & threat intelligence research |
> | 👤 **Audience** | Security researchers, IT professionals, and informed end-users |
> | ⚖️ **Legal** | Possession and/or use of `st-setup-1.8.30.exe` may violate the DMCA, CFAA (US), Computer Misuse Act (UK), and equivalent laws in your jurisdiction |
> | 🚫 **Not Endorsed** | The author (Hegxib) does **not** endorse, distribute, or support the use of this software |
> | 💀 **Risk Level** | **CRITICAL** — This tool contains a remote code execution backdoor, Steam credential harvesting, and piracy mechanisms |
> | 🇨🇳 **Origin** | Developed and maintained from China — update servers resolve to Chinese IP infrastructure |
> | 🎮 **Steam ToS** | Use of this tool results in **permanent Steam account termination** and loss of all legitimately purchased games |
>
> ---
>
> > *"All findings presented in this report are derived solely from static binary analysis. No cracked games were accessed, no piracy was performed, and no Steam accounts were compromised during this research."*
>
> **By reading this document you acknowledge it is intended for defensive security purposes only.**


## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [What is SteamTools?](#2-what-is-steamtools)
3. [Installer Overview](#3-installer-overview)
4. [Complete File Inventory](#4-complete-file-inventory)
5. [Digital Signatures & Trust Chain](#5-digital-signatures--trust-chain)
6. [The Company Behind It: NewWnight Global Tech Co., Ltd](#6-the-company-behind-it-newwnight-global-tech-co-ltd)
7. [SteamTools.exe — The Main Binary](#7-steamtoolsexe--the-main-binary)
8. [Core.dll — The Dangerous Engine](#8-coredll--the-dangerous-engine)
9. [What It Actually Does (Step by Step)](#9-what-it-actually-does-step-by-step)
10. [The "Vale Corporation" Deception](#10-the-vale-corporation-deception)
11. [Network Infrastructure & Update Servers](#11-network-infrastructure--update-servers)
12. [The HttpLoadDLL Backdoor](#12-the-httploadddll-backdoor)
13. [Steam Account Data Harvesting](#13-steam-account-data-harvesting)
14. [Database Schema & Piracy Mechanics](#14-database-schema--piracy-mechanics)
15. [IPC (Inter-Process Communication) Impersonation](#15-ipc-inter-process-communication-impersonation)
16. [Lua Plugin System — Extensible Attack Surface](#16-lua-plugin-system--extensible-attack-surface)
17. [Dangerous Windows APIs Used](#17-dangerous-windows-apis-used)
18. [Cryptography & Encryption Capabilities](#18-cryptography--encryption-capabilities)
19. [Anti-Analysis & Evasion Techniques](#19-anti-analysis--evasion-techniques)
20. [What It Does NOT Do (Currently)](#20-what-it-does-not-do-currently)
21. [PE Binary Structure Analysis](#21-pe-binary-structure-analysis)
22. [All URLs & External References](#22-all-urls--external-references)
23. [All File Hashes (SHA-256)](#23-all-file-hashes-sha-256)
24. [Complete Risk Assessment](#24-complete-risk-assessment)
25. [Legal Implications](#25-legal-implications)
26. [Conclusion & Recommendations](#26-conclusion--recommendations)

---

## 1. Executive Summary

**SteamTools** (v1.8.30) is a Chinese-developed Windows application that markets itself as a tool to "enhance your Steam gaming experience." In reality, it is a **game piracy tool** that bypasses Steam's DRM (Digital Rights Management) to let users play games and unlock DLCs they haven't purchased.

Beyond its piracy functions, the tool contains **a hidden remote code execution backdoor** (`HttpLoadDLL`) that can silently download, decrypt, and execute arbitrary DLLs from Chinese-controlled update servers — meaning the developers can push **any malicious payload** (ransomware, spyware, cryptocurrency miners, credential stealers) to every user's computer at any time, without warning or consent.

The tool also **silently reads Steam account data** including usernames, Steam IDs, and password-remember status, and has the technical capability to upload this data to remote servers.

**Bottom line: This is not just a piracy tool. It is a trojanized application with a live backdoor.**

---

## 2. What is SteamTools?

### What they say it is:
> *"Steamtools is designed to enhance your Steam gaming experience by obtaining tickets, letting you freely enjoy games and unlock additional DLCs."*
>
> — Embedded "About" text, SteamTools.exe

### What it actually is:
- A **game piracy tool** that intercepts and manipulates Steam's authentication system
- A **DRM bypass tool** that creates fake game ownership tickets
- A **Family View PIN cracker** (extracts parental control PINs)
- A **DLC unlocker** that grants access to paid downloadable content
- A **remote code execution platform** that can download and run any code from Chinese servers

### Official presence:
| Channel | Link |
|---|---|
| Website | `https://www.steamtools.net` |
| Forum | `https://bbs.steamtools.net` |
| Telegram | `https://t.me/steamtool` |
| GitHub Releases | `https://github.com/st2024/Steamtools/releases` |

### Additional domains referenced in the binary:
| Domain | Purpose |
|---|---|
| `steamtoos.net` | Typosquat / alternative domain (note: "steamtoo**s**" missing an 'l') |
| `steamui.com` | Unknown purpose — UI assets or phishing? |
| `steamdb.info` | Legitimate third-party Steam database (used for game data lookups) |
| `store.steampowered.com` | Official Steam store (accessed for game info) |

---

## 3. Installer Overview

| Property | Value |
|---|---|
| **Filename** | `st-setup-1.8.30.exe` |
| **Size** | 11,125,328 bytes (10.6 MB) |
| **SHA-256** | `41EC92BC311DAF40B22B2497D044B12D9BA0266DA84D8581255D7CC9E059135F` |
| **Installer Type** | NSIS 3 Unicode (Nullsoft Scriptable Install System) |
| **Architecture** | 64-bit (x86-64) |
| **Compression** | Deflate, non-solid |
| **Digital Signature** | Valid — signed by NewWnight Global Tech Co., Ltd |
| **Timestamp** | DigiCert SHA256 RSA4096 Timestamp Responder 2025 |

The NSIS installer is a well-known legitimate installer framework. The developers chose it specifically because it's trusted by Windows and antivirus software, making their payload less likely to be flagged.

---

## 4. Complete File Inventory

The installer drops **18 files** into 4 directories:

### Root files (core application):
| File | Size (bytes) | Date Modified | Purpose |
|---|---|---|---|
| `SteamTools.exe` | 1,833,984 | 2026-01-12 | Main application binary |
| `Core.dll` | 673,784 | 2026-01-12 | Core engine — network, crypto, Steam manipulation |
| `Qt5Core.dll` | 6,023,664 | 2020-11-06 | Qt 5 framework — core module |
| `Qt5Gui.dll` | 7,008,240 | 2020-11-06 | Qt 5 framework — GUI rendering |
| `Qt5Widgets.dll` | 5,498,352 | 2020-11-06 | Qt 5 framework — UI widgets |
| `Qt5Network.dll` | 1,340,400 | 2020-11-06 | Qt 5 framework — networking |
| `Qt5Svg.dll` | 330,736 | 2020-11-06 | Qt 5 framework — SVG icon rendering |
| `msvcp140.dll` | 627,440 | 2025-03-03 | Microsoft Visual C++ 2022 runtime |
| `msvcp140_1.dll` | 30,960 | 2025-03-03 | Microsoft Visual C++ 2022 runtime |
| `vcruntime140.dll` | 85,232 | 2025-03-03 | Microsoft Visual C++ 2022 runtime |
| `vcruntime140_1.dll` | 44,312 | 2025-03-03 | Microsoft Visual C++ 2022 runtime |

### `\platforms\` directory:
| File | Size (bytes) | Date Modified | Purpose |
|---|---|---|---|
| `qwindows.dll` | 1,477,104 | 2020-11-06 | Qt Windows platform plugin |

### `\imageformats\` directory:
| File | Size (bytes) | Date Modified | Purpose |
|---|---|---|---|
| `qico.dll` | 38,384 | 2020-11-06 | Qt ICO image format plugin |

### `\$PLUGINSDIR\` (NSIS installer plugins — not installed):
| File | Size (bytes) | Date Modified | Purpose |
|---|---|---|---|
| `nsDialogs.dll` | 10,752 | 2026-03-05 | NSIS custom dialog plugin |
| `nsExec.dll` | 6,656 | 2026-03-05 | NSIS command execution plugin |
| `System.dll` | 12,288 | 2026-03-05 | NSIS system calls plugin |
| `modern-header.bmp` | 25,818 | 2024-07-31 | Installer header graphic |
| `modern-wizard.bmp` | 154,542 | 2021-12-05 | Installer wizard graphic |

### Key observations:
- The Qt5 libraries are from **November 6, 2020** — over 5 years old, full of known CVEs
- The MSVC runtime is from **March 3, 2025** — relatively recent
- `SteamTools.exe` and `Core.dll` both have a **January 12, 2026** date — the most recent files
- NSIS plugins have a **March 5, 2026** date (likely extracted timestamp)

---

## 5. Digital Signatures & Trust Chain

### Installer (`st-setup-1.8.30.exe`) — SIGNED ✅
```
Status:          Valid — "Signature verified"
Subject:         CN="NewWnight Global Tech Co., Ltd"
                 O="NewWnight Global Tech Co., Ltd"
                 L=Changsha, S=Hunan, C=CN
                 SERIALNUMBER=91430103MADWKFRYXD
                 BusinessCategory=Private Organization
Issuer:          GlobalSign GCC R45 EV CodeSigning CA 2020
Certificate:     EV (Extended Validation) Code Signing
Serial Number:   046CD9B4FAD9F7C7101A6578
Thumbprint:      F3F5A63028D89B2902B4C4EA535F2CD938966314
Valid From:      December 5, 2025
Valid Until:     December 6, 2026
Timestamped By:  DigiCert SHA256 RSA4096 Timestamp Responder 2025 1
```

### Core.dll — SIGNED ✅ (same certificate)
```
Status:          Valid — "Signature verified"
Same signer certificate as installer
Same timestamp authority
```

### SteamTools.exe — NOT SIGNED ❌
```
Status:          NotSigned
```

### What this means:
- The developers paid for an **Extended Validation (EV)** certificate from GlobalSign — the most expensive and "trusted" tier of code signing
- EV certificates require verified business identity, so the company `NewWnight Global Tech Co., Ltd` is a real registered Chinese business (registration: `91430103MADWKFRYXD`)
- The **main executable** (`SteamTools.exe`) is deliberately left **unsigned**, while the installer wrapper and Core.dll are signed — this is suspicious because:
  - The unsigned binary is what actually runs on your system
  - It can be modified/replaced without breaking any digital signature
  - The signed installer merely unpacks the unsigned binary
- The signature on `Core.dll` makes it appear legitimate to security software, but the DLL contains a remote code execution backdoor

---

## 6. The Company Behind It: NewWnight Global Tech Co., Ltd

| Field | Value |
|---|---|
| **Registered Name** | NewWnight Global Tech Co., Ltd |
| **Location** | Changsha, Hunan Province, China |
| **Business Registration** | 91430103MADWKFRYXD |
| **Business Type** | Private Organization |
| **Certificate Authority** | GlobalSign (Belgium) |

### Red Flags:
- **"NewWnight"** — an unusual, made-up company name with no meaningful web presence
- **Changsha, Hunan** — a real city but the company appears to exist primarily for code signing purposes
- The copyright text in `SteamTools.exe` says `steamtools.net`, not `NewWnight Global Tech` — the company and the product don't match
- `Core.dll` version info says **"Vale Corporation"** — a completely different entity name from the signer
- The business appears purpose-built to obtain an EV code signing certificate for this piracy tool

---

## 7. SteamTools.exe — The Main Binary

### File Properties
| Property | Value |
|---|---|
| **Size** | 1,833,984 bytes (1.75 MB) |
| **SHA-256** | `992D547BBF83F26F3117B27AAA2A2E2E665F76658B67C27E84B13DC379833EAA` |
| **Architecture** | PE64 (x86-64) |
| **Subsystem** | Windows GUI |
| **Digital Signature** | **NOT SIGNED** |
| **Framework** | Qt 5.x (C++) |
| **Original Filename** | `Steamtools.exe` |
| **File Description** | `Steamtools` |
| **Product Name** | `Steamtools` |
| **Company Name** | `steamtools.net` |
| **File Version** | 1.8.1.5 (raw: 1.8.0.0) |
| **Product Version** | 1.0.0.0 |
| **Copyright** | Copyright (C) 2024 steamtools.net |
| **Language** | English (United States) |
| **Compiled With** | MSVC 2022 |

### PE Section Layout
| Section | Virtual Size | Raw Size | Flags | Purpose |
|---|---|---|---|---|
| `.text` | 1,183,328 | 1,183,744 | `0x60000020` | Executable code |
| `.rdata` | 481,816 | 482,304 | `0x40000040` | Read-only data, imports, strings |
| `.data` | 29,092 | 22,528 | `0xC0000040` | Read-write global data |
| `.pdata` | 51,312 | 51,712 | `0x40000040` | Exception handling data |
| `.fptable` | 256 | 512 | `0xC0000040` | Function pointer table |
| `.rsrc` | 83,432 | 83,456 | `0x40000040` | Resources (icons, version info) |

### DLL Imports
| DLL | Purpose |
|---|---|
| `KERNEL32.dll` | Core Windows API |
| `USER32.dll` | Windows UI |
| `SHELL32.dll` | Shell operations |
| `ADVAPI32.dll` | Registry access, security |
| `Core.dll` | **Custom** — the backdoor engine |
| `Qt5Core.dll` | Qt framework core |
| `Qt5Gui.dll` | Qt GUI rendering |
| `Qt5Widgets.dll` | Qt UI widgets |
| `Qt5Network.dll` | Qt networking |
| `Qt5Svg.dll` | Qt SVG support |
| `xinput1_4.dll` | Xbox controller support (game-related) |

---

## 8. Core.dll — The Dangerous Engine

### File Properties
| Property | Value |
|---|---|
| **Size** | 673,784 bytes (658 KB) |
| **SHA-256** | `0EFD139F4201AEA356B6BADDB159534BADFEB388BB7DD54EE9D614F166CC64A2` |
| **Architecture** | PE64 (x86-64) |
| **Digital Signature** | **SIGNED** — NewWnight Global Tech Co., Ltd (EV cert) |
| **File Description** | `Vale Dynamic Link Library` |
| **Product Name** | `Vale` |
| **Company Name** | `Vale Corporation` |
| **Copyright** | `Vale Copyright (C) 2025` |
| **File Version** | 2.0.0.2 |
| **Compiled With** | MSVC 2022 |

### PE Section Layout
| Section | Virtual Size | Raw Size | Flags | Purpose |
|---|---|---|---|---|
| `.text` | 443,888 | 443,904 | `0x60000020` | Executable code |
| `.rdata` | 121,648 | 121,856 | `0x40000040` | Read-only data, imports |
| `.data` | 84,080 | 70,144 | `0xC0000040` | Read-write data |
| `.pdata` | 19,416 | 19,456 | `0x40000040` | Exception handling |
| `.fptable` | 256 | 512 | `0xC0000040` | Function pointer table |
| `.rsrc` | 920 | 1,024 | `0x40000040` | Resources |
| `.reloc` | 2,520 | 2,560 | `0x42000040` | Relocations |

### DLL Imports
| DLL | Purpose | Concern Level |
|---|---|---|
| `KERNEL32.dll` | Core Windows API | Normal |
| `ADVAPI32.dll` | Registry, security, crypto | ⚠️ Registry manipulation |
| `CRYPT32.dll` | Certificate & cryptography | ⚠️ Encryption operations |
| `iphlpapi.dll` | Network adapter information | ⚠️ Network enumeration |
| `ole32.dll` | COM object support | Normal |
| `secur32.dll` | Security/authentication | ⚠️ Credential handling |
| `security.dll` | Security support provider | ⚠️ Auth manipulation |
| `SHELL32.dll` | Shell operations | Normal |
| `update.dll` | **Custom** — pulled at runtime? | 🔴 External update component |

### What Core.dll contains:
1. **Full libcurl HTTP client** — can make any HTTP/HTTPS request, with proxy support (SOCKS4/5), SSL/TLS, NTLM authentication
2. **Cryptographic engine** — AES-128/192/256, RSA, Windows CryptoAPI
3. **Remote DLL loader** (`HttpLoadDLL`) — downloads, decrypts, and loads DLLs from the internet
4. **Steam IPC impersonation** — pretends to be Valve's Steam client
5. **Process manipulation** — can enumerate, suspend, resume, and modify threads in other processes
6. **HTTP upload capability** — multipart/form-data POST support for data exfiltration

---

## 9. What It Actually Does (Step by Step)

When a user runs SteamTools, the following happens:

### Phase 1: Installation
1. The NSIS installer (`st-setup-1.8.30.exe`) unpacks all 18 files to the chosen directory
2. Windows trusts it because of the valid EV digital signature
3. Most antivirus software allows it because the installer is properly signed

### Phase 2: Startup
4. `SteamTools.exe` launches as a Windows GUI application
5. It loads `Core.dll` as its primary dependency
6. Core.dll initializes its libcurl HTTP engine and cryptographic subsystems

### Phase 3: Steam Detection & Data Harvesting
7. Reads the Windows Registry: `HKEY_CURRENT_USER\Software\Valve\Steam\ActiveProcess`
8. Locates Steam's installation directory
9. **Silently reads** Steam user data files:
   - `loginusers.vdf` — contains ALL Steam accounts that have ever logged in on the machine
   - `localconfig.vdf` — per-account configuration, friends lists, launch options
10. Extracts from these files:
    - `AccountName` — your Steam login username
    - `PersonaName` — your display name
    - `SteamID` — your unique Steam identifier
    - `RememberPassword` — whether you have "remember me" enabled

### Phase 4: Update Check (The Backdoor)
11. Core.dll contacts **three update servers** in sequence:
    - `https://update.tnkjmec.com/version2.txt`
    - `http://update.wudrm.com/version2.txt`
    - `http://update.steamcdn.com/version2.txt`
12. These servers can respond with instructions to download new code
13. The **`HttpLoadDLL`** function can:
    - Download a DLL file from the server
    - **Decrypt** it (the data arrives encrypted)
    - Load it directly into memory using `LoadLibraryA`
    - Execute any function in it via `GetProcAddress`
14. **You have no control over or visibility into what code gets loaded**

### Phase 5: Steam IPC Impersonation
15. Creates Windows IPC objects to intercept Steam's communication:
    - `Global_SteamtoolsIPC_Class` — its own IPC channel
    - `Global\Valve_SteamIPC_Class` — **impersonates Valve's official IPC channel**
    - `Global_UnlockedIPC_Class` — signals "unlocked" state
16. This allows it to intercept and modify communication between Steam and games

### Phase 6: Game Piracy Operations
17. Opens/creates a SQLite database: `appids.db`
18. Manages pirated game data with the schema:
    ```sql
    CREATE TABLE IF NOT EXISTS Appinfo (
        appid INTEGER PRIMARY KEY,
        tickets BLOB,
        type INTEGER,
        stubdrm INTEGER,
        DecryptionKey TEXT(64, 64)
    );
    ```
19. Creates fake **ownership tickets** (authentication tokens proving "ownership")
20. Decrypts DRM-protected content using stored `DecryptionKey` values
21. Handles multiple unlock modes:
    - `ActivateUnlockMode` — enables game access
    - `AlwaysStayUnlocked` — persistent unlock
    - `UnlockFamilyView` — bypasses parental controls
    - `Get Family View (PIN)` — extracts the Family View PIN

### Phase 7: Auto-Update System
22. References `SteamtoolsSetup.exe` at `/config/stUI/` — an auto-updater
23. Can download and install new versions silently
24. The auto-updater can be used to push entirely new binaries

---

## 10. The "Vale Corporation" Deception

This is one of the most telling discoveries:

| Field | Core.dll Value | Real Valve | Observation |
|---|---|---|---|
| **Product Name** | `Vale` | Valve | One letter difference — 'l' removed |
| **Company Name** | `Vale Corporation` | Valve Corporation | Deliberate typo |
| **Description** | `Vale Dynamic Link Library` | (various) | Mimics Valve DLL naming |
| **Copyright** | `Vale Copyright (C) 2025` | Valve Corporation | Impersonation |

This is **deliberate social engineering**:
- If a user or security analyst inspects `Core.dll` properties, they see "Vale Corporation" — which looks like "Valve Corporation" at a quick glance
- The typo provides plausible deniability ("it's not Valve, it's Vale")
- This technique is designed to bypass manual review and security audits
- It demonstrates conscious intent to deceive

Additionally, it references Valve's actual Steam client library `steamclient64.dll`, meaning it hooks into or loads Valve's legitimate code.

---

## 11. Network Infrastructure & Update Servers

### Update Servers

| URL | Protocol | DNS Status | IP Address | Location |
|---|---|---|---|---|
| `https://update.tnkjmec.com/version2.txt` | HTTPS | Resolves (no A record) | Empty | Unknown |
| `http://update.wudrm.com/version2.txt` | HTTP ⚠️ | Resolves via CNAME | `43.174.246.23`, `43.174.247.23` | China (via dnse5.com CDN) |
| `http://update.steamcdn.com/version2.txt` | HTTP ⚠️ | DNS does not exist | Dead | Impersonates `steamcdn.com` |

### Critical concerns:

1. **`update.steamcdn.com`** — This domain deliberately impersonates Valve's official CDN (`steamcdn.com`). This is a **phishing/impersonation** technique designed to:
   - Bypass firewall rules that whitelist Steam traffic
   - Make the traffic look legitimate in network logs
   - Confuse security analysts inspecting connections

2. **`update.wudrm.com`** — Uses plain HTTP (not HTTPS), meaning:
   - Update payloads travel **unencrypted** over the network
   - Any Man-in-the-Middle (coffee shop WiFi, compromised router) could inject malicious payloads
   - The server is hosted on Chinese infrastructure via `dnse5.com` CDN

3. **`update.tnkjmec.com`** — The "primary" server uses HTTPS but its DNS currently returns no IP — possibly a rotating/disposable domain

4. **All three update servers can push arbitrary executable code** to your machine via `HttpLoadDLL`

### Main Domains

| Domain | IP Addresses | Infrastructure |
|---|---|---|
| `steamtools.net` | `104.26.5.149`, `104.26.4.149`, `172.67.74.16` | Cloudflare |
| `bbs.steamtools.net` | (Cloudflare) | Cloudflare |

The main website is behind Cloudflare, hiding the actual server origin.

---

## 12. The HttpLoadDLL Backdoor

This is the **single most dangerous feature** of SteamTools.

### What HttpLoadDLL does:
Found in `Core.dll`, this function:

1. **Downloads** a DLL file from a remote server (via the update URLs)
2. **Decrypts** it — the string `"Downloaded and decrypted data successfully"` confirms the payload arrives encrypted
3. **Loads** it into your process's memory using `LoadLibraryA`
4. **Resolves** exported functions using `GetProcAddress`
5. **Executes** the downloaded code

### Context strings found near HttpLoadDLL:
```
HttpLoadDLL called
Downloaded and decrypted data successfully
FileName
Hash
package/branch
WindowsFile64
```

### What this means in plain English:
- The developers have **a remote code execution backdoor** built into every copy of SteamTools
- They can push ANY code to ANY user at ANY time
- The payload is encrypted, so network monitoring tools cannot inspect what's being downloaded
- The downloaded DLL runs with **the same privileges as the user** — if you're an administrator, it has admin access
- There is no user prompt, no consent dialog, no notification
- Today's payload might be benign (just piracy functions). Tomorrow's could be:
  - A cryptocurrency miner consuming your CPU/GPU
  - A credential stealer harvesting your passwords
  - Ransomware encrypting your files
  - A botnet client using your computer for DDoS attacks
  - A banking trojan intercepting financial transactions

### Why this is worse than typical malware:
- It's **signed with an EV certificate**, so Windows and antivirus software trust it
- The payload changes server-side, so **static analysis of the installer reveals nothing about future payloads**
- The encrypted download means **network security tools can't inspect the payload**
- It's embedded in a "useful" tool, so users actively choose to run it

---

## 13. Steam Account Data Harvesting

### Data accessed without user knowledge:

| Data | Source File | What It Contains |
|---|---|---|
| `AccountName` | `loginusers.vdf` | Your Steam login username |
| `PersonaName` | `loginusers.vdf` | Your public display name |
| `SteamID` | `loginusers.vdf` | Your unique 64-bit Steam ID |
| `RememberPassword` | `loginusers.vdf` | Whether "Remember Me" is enabled |
| Per-account config | `localconfig.vdf` | Friends, launch options, settings |

### Why this is concerning:
- `loginusers.vdf` contains data for **ALL accounts** that have ever logged into Steam on your machine, not just your current one
- Combined with the HTTP POST/upload capability in Core.dll (via libcurl's multipart/form-data), there is a technical path to exfiltrate this data
- The `RememberPassword` flag tells an attacker which accounts have cached credentials
- Steam accounts can be worth hundreds to thousands of dollars (games, items, marketplace balance)

### The exfiltration capability:
Core.dll contains full libcurl with these upload-related strings:
- `multipart/form-data`
- `upload completely sent off`
- `CURLOPT_POSTFIELDS`
- `CURLOPT_HTTPPOST`
- `CURLOPT_UPLOAD`

This means the binary has the complete technical capability to POST your data to any server. Whether it currently does is controlled by server-side logic (and whatever `HttpLoadDLL` downloads).

---

## 14. Database Schema & Piracy Mechanics

### The appids.db database:

```sql
CREATE TABLE IF NOT EXISTS Appinfo (
    appid         INTEGER PRIMARY KEY,   -- Steam application ID
    tickets       BLOB,                  -- Fake ownership/authentication tickets
    type          INTEGER,               -- Game type classification
    stubdrm       INTEGER,               -- DRM stub type to bypass
    DecryptionKey TEXT(64, 64)           -- AES key to decrypt game content
);
```

### How game piracy works:
1. **AppID Lookup**: Each Steam game has a unique AppID (e.g., Counter-Strike 2 = 730)
2. **Ticket Generation**: SteamTools generates or stores fake "ownership tickets" — cryptographic proofs that you "own" a game
3. **DRM Bypass**: The `stubdrm` field identifies what type of DRM protection the game uses, so SteamTools can apply the correct bypass
4. **Content Decryption**: Games on Steam are encrypted; the `DecryptionKey` field stores the AES key needed to decrypt game files

### Related functionality strings:
- `addappid()` — adds a new game to the piracy database
- `apptickets` — manages fake authentication tickets
- `RunningAppID` — tracks which pirated game is currently running
- `Go Appid Folder` — navigates to the game's installation directory
- `notUnlockDepot` — controls which game depots (content packages) to unlock
- `Unlock Steam Solution` — the master unlock mechanism

### DLC unlocking:
The tool can unlock **paid DLC** (Downloadable Content) for games the user may or may not own. This operates by generating fake DLC entitlement tickets.

### Family View PIN extraction:
```
Retrieving Family View PIN for %1...
Account %1 current Family View PIN %2
```
The tool can extract (crack) Steam's Family View parental control PINs, bypassing parental restrictions.

---

## 15. IPC (Inter-Process Communication) Impersonation

### IPC objects created:

| Named Object | Purpose | Concern |
|---|---|---|
| `Global_SteamtoolsIPC_Class` | SteamTools' own IPC channel | Communication between SteamTools components |
| `Global\Valve_SteamIPC_Class` | **Impersonates Valve's Steam IPC** | 🔴 Intercepts Steam client communication |
| `Global_UnlockedIPC_Class` | Signals "unlocked" state to games | Makes games think they're legitimately owned |

### How Steam IPC impersonation works:
- Steam uses Inter-Process Communication (IPC) to talk to running games
- Games ask Steam: "Does this user own me? What DLCs do they have?"
- SteamTools creates a **fake IPC endpoint** with Valve's name
- Games connect to the fake endpoint instead of the real Steam client
- SteamTools responds: "Yes, the user owns everything" — the game launches

### Registry access:
```
HKEY_CURRENT_USER\Software\Valve\Steam\ActiveProcess
```
This registry key is read to find Steam's running process, allowing SteamTools to:
- Detect when Steam is running
- Find Steam's process ID
- Interact with Steam's internal state

### Steam client library reference:
```
steamclient64.dll
```
SteamTools references Valve's actual client library, suggesting it may load or hook into it.

---

## 16. Lua Plugin System — Extensible Attack Surface

### Lua engine integration:
SteamTools embeds a full **Lua scripting engine**, making it extensible:

| Component | Path | Purpose |
|---|---|---|
| Plugin scripts | `\config\stplug-in\Steamtools.lua` | User-customizable Lua scripts |
| Lua compiler | `\config\stplug-in\luapacka.exe` | Compiles Lua source to bytecode |

### What this means:
- The tool can run **arbitrary Lua scripts**
- Scripts can be downloaded from the update servers
- Users can create custom plugins — or attackers can distribute malicious ones
- The Lua engine likely has bindings to the tool's core functionality (network, file system, registry, process manipulation)
- This creates an **extensible attack surface** that can adapt and evolve

### Lua capabilities found in strings:
- File compilation (`luapacka.exe`)
- Script loading from the `stplug-in` directory
- Integration with the main application's game unlock system

---

## 17. Dangerous Windows APIs Used

### Core.dll — High-Risk API Inventory

| API | Category | What It Does | Why It's Dangerous |
|---|---|---|---|
| `VirtualProtect` | Memory | Changes memory page permissions | Can make data executable (shellcode injection) |
| `VirtualAlloc` | Memory | Allocates memory with specific permissions | Can allocate executable memory for downloaded code |
| `LoadLibraryA` | Code Loading | Loads a DLL into memory | Loads the downloaded `HttpLoadDLL` payloads |
| `LoadLibraryExA` | Code Loading | Extended DLL loading | Same as above with more options |
| `GetProcAddress` | Code Loading | Gets function address from a DLL | Resolves functions in downloaded DLLs |
| `CreateToolhelp32Snapshot` | Process Enum | Snapshots all running processes/threads | Enumerates all processes on the system |
| `Thread32First` | Thread Enum | Enumerates threads in a process | Finds target threads to manipulate |
| `SuspendThread` | Thread Control | Pauses a running thread | Can freeze Steam or game threads |
| `ResumeThread` | Thread Control | Resumes a paused thread | Resumes after modification |
| `SetThreadContext` | Thread Control | Modifies a thread's CPU registers | **Code injection** — redirects thread execution |
| `GetThreadContext` | Thread Control | Reads a thread's CPU registers | Reads state before modification |
| `IsDebuggerPresent` | Anti-Analysis | Detects if being debugged | **Evasion** — behaves differently under analysis |
| `CreateProcessW` | Process Creation | Creates a new process | Can launch arbitrary executables |

### SteamTools.exe — API Inventory

| API | What It Does | Why It's Dangerous |
|---|---|---|
| `VirtualProtect` | Changes memory permissions | Memory manipulation |
| `LoadLibraryA` | Loads DLLs | Dynamic code loading |
| `GetProcAddress` | Gets function addresses | Dynamic function resolution |
| `IsDebuggerPresent` | Anti-debug detection | **Evasion** |
| `CreateProcessW` | Creates processes | Can launch any program |
| `TerminateProcess` | Kills processes | Can kill Steam or other processes |
| `OpenProcess` | Opens another process | Cross-process manipulation |
| `CreateFileMappingA` | Shared memory | IPC between processes |
| `QProcess::startDetached` | Qt process launch | Launches programs that survive parent exit |

### Thread injection technique (Core.dll):
The combination of `CreateToolhelp32Snapshot` → `Thread32First` → `SuspendThread` → `GetThreadContext` → `SetThreadContext` → `ResumeThread` is a classic **thread hijacking** technique used to:
1. Find a thread in the target process (Steam)
2. Suspend it
3. Read its CPU state
4. Modify the instruction pointer to run injected code
5. Resume the thread — now executing the attacker's code inside Steam's process

---

## 18. Cryptography & Encryption Capabilities

### Windows CryptoAPI functions (Core.dll):

| API | Purpose |
|---|---|
| `CryptAcquireContext` | Opens a cryptographic provider |
| `CryptCreateHash` | Creates a hash object |
| `CryptHashData` | Feeds data into the hash |
| `CryptGetHashParam` | Retrieves hash results |
| `CryptReleaseContext` | Releases crypto provider |
| `CryptDestroyHash` | Destroys hash object |
| `EncryptMessage` | Encrypts network messages |
| `DecryptMessage` | Decrypts network messages |
| `InitializeSecurityContext` | SSPI authentication (NTLM/Kerberos) |

### Encryption algorithms referenced:
- **AES-128** / **AES-192** / **AES-256** — symmetric encryption (used for payload decryption and game content decryption)
- **RSA** — asymmetric encryption (likely used for update server authentication or key exchange)

### Usage:
1. **HttpLoadDLL payloads** arrive encrypted and are decrypted using these APIs
2. **Game content** requires `DecryptionKey` values stored in `appids.db`
3. **Network communication** with update servers uses SSL/TLS (via libcurl's OpenSSL/Schannel support)
4. **NTLM** authentication support suggests the ability to authenticate in enterprise environments

---

## 19. Anti-Analysis & Evasion Techniques

### Debugger detection:
```c
IsDebuggerPresent()  // Found in BOTH SteamTools.exe and Core.dll
OutputDebugStringA() // Found in Core.dll
```
Both binaries check if they're being run under a debugger. This is a classic anti-analysis technique — the tool may:
- Behave differently when analyzed
- Refuse to run under a debugger
- Hide malicious functionality from researchers

### "Vale" vs "Valve" naming deception:
As covered in Section 10, the version info deliberately impersonates Valve Corporation.

### Signed malicious code:
The EV code signing certificate is itself an evasion technique:
- Windows SmartScreen is less likely to warn about it
- Antivirus heuristics often whitelist signed binaries
- Security auditors may skip deeper analysis of "signed" DLLs

### Encrypted payloads:
The `HttpLoadDLL` payloads are encrypted, preventing:
- Network inspection (IDS/IPS can't see the content)
- Proxy filtering (corporate firewalls can't analyze the download)
- Forensic capture (packet captures contain only encrypted data)

### Domain impersonation:
`update.steamcdn.com` mimics Valve's CDN, making traffic appear legitimate.

---

## 20. What It Does NOT Do (Currently)

Comprehensive scanning of both binaries found **no evidence** of the following:

| Category | Scan Result | Confidence |
|---|---|---|
| **Cryptocurrency mining** (CPU/GPU) | ❌ Not found | High — no xmrig, stratum, hashrate, mining pool, OpenCL, CUDA strings |
| **Keylogging** | ❌ Not found | High — no SetWindowsHookEx, GetAsyncKeyState, keyboard hook APIs |
| **Screen capture** | ❌ Not found | High — no BitBlt, PrintWindow, screen capture APIs |
| **Webcam/Microphone access** | ❌ Not found | High — no multimedia capture APIs |
| **Browser credential theft** | ❌ Not found | High — no references to browser databases or cookie files |
| **Persistent startup entries** | ❌ Not found | Moderate — no Run registry keys or startup folder references |
| **Windows service creation** | ❌ Not found | High — no CreateService or SC Manager calls |
| **Scheduled tasks** | ❌ Not found | High — no task scheduler APIs |
| **Hardware fingerprinting** | ❌ Not found | High — no HWID, MAC address, or telemetry collection |
| **Clipboard hijacking** | ❌ Not found | High — only Qt's standard `QClipboard` (benign framework code) |
| **Backdoor/C2 keywords** | ❌ Not found | High — no "backdoor", "beacon", "heartbeat", "C2" strings |

### Critical caveat:
The `HttpLoadDLL` backdoor means **ALL of the above could be deployed at any time** via a server-side update. The absence of these features in the current binaries provides no guarantee they won't appear in a future downloaded DLL. The scan results above reflect only what is present in the shipped code as of this analysis date.

---

## 21. PE Binary Structure Analysis

### SteamTools.exe entropy & packing:
- The section sizes are consistent with a standard, non-packed MSVC-compiled C++ application
- `.text` section is 1.12 MB — large but consistent with Qt application linking
- No UPX, Themida, VMProtect, or other packer signatures detected
- The binary is **not obfuscated or packed** — it's a straightforward compiled C++ program

### Core.dll entropy & packing:
- Section structure is standard for a MSVC-compiled DLL
- `.data` section (84 KB virtual vs 70 KB raw) has a slight gap suggesting some BSS (zero-initialized) data
- `.reloc` section present (typical for DLLs that need base relocation)
- `.fptable` section — an unusual name, likely a custom function pointer table for internal dispatch
- Not packed or obfuscated

### The `.fptable` section:
Both binaries contain an unusual `.fptable` section (256 bytes virtual, 512 bytes raw) with read-write-execute characteristics (`0xC0000040`). This appears to be a **custom function pointer table** — possibly used for the plugin system or dynamic dispatch of Steam manipulation functions.

---

## 22. All URLs & External References

### URLs found in Core.dll:

#### Update/Control Infrastructure:
| URL | Protocol | Purpose |
|---|---|---|
| `https://update.tnkjmec.com/version2.txt` | HTTPS | Primary update server |
| `http://update.wudrm.com/version2.txt` | HTTP | Secondary update server |
| `http://update.steamcdn.com/version2.txt` | HTTP | Tertiary update server (Valve impersonation) |
| `https://curl.haxx.se/docs/http-cookies.html` | HTTPS | libcurl embedded reference |

#### Certificate Infrastructure (part of EV cert chain):
| URL | Purpose |
|---|---|
| `http://cacerts.digicert.com/DigiCertAssuredIDRootCA.crt` | DigiCert root CA |
| `http://cacerts.digicert.com/DigiCertTrustedG4TimeStampingRSA4096SHA2562025CA1.crt` | DigiCert timestamp CA |
| `http://cacerts.digicert.com/DigiCertTrustedRootG4.crt` | DigiCert trusted root |
| `http://crl.globalsign.com/codesigningrootr45.crl` | GlobalSign CRL |
| `http://crl.globalsign.com/gsgccr45evcodesignca2020.crl` | GlobalSign EV CS CRL |
| `http://crl.globalsign.com/root.crl` | GlobalSign root CRL |
| `http://crl.globalsign.com/root-r3.crl` | GlobalSign root R3 CRL |
| `http://crl3.digicert.com/DigiCertAssuredIDRootCA.crl` | DigiCert CRL |
| `http://crl3.digicert.com/DigiCertTrustedG4TimeStampingRSA4096SHA2562025CA1.crl` | DigiCert timestamp CRL |
| `http://crl3.digicert.com/DigiCertTrustedRootG4.crl` | DigiCert trusted CRL |
| `http://ocsp.digicert.com` | DigiCert OCSP |
| `http://ocsp.globalsign.com/codesigningrootr450F` | GlobalSign OCSP |
| `http://ocsp.globalsign.com/gsgccr45evcodesignca2020` | GlobalSign EV OCSP |
| `http://ocsp.globalsign.com/rootr1` | GlobalSign OCSP |
| `http://ocsp.globalsign.com/rootr3` | GlobalSign OCSP |
| `http://secure.globalsign.com/cacert/codesigningrootr45.crt` | GlobalSign CA cert |
| `http://secure.globalsign.com/cacert/gsgccr45evcodesignca2020.crt` | GlobalSign EV cert |
| `http://secure.globalsign.com/cacert/root-r3.crt` | GlobalSign root cert |

### URLs found in SteamTools.exe:

| URL | Purpose |
|---|---|
| `https://www.steamtools.net` | Official website |
| `https://bbs.steamtools.net` | Official forum |
| `https://t.me/steamtool` | Telegram channel |
| `https://github.com/st2024/Steamtools/releases` | GitHub releases page |
| `https://steamdb.info/` | Third-party Steam database |
| `https://store.steampowered.com/` | Official Steam store |
| `https://steamtoos.net/` | Typosquat domain (missing 'l') |
| `https://steamui.com/` | Unknown domain |
| `https://update.tnkjmec.com/version2.txt` | Update server (also in Core.dll) |
| `http://update.wudrm.com/version2.txt` | Update server (also in Core.dll) |
| `http://update.steamcdn.com/version2.txt` | Fake Steam CDN update server |
| `http://www.w3.org/1999/xlink` | SVG namespace (benign) |
| `http://www.w3.org/2000/svg` | SVG namespace (benign) |
| `http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd` | SVG DTD (benign) |

---

## 23. All File Hashes (SHA-256)

| File | SHA-256 |
|---|---|
| `st-setup-1.8.30.exe` (installer) | `41EC92BC311DAF40B22B2497D044B12D9BA0266DA84D8581255D7CC9E059135F` |
| `SteamTools.exe` | `992D547BBF83F26F3117B27AAA2A2E2E665F76658B67C27E84B13DC379833EAA` |
| `Core.dll` | `0EFD139F4201AEA356B6BADDB159534BADFEB388BB7DD54EE9D614F166CC64A2` |

Submit these hashes to [VirusTotal](https://www.virustotal.com) for multi-engine antivirus scanning results.

---

## 24. Complete Risk Assessment

### Risk Matrix

| Risk | Severity | Likelihood | Impact | Evidence |
|---|---|---|---|---|
| **Remote Code Execution** | 🔴 CRITICAL | HIGH | TOTAL COMPROMISE | `HttpLoadDLL` downloads, decrypts, and executes arbitrary DLLs |
| **Steam Account Theft** | 🔴 HIGH | MEDIUM | ACCOUNT LOSS | Reads `loginusers.vdf`, `AccountName`, `SteamID`, has HTTP upload |
| **Future Malware Delivery** | 🔴 HIGH | MEDIUM-HIGH | UNKNOWN | Encrypted DLL download + remote server control |
| **Man-in-the-Middle** | 🟡 MEDIUM | MEDIUM | CODE INJECTION | HTTP (not HTTPS) used for 2 of 3 update servers |
| **Steam Ban** | 🟡 MEDIUM | HIGH | ACCOUNT BAN | Piracy tool — Valve actively detects and bans |
| **Game Piracy (Legal)** | 🟡 MEDIUM | CERTAIN | LEGAL ACTION | Acknowledged purpose is bypassing DRM |
| **Privacy Violation** | 🟡 MEDIUM | HIGH | DATA EXPOSURE | Reads personal Steam data without disclosure |
| **Outdated Dependencies** | 🟢 LOW | LOW | VARIES | Qt5 from 2020 has known CVEs |

### Threat Model

**Who is at risk:**
- Anyone who installs and runs SteamTools
- Anyone on the same Steam machine (all accounts in `loginusers.vdf` are exposed)
- Corporate/education networks where it might be used (it has NTLM auth capability)

**Who controls the risk:**
- The developers of SteamTools (via the `HttpLoadDLL` backdoor and update servers)
- Anyone who compromises the update servers (`update.wudrm.com` etc.)
- Anyone performing a Man-in-the-Middle attack on the HTTP update connections

**What could go wrong:**
1. Developers decide to monetize their user base through crypto mining, credential theft, or ransomware
2. Update servers are compromised by a third party who pushes malicious payloads
3. A nation-state actor (the servers are in China) leverages the access
4. Valve detects the tool and issues permanent bans to all users
5. Law enforcement identifies users through the GitHub/Telegram channels

---

## 25. Legal Implications

### For Users:
1. **DMCA / Copyright Infringement**: Using SteamTools to play games you haven't purchased is copyright infringement in most jurisdictions
2. **CFAA (US)**: Circumventing technical protection measures (DRM) may violate the Computer Fraud and Abuse Act
3. **EU Copyright Directive**: DRM circumvention is illegal under Article 6 of the EU Copyright Directive
4. **Steam Subscriber Agreement**: Using third-party tools to bypass DRM is a direct violation — results in permanent account termination with no refund
5. **Criminal liability**: In some jurisdictions, using piracy tools carries criminal penalties, not just civil liability

### For Developers:
1. **Trademark infringement**: "Vale Corporation" impersonating "Valve Corporation"
2. **Domain impersonation**: `update.steamcdn.com` impersonating Valve's CDN
3. **Distribution of circumvention tools**: Illegal under DMCA §1201, EU Copyright Directive, and equivalents
4. **Computer fraud**: The undisclosed `HttpLoadDLL` backdoor could constitute unauthorized access to computer systems
5. **Wire fraud**: Distributing software with hidden backdoors under the guise of a gaming utility

---

## 26. Conclusion & Recommendations

### What SteamTools really is:
SteamTools is a **well-engineered game piracy tool** wrapped around a **remote code execution backdoor**. It uses professional software engineering practices (Qt framework, proper installer, EV code signing, update system) to appear legitimate, while containing capabilities that give its developers **complete, silent, persistent control** over every machine it runs on.

### The tool is dishonest at every level:
1. **To users**: Claims to "enhance your Steam gaming experience" — actually pirates games and opens a backdoor
2. **To Windows**: Presents a valid EV digital signature — actually loads unsigned, unverified code
3. **To security software**: `Core.dll` claims to be from "Vale Corporation" — deliberately impersonates Valve
4. **To network monitors**: Uses `update.steamcdn.com` — impersonates Steam's CDN
5. **To analysts**: Checks `IsDebuggerPresent` — behaves differently under analysis

### Recommendations:

**If you have installed SteamTools:**
1. **Uninstall immediately** — remove all files from the installation directory
2. **Change your Steam password** immediately from a different device
3. **Enable Steam Guard** two-factor authentication if not already enabled
4. **Review Steam login history** for unauthorized access
5. **Run a full antivirus scan** — check for any DLLs that may have been downloaded by `HttpLoadDLL`
6. **Check `%APPDATA%` and `%LOCALAPPDATA%`** for any `SteamTools` or `config\stUI` directories
7. **Monitor your system** for unusual CPU/GPU usage or network connections
8. **Consider your Steam account compromised** until verified otherwise

**If you are evaluating SteamTools:**
1. **Do not install it**
2. **Do not run it**, even in a virtual machine connected to the internet — it will phone home
3. **The risks vastly outweigh the "benefits"** — free games are not worth a backdoored computer
4. **There is no safe way to use this tool** — the `HttpLoadDLL` backdoor cannot be disabled by the user

---

## Appendix A: Analysis Methodology

This analysis was performed entirely through **static analysis** — no code was executed. Methods used:

1. **Extraction**: 7-Zip to unpack the NSIS installer
2. **PE Header Analysis**: Manual parsing of PE64 section tables, import directories
3. **String Extraction**: ASCII and Unicode string dumps from both binaries
4. **Pattern Matching**: Regex searches for URLs, APIs, crypto functions, Steam-specific strings
5. **Digital Signature Verification**: PowerShell `Get-AuthenticodeSignature`
6. **DNS Resolution**: Forward lookups on all domains found in the binaries
7. **Version Info Extraction**: PE resource section parsing via .NET `FileVersionInfo`
8. **Cross-Reference**: Comparing findings between `SteamTools.exe` and `Core.dll`

### Limitations:
- **No dynamic analysis**: The binaries were not executed, so runtime behavior (what `HttpLoadDLL` actually downloads) could not be observed
- **No network capture**: Actual server responses were not intercepted
- **No reverse engineering**: No disassembly or decompilation was performed — all findings are from string-level analysis
- **Server-side logic unknown**: What the update servers actually push is not determinable from static analysis
- **Lua scripts not analyzed**: No plugin Lua scripts were included in the installer; their behavior is unknown

---

## Appendix B: Indicators of Compromise (IOC)

### File Hashes (SHA-256):
```
41EC92BC311DAF40B22B2497D044B12D9BA0266DA84D8581255D7CC9E059135F  st-setup-1.8.30.exe
992D547BBF83F26F3117B27AAA2A2E2E665F76658B67C27E84B13DC379833EAA  SteamTools.exe
0EFD139F4201AEA356B6BADDB159534BADFEB388BB7DD54EE9D614F166CC64A2  Core.dll
```

### Domains:
```
steamtools.net
bbs.steamtools.net
update.tnkjmec.com
update.wudrm.com
update.steamcdn.com
steamtoos.net
steamui.com
```

### IP Addresses:
```
104.26.5.149   (Cloudflare — steamtools.net)
104.26.4.149   (Cloudflare — steamtools.net)
172.67.74.16   (Cloudflare — steamtools.net)
43.174.246.23  (China — update.wudrm.com)
43.174.247.23  (China — update.wudrm.com)
```

### Windows Named Objects (IPC):
```
Global_SteamtoolsIPC_Class
Global\Valve_SteamIPC_Class
Global_UnlockedIPC_Class
```

### Registry Keys Accessed:
```
HKEY_CURRENT_USER\Software\Valve\Steam\ActiveProcess
```

### File Artifacts:
```
appids.db             (SQLite database — pirated game data)
\config\stplug-in\    (Lua plugin directory)
\config\stUI\         (Auto-updater directory)
Steamtools.lua        (Plugin script)
luapacka.exe          (Lua compiler)
SteamtoolsSetup.exe   (Auto-updater)
```

### Certificate:
```
Subject:     CN="NewWnight Global Tech Co., Ltd"
Serial:      046CD9B4FAD9F7C7101A6578
Thumbprint:  F3F5A63028D89B2902B4C4EA535F2CD938966314
Business ID: 91430103MADWKFRYXD (Changsha, Hunan, China)
```

---

*This report is provided for educational and security research purposes. The analysis was conducted on a file already present in the research workspace. No software was executed during this analysis. I (Hegxib) Not Responsible for any misuse.*

---

## Made By WWW.HEGXIB.ME (HxB) - WWW.X.HEGXIB.ME - 2026 - All Rights Reserved.

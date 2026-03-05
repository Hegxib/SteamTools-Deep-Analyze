# SteamTools `st-setup-1.8.30.exe` — Executive Summary
> Full detailed report: [SteamTools_Full_Analysis_Report.md](SteamTools_Full_Analysis_Report.md)  
> Analysis date: March 5, 2026 | Analyst: Independent Security Researcher

---

## What Is It?
**SteamTools** (`st-setup-1.8.30.exe`) is a Chinese-developed Steam game piracy tool.  
It allows users to play paid Steam games and DLCs **without purchasing them** by forging  
authentication tickets and manipulating Steam's internal processes.

---

## Key Facts at a Glance

| Property | Value |
|---|---|
| **File** | `st-setup-1.8.30.exe` |
| **Size** | ~10.6 MB (11,125,328 bytes) |
| **Type** | NSIS 3 Unicode 64-bit Installer |
| **Installer Signed By** | `NewWnight Global Tech Co., Ltd` — Changsha, Hunan, **China** |
| **Main EXE Signed?** | ❌ NO — `SteamTools.exe` is unsigned |
| **Built With** | Qt5 framework + libcurl |
| **Risk Level** | 🔴 CRITICAL |

---

## What It Does (Plain English)

1. **Cracks Steam games** — Stores forged game tickets + DRM decryption keys in a local SQLite database (`appids.db`) to unlock paid games without purchase
2. **Impersonates Valve** — Creates IPC channels named identically to Valve's own Steam client (`Global\Valve_SteamIPC_Class`) to intercept Steam communication
3. **Reads your Steam account data** — Parses `loginusers.vdf` and `localconfig.vdf` to extract account names, Steam IDs, and session tokens
4. **Remote code backdoor** — Function `HttpLoadDLL` downloads, decrypts, and executes arbitrary DLLs from Chinese update servers at any time without user knowledge
5. **Lua plugin system** — Loaded Lua scripts can execute any code the script author intended
6. **Bypasses parental controls** — `UnlockFamilyView` disables Steam's Family View restrictions
7. **Disables game updates** — Unlocked games are locked out of Steam content updates

---

## What It Does NOT Do (Currently)

| Activity | Status |
|---|---|
| Cryptocurrency mining | ✅ Not found |
| Keylogging | ✅ Not found |
| Screen/webcam capture | ✅ Not found |
| Browser credential theft | ✅ Not found |
| Persistence / startup entries | ✅ Not found |
| Ransomware behavior | ✅ Not found |

> ⚠️ **Critical caveat**: The `HttpLoadDLL` backdoor means ANY of the above could be  
> pushed silently to your machine by the developers at any time — without any update to  
> the installed files.

---

## The Single Biggest Risk

```
HttpLoadDLL → downloads encrypted DLL from Chinese server
            → decrypts it in memory
            → loads and executes it via LoadLibraryA
            → user sees nothing
            → can be a miner, stealer, ransomware — anything
```

This is a **permanent, silent, remote execution channel** that the developers  
control entirely. The tool being "clean today" means nothing.

---

## Deception Tactics Used

| Tactic | Detail |
|---|---|
| **Fake company name** | Core.dll claims to be made by "Vale Corporation" (not "Valve") |
| **Fake product name** | Core.dll calls itself "Vale Dynamic Link Library" |
| **Domain impersonation** | Update server `update.steamcdn.com` mimics Steam's CDN |
| **IPC impersonation** | Named objects mirror Valve's exact IPC naming convention |
| **EV cert on outer installer only** | Legitimate-looking signature hides unsigned main binary |

---

## Update / C2 Infrastructure

| Domain | IP | Location | Status |
|---|---|---|---|
| `update.wudrm.com` | `43.174.247.23` | **China** | 🟢 Active |
| `update.tnkjmec.com` | N/A | Unknown | 🔴 Dead |
| `update.steamcdn.com` | N/A | **Fake Steam domain** | 🔴 Dead |
| `steamtools.net` | `104.26.4.149` | Cloudflare | 🟢 Active |

---

## Legal Risk Summary

| Jurisdiction | Applicable Law | Potential Consequence |
|---|---|---|
| United States | DMCA, CFAA | Criminal charges, civil liability |
| European Union | EU Directive 2009/24/EC | Criminal prosecution |
| United Kingdom | Computer Misuse Act 1990 | Up to 10 years imprisonment |
| China (origin) | Copyright Law of PRC | Civil/criminal liability |
| **Steam Platform** | Steam Subscriber Agreement | **Permanent account ban** |

---

## Recommendations

### For Regular Users
- 🚫 **Do not install or run this software**
- 🗑️ **Delete it immediately** if already installed
- 🔑 **Change your Steam password** if it was ever run on your machine
- 🔍 **Run a full antivirus scan** — check for dropped DLLs in `%APPDATA%` and `%TEMP%`

### For IT / Security Teams
- 🛡️ Block all domains listed in the IOC section of the full report
- 🔒 Hash-block `SteamTools.exe` and `Core.dll` at endpoint level
- 📋 Monitor for named objects: `Global\Valve_SteamIPC_Class` created by non-Steam processes
- 🔎 Alert on `LoadLibraryA` calls following HTTP downloads

### For Developers / Researchers
- 📄 Full binary analysis, API inventory, database schema, and IOCs are in the detailed report
- 🔬 Dynamic analysis in a sandboxed VM is recommended for behavioral confirmation

---

## Indicators of Compromise (Quick Reference)

```
SHA256 (installer):  41EC92BC311DAF40B22B2497D044B12D9BA0266DA84D8581255D7CC9E059135F
Signer:              NewWnight Global Tech Co., Ltd
Signer Cert ID:      91430103MADWKFRYXD
Update server IP:    43.174.247.23
Registry key:        HKCU\Software\Valve\Steam\ActiveProcess (read)
Files dropped:       SteamTools.exe, Core.dll, appids.db
Named objects:       Global\Valve_SteamIPC_Class
                     Global\SteamtoolsIPC_Class
                     Global\UnlockedIPC_Class
```

---

> 📄 *This summary is a condensed version. For full binary-level analysis, API tables,*  
> *PE section breakdowns, and complete IOC lists, see the full report.*

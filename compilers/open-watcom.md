---
title: Open Watcom
description: Open-sourced Watcom C/C++ (16-bit DOS and 32-bit Windows)
published: true
date: 2026-08-20T13:42:59.000Z
tags: compiler, watcom
editor: markdown
dateCreated: 2026-08-20T13:42:59.000Z
---

# Open Watcom

Open Watcom is the open-sourced continuation of Watcom C/C++, one of the dominant DOS/Windows/OS/2-era compiler suites. Its 16-bit DOS and 32-bit Windows compilers both remain usable for matching decompilation, and it is the only compiler family on this site that ships **native Linux binaries** — no emulation layer needed.

## Versions

- **Watcom C/C++ (commercial)** — widely used for 1990s DOS games; the DOS release of *Doom* was famously compiled with Watcom C/C++, and the released Doom source still carries `WATCOM`-specific code.
- **Open Watcom 1.0 (2003)** — source released under the Sybase Open Watcom Public License.
- **Open Watcom 2.0 (current)** — maintained at [open-watcom/open-watcom-v2](https://github.com/open-watcom/open-watcom-v2); the version rebrew/decomp.me tooling uses.

## Running Open Watcom

Unlike every other compiler on this site, Open Watcom ships Linux binaries, so `wcc386` (32-bit) and `wcc` (16-bit DOS) run directly on the host — the [rebrew-toolchains](https://github.com/maci0/rebrew-toolchains) image `rebrew/watcom:2.0-win32` additionally packages the 32-bit compiler behind a `wcc386` wrapper for the docker-first workflow.

The compilers emit **OMF** objects: 32-bit OMF is converted to COFF (via `objconv`) and parsed transparently; 16-bit OMF is decoded by a dedicated OMF parser. Matching is byte-exact for both.

## Identifying a Watcom compiled binary

- **Object format** — Watcom objects are OMF in a dialect distinct from Borland's; `objconv`'s comp.id extracts the compiler version from the object.
- **Detect It Easy** carries Watcom signatures.
- **Runtime** — the DOS-extender-based 16-bit output and the win32 output have distinct CRT signatures (Watcom's own runtime, not msvcrt).

## Known quirks

- **`wcc` (16-bit) rejects `-c`** with E1073 — the 16-bit compiler does not take a separate compile-only flag; the `-fo=`/`-I`/`-zq` flag shape is used instead.
- **Moving snapshot tag** — the 2.0 "Last-CI-build" release tag is a moving target, so pinned toolchains re-download and re-hash it rather than trusting the tag.

## Where to get it

- [open-watcom/open-watcom-v2](https://github.com/open-watcom/open-watcom-v2) — source and CI snapshots.
- The [archaic-toolchains](https://github.com/archaic-toolchains) org mirrors the pinned snapshot.

## Projects

- [rebrew](https://github.com/maci0/rebrew) — `watcom` (wcc386, 32-bit) and `watcom16` (wcc, 16-bit DOS) profiles with byte-exact matching, incl. the chkstk call reloc slot.
- [decomp.me](https://decomp.me) — hosts watcom as a win32 compiler.

# Static surface baseline — 7zFM.exe

**Target:** `C:\Program Files\7-Zip\7zFM.exe`
**Version:** 26.02 · **Publisher:** Igor Pavlov
**SHA-256:** `ebc4f14a05deb7443a2ca3c8c379b6ae1b0f1a3fe193e00f51e10da6bb3adf3a`
**Captured:** 2026-11-17 · **Tooling:** GNU objdump 2.47 via REA `inspect_with_objdump`

## Verified section map

Image base `0x400000`. Six sections, `pei-x86-64`.

| # | Name | Virtual size | VMA | File offset | Align | Flags |
|---|---|---|---|---|---|---|
| 0 | `.text` | `0x0009D512` (644,370) | `0x401000` | `0x400` | 16 | CONTENTS, ALLOC, LOAD, READONLY, CODE |
| 1 | `.rdata` | `0x0003927E` (234,110) | `0x49F000` | `0x9DA00` | 16 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 2 | `.data` | `0x00000200` (512) | `0x4D9000` | `0xD6E00` | 16 | CONTENTS, ALLOC, LOAD, DATA |
| 3 | `.pdata` | `0x0000E3E8` (58,344) | `0x4DB000` | `0xD7000` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 4 | `.rsrc` | `0x0000CD60` (52,576) | `0x4EA000` | `0xE5400` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 5 | `.reloc` | `0x000017F4` (6,132) | `0x4F7000` | `0xF2200` | 4 | CONTENTS, LOAD, READONLY, DATA |

The post-read digest matched the pre-read digest (`artifact_changed: false`).

## Observations

- Code is concentrated in a single `.text` of ~629 KiB; there is no separate
  CRT or third-party library section.
- `.data` is only 512 bytes — this GUI has almost no static writable globals
  relative to its code size.
- `.pdata` (x64 unwind) is present and proportionally large, consistent with a
  C++ codebase using exceptions/SEH.
- No `.tls`, `.didat`, or debug section is present in the section table.

## Deductions

- Classic Win32 application image; the 0x400000 base is the conventional
  executable default and does not imply 32-bit.
- The near-empty `.data` suggests module state is heap-allocated or reached
  through pointers stored in `.rdata`/constructed at run time; allocation
  strategy is **unknown** without code analysis.
- `.pdata` volume is consistent with heavy C++ (exceptions), supporting the
  native C++ deduction already recorded in the README.

## Unknowns

- Import table contents, class layout, message dispatch and the exact
  `7zFM.exe` ↔ `7z.dll` interaction are not established by this baseline.
- Resource types inside `.rsrc` were not enumerated in this pass.

## Provenance

Sections and sizes come directly from `objdump -h` output returned by REA
(evidence `ev_d20347d1…a83`). No code was executed; the file was read only.

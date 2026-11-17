# re-7zip

An independent, clean-room reconstruction of the 7-Zip file manager and archive
engine, derived from static binary analysis of a locally installed build plus
public archive-format documentation.

> **Not affiliated with, endorsed by, or derived from the source code of Igor
> Pavlov or the 7-Zip project.** No third-party binaries, disassembly listings,
> extracted resources, or copied source are included in this repository.

## Target under analysis

| Field | Value |
|---|---|
| Application | 7-Zip File Manager |
| Version | 26.02 |
| Publisher | Igor Pavlov |
| Executable | `C:\Program Files\7-Zip\7zFM.exe` |
| Size | 997,888 bytes |
| Format | PE32+ (`pei-x86-64`), x86-64 |
| SHA-256 | `ebc4f14a05deb7443a2ca3c8c379b6ae1b0f1a3fe193e00f51e10da6bb3adf3a` |
| Companion modules | `7z.dll` (1,906,688 B), `7zG.exe` (723,456 B), `7z.exe`, `7-zip.dll`, `7-zip32.dll` |
| Baseline captured | 2026-11-16 |

Identity, version, size and PE architecture were verified from registry data,
file metadata and a static `objdump -f` header read. The digest was recorded by
the analysis tool before and after the header read, confirming the file was not
modified.

## Objective

Reconstruct the core user-facing capability of the product — open and browse
archives, extract entries, and create archives in at least one container /
compression pair — while documenting the architecture recovered from the binary.

Deliverables:

1. A readable architecture model: module boundaries, format-dispatch design,
   UI/engine separation, and shell-integration surface.
2. A working implementation of the subset above with automated tests.
3. Notes that let a third party reproduce the analysis without redistributing
   the original artifact.

## Non-goals

- Byte-identical behaviour or bug-for-bug compatibility.
- Full 7z/LZMA codec fidelity in early phases (ZIP/Deflate first; 7z deferred).
- Any redistribution of the analysed executable, its DLLs, or extracted data.

## Method and evidence policy

Static-first analysis: PE metadata, import/export and resource inspection,
Ghidra 12.1.4 headless import, function discovery, disassembly, decompilation,
string and cross-reference analysis. The target is **not executed** during
analysis phases; dynamic instrumentation (Frida) is used only if separately
authorised for a specific question.

Every claim in `docs/` is tagged:

- **Verified** — returned directly by a tool, reproducible from the recorded
  digest and tool version.
- **Deduction** — deterministic inference from verified facts.
- **Hypothesis** — plausible interpretation with named alternatives.
- **Unknown** — not established by available evidence.

## Observed module layout (deduction of roles)

| File | Observed | Role (deduction) |
|---|---|---|
| `7zFM.exe` | main GUI executable | file-manager front-end |
| `7zG.exe` | GUI helper executable | progress / operation UI |
| `7z.exe` | console executable | command-line driver |
| `7z.dll` | format/codec engine | archive format + codec providers |
| `7-zip.dll` | shell DLL | Windows Explorer integration |

The split is verified by file presence; the role assignment is a deduction from
names and the product's documented behaviour, to be confirmed by analysis.

## Repository layout

```
README.md          this file
LICENSE            Apache-2.0
ROADMAP.md         phased plan and exit criteria
docs/              evidence, architecture notes, unknowns
src/               implementation (added in Phase 2)
tests/             automated tests (added in Phase 2)
tools/             analysis helper scripts (no binaries)
```

## Status

Phase 0 — scope, methodology and verified evidence baseline. No code yet.

## Licence

Apache-2.0. See `LICENSE`. The licence covers original work in this repository
only; it grants no rights to the original 7-Zip software.

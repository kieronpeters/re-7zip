# Roadmap - re-7zip

## Phase 0 - Scope and baseline (complete)
Deliverables: README, licence, verified evidence baseline, ignore rules.
Exit criteria: target identity and digest recorded; scope and non-goals agreed.

## Phase 1 - Static architecture map
Import `7zFM.exe` and `7z.dll` into Ghidra; inventory imports, exports and
strings; identify the GUI-to-engine boundary and the format enumeration path.
Deliverable: `docs/architecture.md`.
Exit criteria: a documented call path for "open an archive" and a module map.

## Phase 2 - Format engine spike (read)
Implement ZIP central-directory parsing and extraction for store/deflate.
Deliverable: CLI that lists and extracts entries from self-generated archives.
Exit criteria: round-trip tests on archives created by this project.

## Phase 3 - Archive writer
Create ZIP archives; expose a UI-independent API; add tests.
Exit criteria: archives readable by at least one independent tool.

## Phase 4 - Front-end
Minimal GUI (framework chosen from evidence, not assumed): browse, extract,
create. Exit criteria: end-to-end user flow.

## Phase 5 - Shell integration (optional)
Explorer context-menu integration and drag-and-drop.

## Phase 6 - Hardening and documentation
Fuzzing of parsers, error-path tests, final write-up with open questions.

# Mystic BBS Changes and Updates

This folder contains standalone documentation describing how Mystic BBS changed across major versions and how those changes affected Mystic Programming Language (MPL) and embedded Python scripts (`.mpy`).

The reader should not need another wiki or release-history site to understand the documented changes. Each version file explains the relevant software behavior directly and includes upgrade considerations for SysOps, MPL developers, and MPY developers.

## Documentation Scope

Each version file is organized into the following areas:

1. **Release summary** — the overall direction of the release.
2. **Mystic software changes** — changes to the BBS, configuration tools, servers, message and file bases, editors, menus, and utilities.
3. **MPL changes** — compiler, syntax, runtime, variable, function, procedure, and integration changes.
4. **MPY and Python changes** — embedded Python engine, script execution, functions, dictionaries, and compatibility changes.
5. **Compatibility impact** — behavior that may break or alter existing configurations and scripts.
6. **Upgrade actions** — practical checks or migrations recommended when moving to that version.
7. **Documentation impact** — pages in this project that may require updates because of the release.

## Version Index

- [Mystic 1.05](Mystic-1.05.md)
- [Mystic 1.06](Mystic-1.06.md)
- [Mystic 1.07](Mystic-1.07.md)
- [Mystic 1.08](Mystic-1.08.md)
- [Mystic 1.09](Mystic-1.09.md)
- [Mystic 1.10](Mystic-1.10.md)
- [Mystic 1.11](Mystic-1.11.md)
- [Mystic 1.12](Mystic-1.12.md)

## Cross-Version Scripting Index

See [MPL and MPY Change Index](MPL-Change-Index.md) for a condensed scripting-language timeline.

## Terminology

### MPL

**MPL** means Mystic Programming Language, the Pascal-influenced language used to create Mystic scripts and modules.

### MPS

An `.mps` file is MPL source code.

### MPE

Older Mystic releases commonly used **MPE** to describe the execution engine and used `.mpe` for compiled scripts. Historical version files retain the term when discussing behavior from those releases.

### MPX

Mystic 1.10 changed the compiled MPL extension from `.mpe` to `.mpx`. Current documentation should use `.mpx` for compiled MPL programs unless describing an older release.

### MPY

An `.mpy` file is a Python script executed by Mystic's embedded Python engine. Embedded Python first appeared during the Mystic 1.12 development cycle.

### Python 2 and Python 3

Early MPY support used Python 2.7 syntax. Later Mystic 1.12 alpha builds replaced the original engine with one capable of loading Python 2 or Python 3 and added separate execution methods for Python 3.

## Change Categories

The version files use the following labels:

| Label | Meaning |
|---|---|
| **Added** | A new capability became available |
| **Changed** | Existing behavior or syntax was modified |
| **Fixed** | A defect or incorrect behavior was corrected |
| **Removed** | A capability, name, command, or compatibility path was deleted |
| **Migration required** | Existing scripts or configuration may require manual changes |
| **Recompile recommended** | Existing MPL source should be rebuilt with the target compiler |
| **Version-sensitive** | Behavior may differ among alpha builds or platforms |

## Documentation Policy

These files are written as original summaries for this project.

- Do not make the reader leave this repository to understand a change.
- Do not copy complete third-party release notes verbatim.
- Preserve exact identifiers when needed, including command names, variables, functions, extensions, and menu actions.
- Explain why a change matters to a SysOp or script developer.
- Separate software-wide changes from MPL and MPY changes.
- State when Python support did not yet exist.
- Identify breaking changes and required migrations clearly.
- Record the exact Mystic alpha build when behavior changed during the 1.12 development cycle.

## Adding a Version or Build

Use [Entry Template](ENTRY-TEMPLATE.md) when documenting another release or alpha build.

A new entry should answer:

- What changed in Mystic itself?
- What changed in MPL?
- What changed in MPY or the embedded Python engine?
- What existing configuration or source code could be affected?
- What should be tested after upgrading?
- Which documentation pages need revision?

## Verification Levels

| Status | Meaning |
|---|---|
| **Documented** | The change has been explained in this repository |
| **Compiler verified** | Related MPL source compiled with the recorded MPLC version |
| **Runtime verified** | Behavior was tested in Mystic |
| **Platform verified** | Behavior was confirmed on a named operating system and architecture |
| **Version-specific** | Behavior is confirmed only for a named Mystic build |
| **Needs testing** | The documented change has not yet been reproduced locally |

## Current Coverage

- Mystic 1.05 through 1.09 document the early MPE/MPL engine and major BBS changes.
- Mystic 1.10 documents the major modernization of MPL and the `.mpx` transition.
- Mystic 1.11 documents advanced record handling and additional date and timing support.
- Mystic 1.12 documents the long alpha cycle, embedded Python introduction, later Python 2/3 engine replacement, and continued MPL expansion.

This folder is intended to grow as individual alpha-build changes are researched, tested, and incorporated into the appropriate version file.

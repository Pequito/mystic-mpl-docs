# Mystic 1.09 Changes

Status: **Documented overview — detailed build extraction pending**

## Release Summary

Mystic 1.09 was a transitional release between the older 1.08 scripting environment and the major MPL modernization delivered in 1.10. It continued improving the BBS servers, message and file systems, configuration tools, platform support, and MPL integration while preparing for broader language and runtime changes.

## Mystic Software Changes

The 1.09 development cycle continued work in these areas:

- Internet server operation and connection handling
- Message-network and message-reader reliability
- File-base and archive processing
- Configuration and menu editing
- Linux and Windows behavior
- Prompt, display, and user-session handling
- Maintenance utilities and data processing

Individual alpha or maintenance-build behavior should be recorded under a dated subsection as it is tested.

## MPL Changes

Mystic 1.09 retained the older compiled MPE-era model and should be treated as a compatibility bridge rather than as equivalent to 1.10 syntax.

Important implications:

- Do not assume 1.10 block, declaration, array, or operator behavior exists in 1.09.
- Source intended for both 1.09 and 1.10 should be tested separately with each compiler.
- Existing `.mps` source should be preserved before migration to 1.10 because later compiler changes may require edits.
- Compiled programs from this period commonly used the older `.mpe` extension.

## MPY and Python Changes

**MPY was not available in Mystic 1.09.**

There was no embedded Python engine or `.mpy` script format. Python integration would have required an external process.

## Compatibility Impact

Mystic 1.09 should be documented as the final transitional stage before the 1.10 language redesign.

A migration from 1.09 to 1.10 requires special attention to:

- Compiled file extension changes
- Renamed built-in functions and procedures
- Control-flow syntax
- Mathematical expression behavior
- Procedure and function declarations
- Local variable behavior
- Arrays and record structures
- Removed compatibility identifiers

## Upgrade Actions

1. Back up all `.mps`, include, and compiled `.mpe` files.
2. Record the exact 1.09 build currently running.
3. Recompile source with the exact 1.09 compiler before migration to establish a known baseline.
4. Test the source separately with the target 1.10 compiler.
5. Do not overwrite working 1.09 compiled scripts until 1.10 runtime tests succeed.
6. Document every source edit required by the 1.10 transition.

## Documentation Impact

- `wiki/MPL-Syntax.md`
- `wiki/Compiler-Usage.md`
- `wiki/Version-Compatibility.md`
- `documents/mystic-changes/Mystic-1.10.md`

## Detailed Extraction Status

The release overview is self-contained, but the individual 1.09 build entries still need to be classified into:

- Mystic software changes
- MPL compiler changes
- MPL runtime changes
- Added or removed identifiers
- Platform-specific fixes
- Migration requirements

## Verification Record

```text
Mystic version/build: 1.09
Operating system:
Architecture:
MPLC version:
Compiled extension:
Baseline source compile result:
Runtime result:
1.10 migration comparison completed:
Notes:
```

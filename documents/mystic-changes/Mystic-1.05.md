# Mystic 1.05 Changes

Status: **Documented — needs local verification**

## Release Summary

Mystic 1.05a and 1.05b were maintenance releases focused on message and file scanning, QWK reliability, menu-key handling, editor behavior, and early MPE compiler and runtime corrections.

The scripting environment was still referred to primarily as MPE. MPL source used `.mps`, while compiled programs commonly used `.mpe`.

## Mystic Software Changes

### Mandatory Message and File Scanning

Message and file bases could be marked as mandatory in user scan settings. Users could no longer disable those bases from their personal scan configuration.

The `MN` message-new-scan command also gained a `/M` option for scanning only bases marked mandatory.

### Menu Input

Standard, non-lightbar menus gained additional hotkeys:

- `TAB`
- `RIGHT`
- `LEFT`
- `UP`
- `DOWN`

This allowed menu designers to respond to tab and arrow-key input without requiring a lightbar menu.

### QWK and Message Handling

Mystic corrected several QWK-related issues:

- User post totals were not updated when messages were posted through REP packets.
- A corrupted QWK index could cause malformed QWK packets.
- Message and file base configuration errors could corrupt or misrepresent scan behavior.

### Editors and User Management

The full-screen editor preserved the current quote-window position between uses.

The user editor gained easier record navigation and could jump directly to a user record number from its search function.

### Display and Prompt Behavior

Mystic restored page pausing when displaying long text files.

The `DF` MCI code in language prompts was changed so text following the code could continue displaying correctly.

A new `IL` MCI code returned the current node-invisibility state as `ON` or `OFF`.

## MPL and MPE Changes

### Menu Command Runtime Fix

Calling `MenuCmd` from an MPE program could disable arrow-key functions. Mystic 1.05a corrected that runtime problem.

### Large Source Compilation

MIDE's compile-progress display could become corrupted when an `.mps` file exceeded approximately 32,000 bytes. This was corrected.

### Undefined Variable Detection

MIDE and MPLC did not always report undefined variables in MPL source. Mystic 1.05b improved compiler validation so more undefined-variable errors were detected during compilation.

This matters because source that previously compiled could begin failing once the compiler correctly recognized an undeclared identifier.

### Developer Impact

Developers upgrading to 1.05b should:

1. Recompile existing `.mps` programs.
2. Correct any newly reported undefined variables.
3. Test scripts that call `MenuCmd` and then read arrow-key input.
4. Test large source files in both MIDE and MPLC.

## MPY and Python Changes

**MPY was not available in Mystic 1.05.**

Mystic did not yet include the later embedded Python engine or `.mpy` script format. Any Python use with this release would have required an external program or door.

## Compatibility Impact

- Mandatory scan settings may change what users can exclude from scans.
- New arrow and tab menu hotkeys may conflict with assumptions in older menu designs.
- More accurate undefined-variable checks may expose errors in source that appeared to compile previously.
- The release remains part of the older `.mpe` compiled-script era.

## Upgrade Actions

1. Back up message, file, user, and configuration data.
2. Review bases configured for mandatory scanning.
3. Test QWK packet creation and REP importing.
4. Recompile MPL programs and correct compiler errors.
5. Test menu hotkeys and any MPE program that calls `MenuCmd`.
6. Validate long text-file pausing and editor quote behavior.

## Documentation Impact

Review these project pages when documenting historical behavior:

- `wiki/Compiler-Usage.md`
- `wiki/Menu-Integration.md`
- `wiki/Variables.md`
- `wiki/Version-Compatibility.md`

## Verification Record

```text
Mystic version/build: 1.05a or 1.05b
Operating system:
Architecture:
MPLC version:
Date tested:
Undefined-variable test:
MenuCmd arrow-key test:
Large-source compile test:
QWK test:
Notes:
```

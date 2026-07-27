# Mystic 1.07 Changes

Status: **Documented — needs local verification**

## Release Summary

Mystic 1.07 and its maintenance releases significantly improved the MPE/MPL execution engine, expanded message and file group access, added more screen and text interfaces, and rewrote the primary MPL development tools and manual. The release also continued broad BBS work in message reading, menus, protocols, QWK, editors, Telnet configuration, and cross-platform support.

## Mystic Software Changes

### Message and File Areas

Mystic expanded message-reader behavior, new-message indicators, group handling, prompt information, and file-description capacity. New commands and prompt codes gave menus and display formats more information about users and messages.

### Menu and Configuration Tools

Menus gained an `AFTER` auto-execution type and explicit input-mode selection. Configuration and language editing were substantially improved across supported platforms.

### Development Tools

MIDE was rewritten with multiple open files, configurable tabs, improved block editing, optional syntax highlighting, and integrated help. The MPL manual was also rewritten and reorganized.

### Cross-Platform Reliability

Maintenance builds corrected Linux path and filename-case problems, Win32 door behavior, QWK portability, and OS/2 support. `PATHCHAR` was added so scripts could build paths correctly on DOS-like and Unix-like systems.

## MPL and MPE Changes

### Expression and Boolean Improvements

Declared functions could be used inside expressions. Boolean parameters became valid in procedures and functions, and Boolean modifiers such as `Not` could be used during assignment.

Constant support was completed so constants could be used in assignment contexts that were restricted in 1.06.

### Performance

The compiler and execution engine were reworked. Typical MPE execution was reported as several times faster than the previous release.

### New Functions and Interfaces

| Identifier | Type | Purpose |
|---|---|---|
| `Odd` | Function | Reports whether a number is odd |
| `GetFGroup` / `PutFGroup` | Procedures | Read and write file-group records |
| `GetMGroup` / `PutMGroup` | Procedures | Read and write message-group records |
| `GetTextStr` / `PutTextStr` | Procedures | Access text used by messages, file descriptions, and editors |
| `PathChar` | Function | Returns the platform-appropriate path separator |
| `GetScreenInfo` | Procedure | Reads screen-information MCI values |

New group variables exposed group names and access strings to MPL.

### Script Execution

MPE programs could be launched from SysOp macros by prefixing a macro definition with `!`. Script loading outside the normal scripts directory was corrected on Linux, and `ParamStr(0)` was fixed to return the full script path when appropriate.

### Type Checking

Maintenance builds corrected compiler mistakes involving function return types and numeric assignment compatibility. Source that relied on an incorrectly accepted type mismatch could fail after upgrading.

## MPY and Python Changes

**MPY was not available in Mystic 1.07.**

There was no embedded Python engine or `.mpy` format.

## Compatibility Impact

- Constants behave more completely than in 1.06.
- Function calls in expressions and Boolean parameters become valid.
- Faster execution can expose timing assumptions in scripts.
- Cross-platform scripts should use `PathChar` rather than hard-coded separators.
- Type checking became stricter in maintenance releases.
- Older protocol and prompt configurations may require review.

## Upgrade Actions

1. Recompile MPL source with the target 1.07 compiler.
2. Test Boolean parameters and declared functions used inside expressions.
3. Replace hard-coded path separators with `PathChar` where portability matters.
4. Test group record writes using backup data.
5. Test scripts launched from SysOp macros and scripts outside the default path.
6. Review any source that previously compiled despite mismatched types.

## Documentation Impact

- `wiki/Constants.md`
- `wiki/Functions.md`
- `wiki/Operators.md`
- `wiki/File-Handling.md`
- `wiki/Message-Bases.md`
- `wiki/File-Bases.md`
- `wiki/ANSI-and-Screen-Output.md`

## Verification Record

```text
Mystic version/build: 1.07, 1.07.2, or 1.07.3
Operating system:
MPLC version:
Expression-function test:
Boolean parameter test:
Constant assignment test:
PathChar test:
Group record test:
SysOp macro execution test:
Notes:
```

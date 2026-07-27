# Mystic 1.10 Changes

Status: **Expanded into detailed alpha-build documentation**

Mystic 1.10 was one of the largest compatibility and modernization releases in Mystic history. The development cycle covered sixty-three alpha builds before the final release on February 20, 2015.

The release changed Mystic software broadly and redesigned major parts of Mystic Programming Language. Because of the volume of changes, detailed documentation is divided into chronological phase files.

## Detailed Alpha History

- [Alpha 1 through Alpha 21](1.10/Alpha-01-21.md) — MPL parser redesign, Pascal-style syntax, file I/O, expressions, local declarations, arrays, records, functions, and `.mpx` migration foundations
- [Alpha 22 through Alpha 32](1.10/Alpha-22-32.md) — message bases, NodeSpy, ANSI, transfers, menus, file tagging, nodelists, QWK/QWKE, login scripting, MUTIL, FIDOPOLL, and mail statistics
- [Alpha 33 through Alpha 43](1.10/Alpha-33-43.md) — editor fixes, ARM Linux, themes, MPL classes, BinkP, QWK networks, AreaFix, FileFix, domains, and filtering
- [Alpha 44 through Alpha 63 and final release](1.10/Alpha-44-63-and-release.md) — network stabilization, FTP, BinkP, ICE colors, automatic banning, Linux fixes, group handling, node chat, viewers, events, prompt internals, and final release fixes

A directory-level overview is available at [Mystic 1.10 Detailed Change History](1.10/README.md).

## Major Mystic Software Changes

Mystic 1.10 advanced the software into a broader Internet-connected BBS platform. Major areas of development included:

- MIS server expansion
- BinkP, FTP, NNTP, telnet, and transfer improvements
- QWK and QWKE offline mail
- QWK networking
- Echomail and netmail import, export, routing, and 5D addressing
- MUTIL and FIDOPOLL
- AreaFix and FileFix
- New or revised menus, prompts, themes, editors, viewers, and node tools
- User-to-user chat and node-chat commands
- File tagging, archive viewing, and file-description display
- Event and semaphore automation
- Linux, Windows, ARM, and terminal compatibility work
- User, group, message-base, file-base, and configuration data changes

## Major MPL Changes

### Modern parser and syntax

MPL moved toward a more complete Pascal-style grammar:

```pascal
If IsReady Then
Begin
  WriteLn('Ready');
End;
```

`Then`, `Do`, and `Begin`/`End` replaced or superseded historical forms such as `EndIf`, `WEnd`, `FEnd`, and a single `ElseIf` token.

### Mathematical behavior

Standard operator precedence and parentheses were added:

```pascal
Result := 2 + 3 * 4;
Result := (2 + 3) * 4;
```

This could change the runtime result of older programs even when they compiled successfully.

### Variables and routines

MPL gained or expanded:

- Local variables
- Declaration initialization
- Nested procedures and functions
- Recursive procedures
- `Var` reference parameters
- Longer identifiers
- A much higher variable limit

### Strings, arrays, and records

The language gained or improved:

- Explicit string lengths such as `String[30]`
- Numeric character references such as `#32`
- Character indexing into strings
- Multidimensional arrays
- More capable record handling
- Arrays and records used in more complex structures

### Control flow

Modern control-flow features included:

- `Case`
- `Break`
- `Continue`
- Modern `Else If`
- Pascal-style `For`, `While`, and `Repeat` forms

### File I/O

The old specialized record functions were replaced by a broader `File`-based model with explicit open, read, write, close, position, size, and error behavior. `IoResult` became central to safe error handling.

### Functions, variables, and runtime integration

The release added or expanded many areas, including:

- Word and string parsing
- Date, Julian, Gregorian, and timer operations
- Path and filename functions
- Environment-variable access
- Directory and file tests
- Pipe-aware and raw terminal output
- Screen character and attribute inspection
- Bitwise operations and bit helpers
- Program name and parameter variables
- Configuration, message-base, file-base, user, and network variables
- Login integration through startup MPL
- MPL class interfaces for ANSI boxes, input, images, and screens

### Renamed and removed identifiers

Important migrations included:

| Older name | Newer name |
|---|---|
| `fExist` | `FileExist` |
| `fErase` | `FileErase` |
| `fCopy` | `FileCopy` |
| `CLS` | `ClrScr` |
| `USERLAST` | `UserLastOn` |
| `USERFIRST` | `UserFirstOn` |
| `FB...` variables | `FBASE...` naming |

`IsNoFile` was removed after `DispFile` returned a Boolean result.

### Compiled extension

Compiled programs changed from `.mpe` to `.mpx`:

```text
program.mps -> program.mpx
```

All menus, events, startup commands, deployment scripts, and documentation referencing `.mpe` required review.

## MPY and Python Changes

**MPY was not available in Mystic 1.10.**

The embedded Python engine and `.mpy` scripts appeared later during the Mystic 1.12 development cycle.

## Compatibility Impact

Mystic 1.10 should not be treated as a recompile-only upgrade. Existing source could be affected by:

- Parser and block syntax changes
- New expression evaluation order
- Renamed or removed identifiers
- Changed file I/O
- New local-scope behavior
- More accurate type checking
- Record and array changes
- Function-signature changes
- Different compiled output extension
- Corrected message, user, group, network, and configuration records

## Upgrade Actions

1. Back up the full Mystic installation and all MPL source.
2. Keep legacy `.mpe` files until replacement `.mpx` programs are tested.
3. Convert historical syntax to the modern parser rules.
4. Search source and includes for renamed functions and variables.
5. Review every arithmetic and Boolean expression.
6. Migrate old file I/O and add explicit error checks.
7. Recompile with the exact target MPLC build.
8. Update menus and commands to use `.mpx`.
9. Test every program under real terminal and node conditions.
10. Test message, file, QWK, echomail, netmail, BinkP, FTP, and event workflows used by the system.
11. Validate authentication scripts and avoid logging credentials.
12. Record the final verified compiler, platform, and Mystic build.

## Verification Record

```text
Mystic version/build: 1.10
Operating system:
Architecture:
MPLC version:
Source migration completed:
.mpx deployment tested:
Expression results compared:
File I/O tested:
Local and nested routines tested:
Var parameters tested:
Arrays and records tested:
Message and network integration tested:
Menus and events updated:
Notes:
```

## Documentation Impact

Mystic 1.10 affects nearly every language and integration page in this repository, especially:

- MPL syntax
- Variables and data types
- Procedures and functions
- Conditional logic and loops
- Operators
- Arrays and records
- Strings
- File handling
- Compiler usage
- Include files
- ANSI and screen output
- Menus and prompts
- User, message-base, and file-base access
- Network integration
- Version compatibility

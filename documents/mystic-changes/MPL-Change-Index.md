# MPL and MPY Cross-Version Change Index

This page summarizes the evolution of Mystic scripting across major releases. The version files contain the full software, MPL, and MPY explanations.

## Timeline

| Mystic version | MPL and scripting significance |
|---|---|
| 1.05 | Early MPE compiler and runtime fixes; stronger undefined-variable detection; menu and keyboard behavior corrected for MPE execution |
| 1.06 | Major language foundation: user procedures and functions, parameters, local variables, constants, additional built-ins, and improved MPE integration |
| 1.07 | Expression and Boolean improvements, much faster executor, new file/message group access, text-editing access, cross-platform path support, screen-information access, and rewritten MPL tools and manual |
| 1.08 | Continued MPL API growth, including user-time access and many Mystic integration additions |
| 1.09 | Transitional modernization leading toward the major 1.10 language and runtime changes |
| 1.10 | Major MPL redesign: Pascal-style language expansion, PEMDAS and parentheses, local declarations, nested and recursive routines, `Var` parameters, block comments, multidimensional arrays, new loop control, renamed built-ins, and `.mpx` compiled files |
| 1.11 | Advanced record support, records returned from functions, nested records and arrays, and additional date and timing capabilities |
| 1.12 | Long alpha cycle with major MPL expansion and embedded Python (`.mpy`) introduction; later Python 2/3 engine replacement and continued scripting API development |

## Compiled MPL Extension

### Before Mystic 1.10

Compiled Mystic programs commonly used the `.mpe` extension and the runtime was frequently described as the MPE engine.

### Mystic 1.10 and Later

The compiled extension changed to `.mpx`. Current documentation should use:

```text
source.mps -> program.mpx
```

Older `.mpe` terminology should be retained only when describing historical releases.

## MPL Language Development

### Procedures and Functions

Mystic 1.06 established much of the reusable-routine model, including declared procedures, functions, parameters, local variables, and constants.

Mystic 1.07 improved what those routines could do inside expressions and added Boolean parameter support.

Mystic 1.10 expanded routine behavior with nested definitions, recursion, `Var` reference parameters, and the ability to call a function while intentionally discarding its result.

Mystic 1.11 added records as function result types.

### Expressions and Operators

Early MPL behavior was more limited and did not consistently implement normal mathematical precedence.

Mystic 1.07 improved expression evaluation, including declared functions and Boolean modifiers.

Mystic 1.10 introduced standard precedence behavior, parentheses, exponent support, and broader bitwise operations. Documentation that says MPL always evaluates strictly left-to-right applies only to older engines.

### Variables, Arrays, and Records

Mystic 1.06 added or completed local variables and constants.

Mystic 1.10 added local declarations inside more blocks, declaration initialization, multidimensional arrays, and Pascal-style reference parameters.

Mystic 1.11 expanded record composition and allowed records to be returned by functions.

Mystic 1.12 continued extending record, array, and BBS-data interfaces throughout its alpha builds.

### Control Flow

Modern MPL uses Pascal-style control flow:

```pascal
If Condition Then
Begin
  Statement;
End;
```

Mystic 1.10 replaced several older control-flow forms and added or standardized features such as `Case`, `Break`, and `Continue`.

Pages describing `EndIf`, `FEnd`, `WEnd`, or a single `ElseIf` keyword should identify those forms as historical.

## MPL Runtime and Integration

MPL evolved from an early embedded MPE execution environment into a broader Mystic integration language capable of:

- Executing menu commands
- Reading and writing user, message-base, file-base, and group records
- Accessing screen and prompt information
- Working with text editors and message text
- Running from menus, command lines, and SysOp macros
- Handling platform-specific paths
- Reporting more detailed execution and compiler errors

Each version file identifies the specific interfaces added or changed.

## Renamed and Removed MPL Identifiers

Mystic 1.10 renamed several older built-ins to clearer names. Examples include:

| Older name | Newer name |
|---|---|
| `fExist` | `FileExist` |
| `fErase` | `FileErase` |
| `fCopy` | `FileCopy` |
| `CLS` | `ClrScr` |

Some older helper functions were removed because their result became part of another function. One example is the older `IsNoFile` pattern after `DispFile` gained a Boolean result.

Migration work should search source code for historical identifiers before recompilation.

## MPY and Embedded Python

### Before Mystic 1.12

Embedded Python and `.mpy` scripts were not part of Mystic 1.05 through 1.11. Any Python integration in those environments was external rather than the later built-in MPY runtime.

### Mystic 1.12

Mystic 1.12 introduced an embedded Python scripting system using `.mpy` files.

The first implementation was based on Python 2.7 and exposed Mystic data and operations through built-in Python functions and dictionaries.

Later 1.12 alpha development replaced the original engine with a new implementation that could load Python 2 or Python 3, depending on the installed library and execution method. Python 3 execution paths and compatibility work were added during this period.

Because 1.12 spans many alpha builds, MPY documentation must record the exact alpha version tested.

## Python 2 and Python 3 Compatibility

Python 2 and Python 3 differ in areas including:

- `print` statement versus `print()`
- Text and byte handling
- Integer-division behavior
- Module names
- Dictionary iteration behavior
- C-extension and shared-library compatibility

An MPY script written for the first Python 2.7 engine should not be assumed to run unchanged under a later Python 3 execution path.

## Required Documentation Review by Topic

| Topic | Versions most likely to affect it |
|---|---|
| Syntax and program structure | 1.06, 1.07, 1.10 |
| Variables and constants | 1.06, 1.07, 1.10 |
| Procedures and functions | 1.06, 1.07, 1.10, 1.11 |
| Conditional logic and loops | 1.10 |
| Arrays and records | 1.10, 1.11, 1.12 |
| File and path handling | 1.07, 1.10, 1.12 |
| Mystic record access | 1.07 through 1.12 |
| Compiler and execution behavior | Every version |
| MPY/Python | 1.12 only |

## Migration Checklist

When upgrading an MPL or MPY installation:

1. Identify the source version and target Mystic build.
2. Preserve original `.mps`, include, and `.mpy` source files.
3. Search for renamed or removed functions and procedures.
4. Review syntax changes, especially when moving to or beyond 1.10.
5. Recompile all MPL source with the target MPLC.
6. Test record-writing operations with backup data.
7. Confirm path handling on the target operating system.
8. For MPY, record whether the script runs under Python 2 or Python 3.
9. Test scripts from the same menu, command line, or event context used in production.
10. Update the applicable version file with the verified result.

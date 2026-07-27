# Mystic 1.10 Changes

Status: **Documented — compiler verification recommended**

## Release Summary

Mystic 1.10 was a major MPL modernization release. It changed the compiled script extension, expanded Pascal-style syntax, corrected expression behavior, added local and nested declarations, improved arrays and records, and renamed or removed several older built-ins. It also continued substantial work across Mystic's servers, menus, prompts, message and file systems, and platform support.

## Mystic Software Changes

Mystic 1.10 continued the transition toward a modern Internet BBS platform. The release cycle included server, configuration, message-network, file-processing, display, and runtime changes. SysOps should treat the version as both a software upgrade and an MPL migration point.

Execution errors from MPL programs became easier to notice because Mystic briefly delayed after displaying an MPL execution error rather than immediately clearing or replacing it.

## Compiled MPL Extension

The compiled script extension changed from `.mpe` to `.mpx`.

```text
program.mps -> program.mpx
```

Menus, events, macros, command lines, deployment scripts, and documentation that explicitly referenced `.mpe` files required updating.

## MPL Language Changes

### Mathematical Expressions

MPL gained standard operator precedence and parentheses. Older documentation that says MPL always performs calculations strictly from left to right applies to earlier engines.

```pascal
Result := 2 + 3 * 4;
Result := (2 + 3) * 4;
```

These expressions now produce different results according to normal mathematical grouping.

### Local Declarations

Variables could be declared in local routines and blocks with more complete behavior than older engines.

```pascal
Procedure DisplayName(Name String);
Var
  LabelText : String;
Begin
  LabelText := 'User: ' + Name;
  WriteLn(LabelText);
End;
```

Declaration initialization was also expanded:

```pascal
Var Count : Integer = 0;
```

### Nested Routines

Procedures and functions could be declared inside another procedure or function. This allowed helper routines to remain private to the outer routine.

### Recursive Procedures

Procedures could call themselves. Every recursive routine requires a terminating condition.

### `Var` Parameters

Pascal-style reference parameters allowed a routine to modify the caller's variable.

```pascal
Procedure ResetCount(Var Count : Integer);
Begin
  Count := 0;
End;
```

### Block Comments

Pascal-style block comments were added:

```pascal
(*
  Multi-line comment
*)
```

### Arrays

MPL added broader multidimensional-array support, including arrays with up to three dimensions in documented modern usage.

```pascal
Var Grid : Array[1..80, 1..25] of Char;
```

### Control Flow

Modern Pascal-style control flow replaced or superseded several historical forms. Documentation should use:

```pascal
If Condition Then
Begin
  Statement;
End;
```

Rather than treating older keywords such as `EndIf`, `FEnd`, `WEnd`, or a single `ElseIf` token as current syntax.

`Break` and `Continue` became available for loop control.

### Function Results May Be Ignored

A function could be called for its side effect without assigning or otherwise using its return value.

This is useful for input or display functions, but a result containing success, failure, or user input should not be discarded accidentally.

## Renamed and Removed Identifiers

Several built-ins were renamed for clarity.

| Older identifier | Newer identifier |
|---|---|
| `fExist` | `FileExist` |
| `fErase` | `FileErase` |
| `fCopy` | `FileCopy` |
| `CLS` | `ClrScr` |

The older `IsNoFile` pattern was removed after `DispFile` gained a Boolean result that could directly indicate whether a file was displayed.

Source migration should search for every older name before recompilation.

## MPL Runtime and Compiler Impact

Mystic 1.10 should not be treated as a simple recompile-only upgrade for all source. Existing code may need edits because of:

- Renamed and removed identifiers
- Changed expression evaluation
- Modernized control-flow syntax
- More accurate type checking
- New local-scope behavior
- Changed compiled extension
- Expanded array and record rules

Tests should compare actual runtime output, not only successful compilation.

## MPY and Python Changes

**MPY was not available in Mystic 1.10.**

The embedded Python engine and `.mpy` format appeared later in the 1.12 development cycle.

## Compatibility Impact

This is one of the most important migration boundaries in Mystic scripting history.

A script may compile but behave differently because operator precedence changed. Parentheses should be added where the intended order must be explicit.

Deployment automation and menu commands must use `.mpx` instead of `.mpe`.

## Upgrade Actions

1. Back up all MPL source, includes, and compiled `.mpe` files.
2. Search for renamed and removed built-ins.
3. Review every nontrivial mathematical and Boolean expression.
4. Convert historical control-flow syntax to the modern Pascal-style form.
5. Recompile with the target 1.10 MPLC.
6. Rename or redeploy compiled output as `.mpx`.
7. Test procedures, functions, local variables, arrays, and record access.
8. Update menus, macros, and command-line execution paths.

## Documentation Impact

- `wiki/MPL-Syntax.md`
- `wiki/Variables.md`
- `wiki/Procedures.md`
- `wiki/Functions.md`
- `wiki/Conditional-Logic.md`
- `wiki/Loops.md`
- `wiki/Compiler-Usage.md`
- `wiki/Version-Compatibility.md`

## Verification Record

```text
Mystic version/build: 1.10
Operating system:
Architecture:
MPLC version:
.mpx deployment test:
Operator precedence test:
Local variable test:
Var parameter test:
Nested routine test:
Recursive procedure test:
Array test:
Renamed identifier migration test:
Notes:
```

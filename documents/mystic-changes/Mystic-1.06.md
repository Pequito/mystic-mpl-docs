# Mystic 1.06 Changes

Status: **Documented — needs local verification**

## Release Summary

Mystic 1.06 was a major early scripting release. It expanded the MPE/MPL language from mostly linear scripts into a language with reusable procedures, parameters, local variables, constants, and functions. It also introduced a beta Win32 Telnet server and many BBS, menu, prompt, and message-management changes.

## Mystic Software Changes

### Telnet and Platform Support

Mystic added a beta native Win32 Telnet server. It allowed incoming Telnet sessions without a virtual modem driver, although DOS doors and external protocol drivers were not expected to work through that early server implementation.

Win32 console titles were updated to include the active node number. Several Win32 and OS/2 MPE execution defects were corrected.

### Message and File Operations

Message-posting menu commands gained parameters for setting recipient, subject, network address, and forced posting. File-list creation gained options for new files and all groups.

Mystic also added or changed prompts for message searching, message export, password errors, mass e-mail, blind uploads, and other workflows.

### User Records

Mystic introduced a permanent user index number. This index was exposed through ACS and MPL so scripts could identify a user independently of a movable database record position.

## MPL and MPE Changes

### Procedure Syntax Changed

The older `Proc` form was replaced by `Procedure`, and the procedure name was no longer required after `Pend`.

Older form:

```pascal
Proc Hello
  WriteLn('Hello')
Pend Hello
```

New 1.06 form:

```pascal
Procedure Hello
  WriteLn('Hello')
Pend
```

This was a migration-required change. Older source needed editing. Leaving a name after `Pend` could compile but execute incorrectly.

### Procedure Parameters

Procedures could receive typed values:

```pascal
Procedure PrintText(Text String)
  WriteLn(Text)
Pend
```

### Local Procedure Variables

Variables declared inside a procedure became local to that procedure. Program-level declarations remained global.

### Functions Added

User-defined functions were added. Functions returned a typed value by assigning to their own name.

```pascal
Function ReturnTen : Byte
Begin
  ReturnTen := 10
Pend
```

### Constants Added

`Const` declarations were added, but early support was limited. Constants could participate in expressions but were not yet valid in every assignment context.

### Variable and Array Handling

Variable handling was rewritten. Array indexes could use full expressions rather than only simple literal or variable indexes.

The `Var` declaration section also accepted comma-separated definitions in the syntax used at that time.

### `Begin` and `End`

The compiler ignored `Begin` and `End` so source could look more Pascal-like, but those words were not yet true block delimiters in the modern sense. Historical 1.06 code must not be interpreted using current MPL block semantics without testing.

### Output Type Enforcement

`Write` and `WriteLn` began requiring string values. Numeric values had to be converted with the available integer-to-string function before display.

### Added, Changed, and Removed Identifiers

| Identifier | Type | Change |
|---|---|---|
| `IsLocalKey` | Function | Added; reports whether the last key came from the local console |
| `WriteLocalXY` | Procedure | Added; writes only to the local display |
| `StatusWrite` | Procedure | Removed; replaced by `WriteLocalXY` |
| `MoveY` | Procedure | Added; vertical equivalent of `MoveX` |
| `OneKey` | Command | Added |
| `CFGSTATUSTYPE` | Configuration variable | Added |
| `USERPERMIDX` | User variable | Added permanent user index access |

### Runtime Fixes

The `MenuCmd` arrow-key problem was corrected again after being accidentally reintroduced. Cross-platform MPE engine problems in Win32 and OS/2 were also addressed.

## MPY and Python Changes

**MPY was not available in Mystic 1.06.**

The embedded Python engine and `.mpy` format did not exist in this release.

## Compatibility Impact

- Older procedures using `Proc` require conversion to `Procedure`.
- Procedure names after `Pend` should be removed.
- Numeric output must be converted to a string.
- `StatusWrite` calls must be replaced.
- Constants have historical limitations in this version.
- `Begin` and `End` do not yet have all modern block semantics.

## Upgrade Actions

1. Search all source for `Proc`, named `Pend` statements, and `StatusWrite`.
2. Update procedure declarations and calls.
3. Convert numeric output arguments to strings.
4. Recompile all scripts with the 1.06 compiler.
5. Test procedure parameters, local variables, constants, and functions.
6. Test MPE execution on the target DOS, Win32, OS/2, or Linux platform.

## Documentation Impact

- `wiki/Variables.md`
- `wiki/Procedures.md`
- `wiki/Functions.md`
- `wiki/Compiler-Usage.md`
- `wiki/Version-Compatibility.md`

## Verification Record

```text
Mystic version/build: 1.06
Operating system:
MPLC version:
Procedure migration test:
Local variable test:
Function return test:
Constant test:
Numeric WriteLn test:
Runtime result:
Notes:
```

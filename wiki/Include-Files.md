# Include Files and Compiler Directives

Include files let an MPL program reuse declarations, constants, types, variables, procedures, and functions stored in another source file.

Modern MPL treats `Include` as a language keyword. Older Mystic releases used directive-like include syntax, so include handling is an important compatibility boundary when maintaining historical MPL source.

> **Documentation status**
>
> Modern `Include filename` syntax is established in the repository's Mystic 1.10 history. Mystic 1.12 later improved include-library handling and source-relative include lookup. This page does not assume that general Free Pascal compiler directives such as conditional-compilation directives are supported by MPL unless they are separately verified with the target MPLC build.

## Why Use Include Files?

As an MPL project grows, common code can be separated from the main program.

Instead of placing every procedure in one file:

```text
main.mps
```

a project can use shared source:

```text
project/
├── main.mps
└── include/
    ├── constants.mps
    ├── types.mps
    └── common.mps
```

The main program includes the shared source during compilation.

Benefits include:

- Reusing procedures and functions
- Keeping large programs organized
- Sharing constants and custom types
- Centralizing common record declarations
- Reducing duplicate source
- Making version-specific code easier to isolate

## Modern Include Syntax

Later Mystic 1.10 builds standardized include handling as its own MPL keyword:

```pascal
Include common.mps
```

A relative path can also be used when supported by the compiler:

```pascal
Include include/common.mps
```

The included source becomes part of the program being compiled.

## Basic Include Example

Shared file:

```text
common.mps
```

Contents:

```pascal
Procedure ShowTitle;
Begin
  WriteLn('Mystic MPL Example');
End;
```

Main program:

```pascal
Include common.mps

Begin
  ShowTitle;
End.
```

The procedure declared in `common.mps` is available to the main source after the include is processed.

## Include Order Matters

An included declaration must be available before source that depends on it.

Example:

```pascal
Include include/types.mps
Include include/common.mps

Begin
  RunApplication;
End.
```

If `common.mps` uses a type defined in `types.mps`, the type file should be included first.

A useful order is:

```text
Constants
    ↓
Types
    ↓
Shared variables
    ↓
Procedures and functions
    ↓
Main executable code
```

This follows the same general organization described on [Program Structure](Program-Structure).

## Include File Example with Constants and Types

`include/common.mps`:

```pascal
Const
  MinimumLevel = 20;

Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;

Function HasAccess(UserInfo : TUserInfo) : Boolean;
Begin
  HasAccess := UserInfo.Level >= MinimumLevel;
End;
```

Main source:

```pascal
Include include/common.mps

Var
  UserInfo : TUserInfo;

Begin
  UserInfo.Name := 'Example User';
  UserInfo.Level := 25;

  If HasAccess(UserInfo) Then
    WriteLn('Access granted');
End.
```

The include supplies the constant, record type, and function.

## Include-Only Libraries

Mystic 1.12 Alpha 18 added a useful source marker for files intended to be include libraries.

If the first word in the source file is:

```pascal
Library
```

MIDE and MPLC can treat the source as an include library rather than compiling it as a standalone program during bulk compilation.

Example include library:

```pascal
Library

Const
  AppName = 'Example MPL';

Procedure ShowAppName;
Begin
  WriteLn(AppName);
End;
```

Main program:

```pascal
Include include/common.mps

Begin
  ShowAppName;
End.
```

This is especially useful when commands such as bulk or recursive compilation scan every `.mps` file in a project tree.

## Why `Library` Matters

Without an include-only marker, a bulk compiler operation may encounter:

```text
include/common.mps
```

and attempt to treat it as an independent program.

A library source often contains declarations and routines but no meaningful standalone execution path.

Using:

```pascal
Library
```

at the start of supported include-only source tells compatible MPLC/MIDE versions that the file is intended to be included rather than independently compiled.

`Library` is a Mystic 1.12-era feature. Do not assume it works on Mystic 1.10 or 1.11.

## Historical Include Syntax

Include syntax changed several times as MPL evolved.

### Older MPE Form

Earlier Mystic/MPE development used a preprocessor-like form:

```text
#include common.mps
```

This belongs to older source and should not be used as the default form for modern MPL.

### Early Mystic 1.10 Form

During the 1.10 redesign, include handling used a directive-like Pascal form:

```text
{$include common.mps}
```

This was transitional syntax.

### Later Mystic 1.10 Form

Mystic then changed include handling to a dedicated MPL keyword:

```pascal
Include common.mps
```

For modern source in this documentation project, use the direct `Include` form unless the target MPLC build requires another syntax.

## Migration Example

Historical source:

```text
{$include common.mps}
```

Modernized source:

```pascal
Include common.mps
```

When migrating a project, search all `.mps` and include files for historical forms rather than fixing only the main program.

Useful search targets include:

```text
#include
{$include
Include
```

## Compiler Directives Versus MPL Keywords

The historical syntax:

```text
{$include common.mps}
```

looked like a Pascal compiler directive.

Later Mystic 1.10 explicitly moved include handling away from a compiler-directive form and made `Include` its own MPL keyword.

That distinction matters.

Modern form:

```pascal
Include common.mps
```

should be treated as MPL syntax, not as a generic Pascal preprocessor directive.

## Do Not Assume Free Pascal Directives

MPL is Pascal-like, but it is not Free Pascal.

Do not automatically add directives such as:

```text
{$IFDEF ...}
{$DEFINE ...}
{$MODE ...}
{$I ...}
{$R ...}
```

to MPL source merely because they are valid in another Pascal compiler.

Unless a directive has been verified in the target MPLC build, treat it as unsupported.

This wiki should document only directives or compiler-control syntax that have been confirmed for Mystic MPL.

## Include Search Paths

The compiler must locate every included source file.

Possible factors include:

- The main source file directory
- The current working directory
- A relative path in the `Include` statement
- Compiler-version-specific search behavior
- Filename capitalization
- Operating-system path rules

Example:

```text
project/
├── main.mps
└── include/
    └── common.mps
```

Main source:

```pascal
Include include/common.mps
```

Using a path relative to the source tree makes the relationship clear.

## Mystic 1.12 Source-Relative Include Lookup

Mystic 1.12 Alpha 21 improved include lookup.

When an include filename did not contain its own directory, MPLC could search relative to the source file's directory in supported cases.

That allows layouts such as:

```text
project/
├── main.mps
└── common.mps
```

with:

```pascal
Include common.mps
```

This behavior should still be tested with the exact MPLC build used for deployment.

Do not assume every older compiler searches in the same order.

## Explicit Relative Paths

For larger projects, an explicit relative path often makes intent clearer:

```pascal
Include include/common.mps
```

This is generally easier to understand than depending on the compiler's current working directory.

For deeper organization:

```text
project/
├── main.mps
└── include/
    ├── core/
    │   └── common.mps
    └── data/
        └── records.mps
```

the source may use forms such as:

```pascal
Include include/core/common.mps
Include include/data/records.mps
```

Verify path separators and path resolution on the target operating system.

## Linux Filename Case

On Linux and other case-sensitive filesystems, these may be different files:

```text
common.mps
Common.mps
COMMON.MPS
```

Keep include filenames consistent.

For example, if the file is:

```text
include/common.mps
```

use:

```pascal
Include include/common.mps
```

rather than relying on case-insensitive behavior from another operating system.

## Include File Extensions

Historical and modern examples commonly use `.mps` for included MPL source:

```pascal
Include common.mps
```

This matches the normal MPL source extension.

Do not assume a custom extension such as `.inc` is accepted by every MPLC build unless tested.

A project may choose descriptive filenames, but the compiler's accepted filename behavior should be verified before standardizing another extension.

## Shared Project Layout

A practical layout is:

```text
mpl-project/
├── main.mps
├── include/
│   ├── constants.mps
│   ├── types.mps
│   ├── strings.mps
│   └── common.mps
└── build/
```

The main source might contain:

```pascal
Include include/constants.mps
Include include/types.mps
Include include/strings.mps
Include include/common.mps

Begin
  RunApplication;
End.
```

With Mystic 1.12-compatible include libraries, each shared source file can begin with:

```pascal
Library
```

when the file should be skipped as a standalone target during bulk compilation.

## Avoid Circular Includes

Do not design:

```text
a.mps includes b.mps
b.mps includes a.mps
```

Conceptually:

```text
a.mps
  ↓
b.mps
  ↓
a.mps
  ↓
...
```

Whether MPLC detects this cleanly or eventually fails is compiler-specific.

A safer dependency direction is:

```text
constants
    ↓
types
    ↓
shared routines
    ↓
main program
```

Shared files should depend downward on foundational files rather than on the main program.

## Avoid Duplicate Includes

If two files both include the same declarations, the compiler may encounter duplicate definitions.

Example risk:

```text
main.mps
├── includes a.mps
│   └── includes common.mps
└── includes b.mps
    └── includes common.mps
```

If `common.mps` defines the same constants, types, or routines twice in one compilation, duplicate declaration errors may result.

Do not assume MPL provides include guards like C/C++.

Structure dependencies so shared declarations are included once unless compiler testing proves another safe mechanism.

## Include Files and Scope

An include file does not automatically create a separate namespace.

Names introduced by included source become part of the program's compilation context.

For example, if an include defines:

```pascal
Const
  MaxUsers = 100;
```

the main program can use:

```pascal
For Index := 1 To MaxUsers Do
Begin
  ...
End;
```

This also means names in includes can conflict with declarations elsewhere in the program.

Use distinctive names and keep shared declarations organized.

## Include Files and Records

Record types are a strong candidate for shared source.

`include/records.mps`:

```pascal
Library

Type
  TUserInfo = Record
    Name     : String[30];
    Level    : Integer;
    IsActive : Boolean;
  End;
```

Main program:

```pascal
Include include/records.mps

Var
  UserInfo : TUserInfo;

Begin
  UserInfo.Name := 'Example User';
End.
```

See [Records](Records).

## Include Files and Procedures

`include/display.mps`:

```pascal
Library

Procedure ShowHeader;
Begin
  WriteLn('Mystic MPL Application');
End;
```

Main source:

```pascal
Include include/display.mps

Begin
  ShowHeader;
End.
```

See [Procedures](Procedures).

## Include Files and Functions

`include/access.mps`:

```pascal
Library

Const
  MinimumLevel = 20;

Function HasAccess(Level Integer) : Boolean;
Begin
  HasAccess := Level >= MinimumLevel;
End;
```

Main source:

```pascal
Include include/access.mps

Begin
  If HasAccess(UserLevel) Then
    WriteLn('Access granted');
End.
```

See [Functions](Functions).

## MPLC Bulk Compilation

Mystic 1.12 expanded MPLC bulk and recursive compilation options.

Repository history documents forms such as:

```text
-ALL
-C
-P
-R
```

Common purposes include:

| Option | General purpose |
|---|---|
| `-ALL` | Compile scripts recursively from the current tree |
| `-C` | Compile scripts in the current directory |
| `-P [path]` | Compile scripts in a selected path |
| `-R [path]` | Compile scripts recursively below a selected path |

Exact option behavior should be confirmed with:

```text
MPLC help output
Mystic version
MPLC version
Operating system
```

The `Library` source marker is particularly useful with these operations because include-only files can be encountered during directory scans.

## Compiler Options Are Not Source Directives

Commands such as:

```text
MPLC -ALL
MPLC -C
MPLC -P ...
MPLC -R ...
```

control MPLC from the command line.

They are **compiler options**, not statements written inside an MPL source file.

Keep the concepts separate:

```text
Include common.mps     ← MPL source syntax
Library                ← source marker for include libraries
MPLC -ALL              ← compiler command-line option
{$include ...}         ← historical directive-like syntax
```

## Compiler Error Location

An error inside an included file can prevent the main source from compiling.

When diagnosing an include error, record:

```text
Main source:
Included source:
Include statement:
MPLC version:
Error file:
Error line:
Error column:
Working directory:
```

Correct the earliest reported error first.

A syntax failure inside one include can cause later declarations to appear missing and generate secondary errors.

## Safe Include Migration Workflow

When converting an older MPL project:

1. Back up all source and compiled files.
2. Identify every main `.mps` program.
3. Identify every shared/include source file.
4. Search for `#include`.
5. Search for `{$include`.
6. Convert to the `Include filename` form accepted by the target compiler.
7. Verify include order.
8. Check filename capitalization.
9. Compile one main program at a time.
10. Correct errors inside includes before downstream errors.
11. Test the resulting `.mpx` program.
12. When using Mystic 1.12-compatible MPLC, consider `Library` for include-only source.
13. Test bulk compilation separately from single-file compilation.

## Complete Example

Project:

```text
example/
├── main.mps
└── include/
    ├── constants.mps
    ├── types.mps
    └── access.mps
```

`include/constants.mps`:

```pascal
Library

Const
  MinimumLevel = 20;
```

`include/types.mps`:

```pascal
Library

Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;
```

`include/access.mps`:

```pascal
Library

Function HasAccess(UserInfo : TUserInfo) : Boolean;
Begin
  HasAccess := UserInfo.Level >= MinimumLevel;
End;
```

`main.mps`:

```pascal
Include include/constants.mps
Include include/types.mps
Include include/access.mps

Var
  UserInfo : TUserInfo;

Begin
  UserInfo.Name := 'Example User';
  UserInfo.Level := 25;

  WriteLn('User: ' + UserInfo.Name);

  If HasAccess(UserInfo) Then
    WriteLn('Access granted')
  Else
    WriteLn('Access denied');

  WriteLn('|PA');
End.
```

This example demonstrates:

- Three include libraries
- Dependency ordering
- Shared constants
- Shared record types
- Shared functions
- A small main source focused on application flow

The `Library` marker requires Mystic 1.12 or a compatible later MPLC build. For an earlier compiler, use the include syntax appropriate to that release and verify how include-only source behaves during compilation.

## Common Errors

### Using Historical Include Syntax in Modern Source

Historical:

```text
#include common.mps
```

or:

```text
{$include common.mps}
```

Modern 1.10-style form:

```pascal
Include common.mps
```

### Assuming `Library` Exists in Mystic 1.10

`Library` as an include-only source marker is documented in Mystic 1.12 Alpha 18.

Do not use it as a portability requirement for 1.10 or 1.11 source.

### Wrong Include Path

Source:

```pascal
Include include/common.mps
```

but actual file:

```text
includes/common.mps
```

The compiler cannot resolve a path that does not match the project layout.

### Filename Case Mismatch

File:

```text
Common.mps
```

Source:

```pascal
Include common.mps
```

This may fail on a case-sensitive filesystem.

### Including a File Twice

Duplicate declarations may be produced when the same source is included more than once through different dependency paths.

### Depending on Working Directory

A build that succeeds only when MPLC is launched from one exact directory is fragile.

Prefer a clear source-relative project layout when supported.

### Assuming Generic Pascal Directives

Do not copy conditional-compilation or mode directives from Free Pascal documentation into MPL without testing them in MPLC.

## Verification Checklist

Before marking this page fully verified, test and record:

- Modern `Include filename` syntax
- Include with a same-directory file
- Include with a relative subdirectory
- Include search relative to the source file
- Include search relative to the working directory
- Nested includes
- Duplicate include behavior
- Circular include behavior
- Filename case sensitivity
- Include filenames containing spaces, if needed
- Alternate path separators
- Include file extension handling
- Historical `#include` behavior on old compilers
- Historical `{$include ...}` behavior
- `Library` marker
- Standalone compilation of a `Library` source
- Bulk compilation with library sources present
- `-ALL`
- `-C`
- `-P`
- `-R`
- Error file/line reporting for included source
- Maximum include depth
- General compiler-directive support, if any

Suggested verification record:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
Include syntax:
Same-directory include:
Relative-path include:
Source-relative lookup:
Nested include:
Duplicate include:
Circular include:
Case sensitivity:
Library marker:
Bulk compile:
-ALL:
-C:
-P:
-R:
Historical include syntax:
Other directives:
Notes:
```

## Version Reference

Include behavior changed across Mystic's MPL history.

### Pre-1.10 / Historical MPE

Older source used directive-like include syntax such as:

```text
#include common.mps
```

### Mystic 1.10

The 1.10 development cycle moved through a directive-like form:

```text
{$include common.mps}
```

and later standardized the direct MPL keyword:

```pascal
Include common.mps
```

Repository references:

- [Mystic 1.10 Changes](../documents/mystic-changes/Mystic-1.10.md)
- [Mystic 1.10 Alpha 1–21](../documents/mystic-changes/1.10/Alpha-01-21.md)
- [Mystic 1.10 Alpha 33–43](../documents/mystic-changes/1.10/Alpha-33-43.md)

### Mystic 1.12

Important compiler/include changes include:

- `Library` source marker for include-only files
- Bulk and recursive MPLC compilation options
- Improved include-file lookup relative to source files
- Improved error reporting during multi-file compilation

Repository references:

- [Mystic 1.12 Changes](../documents/mystic-changes/Mystic-1.12.md)
- [Mystic 1.12 Alpha 13–24](../documents/mystic-changes/1.12/Alpha-13-24.md)
- [MPL Change Index](../documents/mystic-changes/MPL-Change-Index.md)

## Related Pages

- [Program Structure](Program-Structure)
- [Constants](Constants)
- [Data Types](Data-Types)
- [Arrays](Arrays)
- [Records](Records)
- [Strings](Strings)
- [Procedures](Procedures)
- [Functions](Functions)
- [Compiler Behavior](Compiler-Behavior)
- [Installing and Compiling](Installing-and-Compiling)
- [Compiler Usage](Compiler-Usage)
- [Troubleshooting](Troubleshooting)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- Repository: `documents/mystic-changes/Mystic-1.10.md`
- Repository: `documents/mystic-changes/1.10/Alpha-01-21.md`
- Repository: `documents/mystic-changes/1.10/Alpha-33-43.md`
- Repository: `documents/mystic-changes/Mystic-1.12.md`
- Repository: `documents/mystic-changes/1.12/Alpha-13-24.md`
- Repository: `documents/mystic-changes/MPL-Change-Index.md`

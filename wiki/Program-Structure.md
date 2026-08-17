# Program Structure

MPL programs can be very small scripts or larger structured programs containing constants, variables, custom types, procedures, functions, and a main executable block.

This page explains how those pieces fit together and where executable code belongs.

> **Documentation status**
>
> This page documents the modern Pascal-style MPL structure established during the Mystic 1.10 language redesign. Mystic's own examples often omit a Pascal `Program Name;` header, so that header is not treated here as a required part of an MPL source file. Compiler-specific behavior should still be verified with the exact Mystic and MPLC build in use.

## Source and Compiled Files

MPL source code is normally stored in a file using the `.mps` extension:

```text
example.mps
```

MPLC compiles the source into an executable MPL file:

```text
example.mpx
```

The normal workflow is:

```text
example.mps
    ↓
   MPLC
    ↓
example.mpx
    ↓
  Mystic
```

The `.mps` file is the source that should be edited and retained. Mystic executes the compiled `.mpx` program.

See [Installing and Compiling](Installing-and-Compiling) and [Compiler Behavior](Compiler-Behavior) for compiler details.

## Minimal MPL Program

An MPL source file can be extremely small.

```pascal
WriteLn('Hello from MPL!');
WriteLn('|PA');
```

This form is useful for small utilities, short menu scripts, compiler tests, and simple display or control scripts.

A source file does not need to be artificially expanded into a large Pascal-style skeleton when only a few executable statements are required.

## Structured MPL Program

As a program grows, declarations and routines make the source easier to understand and maintain.

A common structure is:

```pascal
Const
  AppName = 'Example MPL';

Type
  TExample = Record
    Name : String[30];
    Count : Integer;
  End;

Var
  Item : TExample;
  IsRunning : Boolean = True;

Procedure DisplayTitle;
Begin
  WriteLn(AppName);
End;

Function HasItems : Boolean;
Begin
  HasItems := Item.Count > 0;
End;

Begin
  DisplayTitle;

  Item.Name := 'Test';
  Item.Count := 1;

  If HasItems Then
    WriteLn('Items are available');
End;
```

This separates the program into constants, custom types, variables, procedures and functions, and main executable code. Not every program needs every section.

## No Required `Program Name;` Header

Traditional Pascal commonly begins with:

```pascal
Program Example;
```

Mystic's current MPL documentation and distributed-style examples generally begin directly with declarations or executable statements instead.

For example:

```pascal
Var
  Count : Integer = 1;

Begin
  WriteLn(Int2Str(Count));
End;
```

or simply:

```pascal
WriteLn('Hello');
```

For this documentation project, `Program Example;` should therefore **not** be presented as required MPL syntax unless it is separately confirmed with the target MPLC build.

## Declaration Sections

Structured MPL programs can declare values and routines before executable code.

Common declaration sections include:

```text
Const
Type
Var
Procedure
Function
```

The exact sections used depend on the program.

### Constants

```pascal
Const
  MaxAttempts = 3;
  AppTitle = 'User Lookup';
```

See [Constants](Constants).

### Types

`Type` declarations define custom data structures such as records.

```pascal
Type
  TUserInfo = Record
    Name : String[30];
    Level : Integer;
  End;
```

The full record syntax belongs on the future [Records](Records) page.

### Variables

```pascal
Var
  UserName : String;
  UserLevel : Integer;
  IsActive : Boolean;
```

Modern MPL also allows declaration-time initialization:

```pascal
Var
  Count : Integer = 0;
  IsRunning : Boolean = True;
```

Mystic 1.10 also expanded local declarations so variables can exist within procedures and functions.

See [Variables](Variables) and [Data Types](Data-Types).

## Procedures

Procedures group reusable statements that do not need to return a value.

```pascal
Procedure DisplayHeader;
Begin
  WriteLn('Mystic MPL Example');
  WriteLn('------------------');
End;
```

The procedure can later be called from executable code:

```pascal
DisplayHeader;
```

See [Procedures](Procedures).

## Functions

Functions group reusable logic and return a value.

```pascal
Function IsValidLevel(Level Integer) : Boolean;
Begin
  IsValidLevel := Level >= 20;
End;
```

The result can be used in an expression or condition:

```pascal
If IsValidLevel(UserLevel) Then
  WriteLn('Access granted');
```

See [Functions](Functions).

## Main Executable Block

A larger MPL program commonly places its primary executable statements inside a final `Begin` and `End` block.

```pascal
Begin
  DisplayHeader;
  LoadData;
  ProcessData;
End;
```

This makes the program's startup flow easy to identify after declarations and routines.

Small scripts may instead contain executable statements directly without a surrounding main block.

## `Begin` and `End`

`Begin` and `End` group several statements into one code block.

```pascal
Begin
  WriteLn('First statement');
  WriteLn('Second statement');
End;
```

Blocks are particularly important in control structures when more than one statement belongs to a branch or loop.

```pascal
If IsActive Then
Begin
  WriteLn('User is active');
  WriteLn('Loading user data');
End;
```

The Mystic 1.10 parser redesign standardized this Pascal-style block syntax and replaced older forms such as `EndIf`, `WEnd`, and `FEnd` with `End` where a block is required.

See [Conditional Logic](Conditional-Logic) and [Loops](Loops).

## `End;` and `End.`

Pascal-style source uses a semicolon when a block is part of a larger structure:

```pascal
Procedure ShowStatus;
Begin
  WriteLn('Ready');
End;
```

Many structured MPL examples use a period to terminate the final program block:

```pascal
Begin
  ShowStatus;
End.
```

Mystic examples are not completely consistent about final punctuation, especially between older and newer documentation. For portable wiki examples, use `End;` for procedure, function, conditional, loop, and nested blocks, and prefer `End.` for the final structured program block when testing confirms that form for the target compiler.

A minimal statement-only script does not require a final `End.` because it has no enclosing main block.

## Semicolons

Statements are generally terminated with a semicolon:

```pascal
Count := Count + 1;
WriteLn('Done');
```

Declarations also commonly use semicolons:

```pascal
Var
  Count : Integer;
  Name : String;
```

Do not assume that a missing semicolon accepted by one MPLC build is portable to another build. Use consistent semicolon termination in wiki examples.

## Scope

Where a variable is declared determines where it can be used.

### Program-level variables

```pascal
Var
  GlobalCount : Integer;
```

A program-level variable can be used by the program and routines that can see that declaration.

### Local variables

```pascal
Procedure ShowCount;
Var
  LocalCount : Integer = 10;
Begin
  WriteLn(Int2Str(LocalCount));
End;
```

`LocalCount` belongs to `ShowCount` rather than the entire program.

Local variables and nested routine definitions were major additions during the Mystic 1.10 MPL redesign.

## Nested Procedures and Functions

Modern MPL allows procedures and functions to be defined inside other routines.

```pascal
Procedure OuterProcedure;

  Procedure InnerProcedure;
  Begin
    WriteLn('Inside');
  End;

Begin
  InnerProcedure;
End;
```

Nested routines should be used when the helper belongs only to the containing routine. If several parts of the program need the helper, a program-level routine is usually clearer.

## Initialization Order

Declaration-time initialization is supported by modern MPL.

```pascal
Var
  Counter : Integer = 1;
  IsReady : Boolean = True;
```

Mystic 1.10 development also added support for initializing variables using function results.

```pascal
Var
  TotalBases : LongInt = GetMBaseTotal(False);
```

When initialization depends on Mystic runtime state, test the program in the actual execution context.

## Includes and Shared Source

Larger MPL projects may move common declarations and routines into include files.

Include syntax changed during the Mystic 1.10 development cycle, so include directives should be documented separately and verified against the target compiler.

Do not copy an include form from an old MPL program without checking its version.

See the planned [Include Files](Include-Files) page.

## Runtime Context

An MPL program runs inside Mystic and can access BBS-specific state in addition to normal language constructs.

Modern MPL includes runtime information such as:

```text
ProgName
ProgParams
```

`ProgName` identifies the current MPL program path and filename, while `ProgParams` contains parameters passed to the program.

```pascal
Begin
  WriteLn('Program: ' + ProgName);
  WriteLn('Parameters: ' + ProgParams);
End;
```

Runtime variables and Mystic-specific records will be covered on their own integration pages rather than mixed into the core language structure.

## Recommended File Organization

For a medium-sized MPL program, a useful source order is:

```text
Documentation comments
        ↓
Constants
        ↓
Custom types
        ↓
Program-level variables
        ↓
Procedures and functions
        ↓
Main executable block
```

Example:

```pascal
// User status example

Const
  MinimumLevel = 20;

Type
  TStatus = Record
    Name : String[30];
    Level : Integer;
  End;

Var
  Status : TStatus;

Function HasAccess(Level Integer) : Boolean;
Begin
  HasAccess := Level >= MinimumLevel;
End;

Procedure ShowStatus;
Begin
  WriteLn('User: ' + Status.Name);
  WriteLn('Level: ' + Int2Str(Status.Level));
End;

Begin
  Status.Name := 'Example User';
  Status.Level := 25;

  ShowStatus;

  If HasAccess(Status.Level) Then
    WriteLn('Access granted')
  Else
    WriteLn('Access denied');
End.
```

## Small Script Versus Structured Program

Use a small direct script when the job is simple:

```pascal
WriteLn('Maintenance complete');
WriteLn('|PA');
```

Use a structured program when the source contains repeated logic, several variables, records or arrays, file access, multiple Mystic APIs, or logic that needs compiler tests.

Structured code reduces duplication and makes version-specific changes easier to isolate.

## Common Structural Mistakes

### Assuming a Pascal `Program` Header Is Required

Do not automatically begin every MPL file with:

```pascal
Program Example;
```

Mystic's current examples commonly omit this declaration. Treat it as compiler-specific until verified.

### Executable Code in the Wrong Place

Incorrect concept:

```pascal
Var
  Count : Integer;
  WriteLn('Starting');
```

Keep declarations and executable code separate.

### Missing a Block Around Multiple Statements

Incorrect:

```pascal
If IsReady Then
  WriteLn('Ready');
  Count := Count + 1;
```

Only the first statement belongs to the condition.

Correct:

```pascal
If IsReady Then
Begin
  WriteLn('Ready');
  Count := Count + 1;
End;
```

### Unbalanced `Begin` and `End`

```pascal
Begin
  If IsReady Then
  Begin
    WriteLn('Ready');
  End;
End.
```

Indentation makes unmatched blocks easier to find even though indentation itself does not define scope.

### Mixing Old and Modern MPL Syntax

Older programs may contain:

```text
EndIf
WEnd
FEnd
ElseIf
```

Modern Pascal-style MPL uses `Then`, `Do`, `Begin`, `End`, and separate `Else If` keywords.

Do not combine old and new structural forms in the same example.

## Complete Example

```pascal
Const
  MinimumLevel = 20;

Var
  UserName : String = 'Example User';
  UserLevel : Integer = 25;
  IsActive : Boolean = True;

Function CanEnter(Level Integer, Active Boolean) : Boolean;
Begin
  CanEnter := (Level >= MinimumLevel) And Active;
End;

Procedure DisplayUser;
Begin
  WriteLn('User: ' + UserName);
  WriteLn('Level: ' + Int2Str(UserLevel));
End;

Begin
  DisplayUser;

  If CanEnter(UserLevel, IsActive) Then
  Begin
    WriteLn('Access granted');
  End
  Else
  Begin
    WriteLn('Access denied');
  End;

  WriteLn('|PA');
End.
```

This example demonstrates a constant section, initialized program-level variables, a Boolean function, a procedure, a final executable block, and Pascal-style conditional blocks.

## Verification Checklist

Before marking this page fully verified, test and record:

- Statement-only `.mps` source
- Structured source with a final `Begin` / `End` block
- Whether `Program Name;` is accepted, ignored, or rejected
- Final `End.` behavior
- Final `End;` behavior
- Declaration order requirements
- `Const`, `Type`, and `Var` sections
- Program-level procedures and functions
- Local variables
- Nested procedures and functions
- Declaration-time initialization
- Function-result initialization
- Include-file placement and syntax
- `ProgName`
- `ProgParams`

Suggested verification record:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
Statement-only source:
Structured main block:
Program header:
End-period behavior:
Declaration sections:
Local declarations:
Nested routines:
Initialization behavior:
ProgName/ProgParams:
Notes:
```

## Version Reference

The modern structure described here is primarily based on the Mystic 1.10 MPL redesign.

Repository references:

- [Mystic 1.10 Changes](../documents/mystic-changes/Mystic-1.10.md)
- [Mystic 1.10 Alpha 1–21](../documents/mystic-changes/1.10/Alpha-01-21.md)
- [MPL Change Index](../documents/mystic-changes/MPL-Change-Index.md)

The 1.10 redesign standardized Pascal-style declarations and blocks, added local variables and nested routines, increased variable capacity, and changed several older structural forms.

## Related Pages

- [Getting Started](Getting-Started)
- [Your First MPL Program](Your-First-MPL-Program)
- [MPL Syntax](MPL-Syntax)
- [Constants](Constants)
- [Variables](Variables)
- [Data Types](Data-Types)
- [Operators](Operators)
- [Conditional Logic](Conditional-Logic)
- [Loops](Loops)
- [Procedures](Procedures)
- [Functions](Functions)
- [Compiler Behavior](Compiler-Behavior)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- Mystic BBS Wiki: Mystic Programming Language
- Mystic BBS Wiki: Mystic 1.10 Changes
- Repository: `documents/mystic-changes/Mystic-1.10.md`

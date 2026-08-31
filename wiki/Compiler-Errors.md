# Compiler Errors

Compiler errors occur when MPLC cannot successfully translate an MPL source file into a valid compiled `.mpx` program.

This page focuses specifically on **compile-time errors**: how to read them, how to isolate the real cause, and how to distinguish syntax, declaration, type, include, and version-compatibility problems.

Runtime failures after a program has compiled belong on [Troubleshooting](Troubleshooting) and the relevant language or integration page.

> **Documentation status**
>
> Exact MPLC wording varies between Mystic releases. The error categories and diagnostic methods on this page are intended to be stable, while literal error messages should be recorded from the exact MPLC build being tested.

## Compiler Error Versus Runtime Error

A compiler error happens before a valid new `.mpx` is produced.

Examples include:

- Invalid syntax
- Unknown identifiers
- Type mismatches
- Missing declarations
- Incorrect procedure or function calls
- Missing include files
- Invalid record or array use
- Unsupported syntax for the target Mystic version

A runtime error occurs after compilation when the `.mpx` program is executed.

Examples include:

- Invalid file access
- Division by zero
- Array index problems
- Unexpected BBS state
- Missing runtime files
- Incorrect Mystic integration assumptions

The first question when debugging should be:

```text
Did MPLC fail to compile the source,
or did the compiled MPX fail while running?
```

## Capture the Exact Compiler Output

Before changing source, record the complete MPLC output.

Example build:

```bash
./mplc example.mps
```

Capture it:

```bash
./mplc example.mps >example-build.log 2>&1
```

Also record the exit status:

```bash
./mplc example.mps >example-build.log 2>&1
printf 'exit_status=%s\n' "$?" >>example-build.log
```

Do not paraphrase compiler messages when documenting a problem. Preserve:

```text
Source filename
Line number
Column number
Exact error text
MPLC version
Mystic version
Operating system
Architecture
```

## Read the First Error First

One bad token can cause many later errors.

For example, a missing semicolon can make several following lines appear invalid:

```pascal
Var
  Count : Integer

Begin
  Count := 1;
  WriteLn(Int2Str(Count));
End.
```

The actual problem is after:

```pascal
Count : Integer
```

not necessarily where the final compiler message points.

General rule:

> Fix the earliest compiler error, compile again, and only then evaluate the remaining errors.

Do not try to solve twenty reported errors independently when the first one may be causing nineteen secondary errors.

## Line and Column Numbers

Modern MPLC builds provide source-location information that can identify the line and, in supported builds, the column where parsing failed.

A representative diagnostic might conceptually identify:

```text
example.mps
line 27
column 14
```

The reported position tells you where MPLC became unable to continue parsing correctly.

It does **not** guarantee that the mistake began at that exact character.

Inspect:

1. The reported line
2. The previous line
3. The current enclosing block
4. The current procedure or function declaration
5. Any included source that feeds into that location

## The Error Can Be on the Previous Line

This is one of the most important compiler-debugging habits.

Source:

```pascal
UserName := 'Mystic User'
WriteLn(UserName);
```

The compiler may complain when it reaches:

```pascal
WriteLn
```

but the actual problem is the missing semicolon after:

```pascal
'Mystic User'
```

Whenever the reported line looks valid, inspect the statement immediately before it.

## Syntax Errors

Syntax errors mean MPLC could not parse the source according to the language grammar.

Common causes include:

- Missing semicolon
- Missing `Then`
- Missing `Do`
- Missing `Begin`
- Missing `End`
- Invalid declaration syntax
- Invalid parameter syntax
- Unterminated string
- Unterminated comment
- Missing parenthesis
- Extra parenthesis
- Historical syntax used with a modern compiler

## Missing Semicolon

Incorrect:

```pascal
Count := 10
WriteLn(Int2Str(Count));
```

Correct:

```pascal
Count := 10;
WriteLn(Int2Str(Count));
```

The compiler may report the second line even though the first line contains the mistake.

## Missing `Then`

Modern MPL conditional syntax uses `Then`.

Incorrect:

```pascal
If Count > 10
  WriteLn('High');
```

Correct:

```pascal
If Count > 10 Then
  WriteLn('High');
```

This is especially important when migrating pre-1.10 MPL source.

See [Conditional Logic](Conditional-Logic).

## Missing `Do`

Modern `For` and `While` syntax uses `Do`.

Incorrect:

```pascal
While Count < 10
Begin
  Count := Count + 1;
End;
```

Correct:

```pascal
While Count < 10 Do
Begin
  Count := Count + 1;
End;
```

See [Loops](Loops).

## Unbalanced `Begin` and `End`

Incorrect:

```pascal
If IsReady Then
Begin
  WriteLn('Ready');
```

The block is never closed.

Correct:

```pascal
If IsReady Then
Begin
  WriteLn('Ready');
End;
```

Indentation is not required by the compiler, but it makes structural errors easier to see.

## Unexpected `End`

An `End` error often means one of these:

- Too many `End` statements
- Missing `Begin`
- A procedure or function was closed too early
- A control structure is malformed
- An earlier syntax error caused the parser to lose block context

Example:

```pascal
Begin
  WriteLn('One');
End;
End;
```

Count blocks from the nearest enclosing declaration rather than only examining the reported line.

## Unexpected End of File

A compiler reaching the end of source unexpectedly commonly indicates:

- Missing `End`
- Missing closing parenthesis
- Unterminated string literal
- Unterminated block comment
- Incomplete procedure
- Incomplete function
- Incomplete record declaration
- Incomplete `Case` block

Check the final complete block first.

## Unterminated String

Incorrect:

```pascal
WriteLn('Hello);
```

Correct:

```pascal
WriteLn('Hello');
```

A missing quote can cause much of the following source to be interpreted incorrectly.

## Comment Errors

Modern MPL source should use comment forms verified for the target compiler.

Examples commonly used by modern MPL:

```pascal
// One-line comment
```

and:

```pascal
(*
  Block comment
*)
```

An unclosed block comment can make the rest of the file disappear from the parser's point of view.

When migrating historical source, check comment syntax as part of the conversion.

## Unknown Identifier

An unknown identifier means MPLC cannot resolve a name being used.

Possible causes include:

- Misspelled variable
- Misspelled constant
- Misspelled procedure
- Misspelled function
- Declaration appears after use
- Missing include file
- Wrong include order
- Identifier is out of scope
- Built-in was renamed
- Built-in does not exist in the target Mystic version

Example:

```pascal
Var
  UserName : String;

Begin
  WriteLn(UserNmae);
End.
```

The typo:

```text
UserNmae
```

does not match:

```text
UserName
```

## Declaration Before Use

Custom types and declarations should exist before dependent declarations.

Incorrect:

```pascal
Var
  UserInfo : TUserInfo;

Type
  TUserInfo = Record
    Name : String[30];
  End;
```

Correct:

```pascal
Type
  TUserInfo = Record
    Name : String[30];
  End;

Var
  UserInfo : TUserInfo;
```

See [Program Structure](Program-Structure) and [Records](Records).

## Scope Errors

A local variable belongs to its routine.

Example:

```pascal
Procedure Test;
Var
  LocalCount : Integer;
Begin
  LocalCount := 1;
End;
```

Do not assume this is valid outside the procedure:

```pascal
WriteLn(Int2Str(LocalCount));
```

If MPLC reports an unknown identifier, verify the variable's scope before assuming the spelling is wrong.

## Duplicate Identifier

Duplicate declarations can occur when:

- A variable is declared twice
- A constant and variable reuse the same name
- Two include files define the same identifier
- The same include file is processed twice
- A shared routine has been copied into the main file and also included

Example:

```pascal
Var
  Count : Integer;
  Count : LongInt;
```

Use one clear declaration.

Include dependency problems are covered in [Include Files and Compiler Directives](Include-Files).

## Type Mismatch

A type mismatch occurs when the source combines incompatible values.

Incorrect:

```pascal
Var
  Count : Integer;

Begin
  Count := 'Ten';
End.
```

The destination is numeric, but the assigned value is text.

Correct:

```pascal
Count := 10;
```

## String and Numeric Types

Do not assume automatic numeric-to-string conversion.

Potentially invalid:

```pascal
WriteLn('Count: ' + Count);
```

Preferred documented form:

```pascal
WriteLn('Count: ' + Int2Str(Count));
```

See [Strings](Strings) and [Data Types](Data-Types).

## `Char` Versus `String`

A `Char` holds one character.

Incorrect concept:

```pascal
Var
  Choice : Char;

Begin
  Choice := 'YES';
End.
```

Use:

```pascal
Choice := 'Y';
```

or use a `String` when multiple characters are required.

## Invalid Array Use

Common array-related compiler problems include:

- Wrong declaration syntax
- Parentheses used instead of modern square brackets
- Wrong number of dimensions
- Incompatible element assignment
- Invalid array parameter syntax

Modern access:

```pascal
Scores[1] := 100;
```

Historical form:

```text
Scores(1)
```

See [Arrays](Arrays).

## Invalid Record Use

A field must be accessed through the record variable:

```pascal
UserInfo.Name := 'Sysop';
```

Do not confuse the complete record with one field:

```pascal
UserInfo := 'Sysop';
```

unless the right side is another compatible complete record.

See [Records](Records).

## Procedure and Function Errors

Compiler errors involving routines commonly come from:

- Wrong procedure name
- Wrong function name
- Too few parameters
- Too many parameters
- Parameters in the wrong order
- Wrong parameter types
- Missing `Var` compatibility
- Wrong return type
- A feature not supported by the target Mystic version

## Incorrect Parameter Count

Given:

```pascal
Procedure ShowUser(Name String, Level Integer);
Begin
  WriteLn(Name);
End;
```

incorrect:

```pascal
ShowUser('Example');
```

correct:

```pascal
ShowUser('Example', 20);
```

## Incorrect Parameter Type

Given:

```pascal
Procedure SetLevel(Level Integer);
Begin
End;
```

do not pass text:

```pascal
SetLevel('Twenty');
```

Pass a compatible integer:

```pascal
SetLevel(20);
```

## `Var` Parameter Errors

A `Var` parameter requires a writable variable.

Given:

```pascal
Procedure ResetCount(Var Count : Integer);
Begin
  Count := 0;
End;
```

valid:

```pascal
ResetCount(CurrentCount);
```

invalid concept:

```pascal
ResetCount(10);
ResetCount(CurrentCount + 1);
```

The procedure needs storage it can modify.

## Function Result Errors

A function returns a value through its declared result.

Example:

```pascal
Function AddOne(Value Integer) : Integer;
Begin
  AddOne := Value + 1;
End;
```

Check:

- Function result type
- Assignment to the function name
- Caller destination type
- Parameter types

Record-returning functions require a compatible Mystic version; see [Records](Records).

## Include File Errors

Include-related compiler failures can look like missing language declarations because the expected shared source was never loaded.

Check:

```pascal
Include include/common.mps
```

against the actual filesystem:

```text
include/common.mps
```

On case-sensitive systems:

```text
common.mps
Common.mps
```

may be different files.

## Error Inside an Include

An include can contain the actual compiler error.

Project:

```text
main.mps
└── include/common.mps
```

If `common.mps` has a syntax error, the main program cannot compile correctly.

When MPLC reports a file name, line, or column, confirm whether the location belongs to:

```text
main.mps
```

or an included source file.

Always fix the earliest error in the actual file reported.

## Include Order Errors

Example:

```pascal
Include include/functions.mps
Include include/types.mps
```

If `functions.mps` depends on a record defined in `types.mps`, the compiler may report unknown types or identifiers.

Correct dependency order:

```pascal
Include include/types.mps
Include include/functions.mps
```

See [Include Files and Compiler Directives](Include-Files).

## Historical Syntax Errors

Mystic 1.10 significantly changed MPL syntax.

Historical source may contain forms such as:

```text
EndIf
WEnd
FEnd
ElseIf
Scores(1)
{$include common.mps}
```

Modern source uses forms such as:

```pascal
End
Else If
Scores[1]
Include common.mps
```

An error in old source may be a version-migration problem rather than a simple typo.

## Unsupported Feature Errors

A source file may be syntactically correct for one Mystic release and fail on another.

Examples of version-sensitive features include:

- Modern 1.10 parser syntax
- Multidimensional arrays
- Record assignment
- Record parameters
- Record-returning functions
- Embedded records
- `Library` include-only source marker
- Newer built-ins
- Changed runtime variables

When a valid-looking construct fails, record the exact:

```text
Mystic version
MPLC version
Feature
Earliest known supported release
```

Then check [Version Compatibility](Version-Compatibility) and the repository's version-history files.

## Not-Equal Operator Conflict

This repository already records a version/documentation conflict between:

```text
<>
```

and:

```text
!=
```

for not-equal comparisons.

If MPLC rejects one form, test the other against the target build and document the compiler result.

Do not "fix" the source solely from generic Pascal expectations.

See [Operators](Operators).

## Record Initialization Errors

Record declaration-time initialization is not a normal supported pattern.

Do not assume:

```pascal
Var
  UserInfo : TUserInfo = ...;
```

Later Mystic 1.12 compiler work explicitly rejected invalid default record initialization rather than compiling it incorrectly.

Initialize record fields in executable code.

See [Records](Records).

## Array Initialization Errors

The documented MPL rules exclude arrays from ordinary declaration-time initialization.

Do not assume:

```pascal
Var
  Values : Array[1..3] of Integer = ...;
```

Initialize elements explicitly:

```pascal
Values[1] := 10;
Values[2] := 20;
Values[3] := 30;
```

See [Arrays](Arrays).

## Compiler Reports Multiple Errors

Mystic 1.12 development improved MPLC so it could track multiple compiler errors and print a summary after processing.

This is useful, but it does not change the debugging strategy.

If MPLC reports:

```text
Error 1
Error 2
Error 3
Error 4
```

start with:

```text
Error 1
```

Compile again after fixing it.

The number of remaining errors may drop dramatically.

## One Bad Include Can Cause Many Errors

Suppose an include that defines:

```pascal
TUserInfo
HasAccess
MinimumLevel
```

fails near the beginning.

The main program may then report all three as unknown.

Those are downstream symptoms.

Fix the include's first syntax error before editing every use of the missing declarations.

## Error Output to Build Tools

Mystic 1.12 development changed MPLC output so compiler text could be written through standard I/O for integration with editors and build tools.

This allows commands such as:

```bash
./mplc example.mps >example-build.log 2>&1
```

and editor/build integrations that capture compiler messages.

Verify the exact behavior of the target MPLC build before relying on automated parsing.

## Stale `.mpx` After a Compiler Failure

A failed compile does not necessarily prove that no older `.mpx` exists.

Check:

```bash
ls -l example.mps example.mpx
```

Compare timestamps.

A stale `.mpx` can make testing confusing:

```text
Source contains new code
        ↓
Compilation fails
        ↓
Old MPX remains
        ↓
Mystic runs old behavior
```

Never assume the running program represents the source you just attempted to compile.

## Confirm the Output File Changed

After a successful compile:

```bash
ls -l example.mps example.mpx
```

Confirm that the `.mpx` timestamp reflects the new build.

When diagnosing deployment problems, also search for duplicate output files:

```bash
find . -type f -iname 'example.mpx' -ls
```

## Minimal Reproduction

When the error is unclear, reduce the source.

Start with:

```pascal
WriteLn('Test');
```

Compile it.

Then add one language feature at a time:

```text
Variable declaration
String
Condition
Loop
Procedure
Function
Array
Record
Include
Mystic built-in
```

When the error returns, the last change usually identifies the relevant language area.

## Binary Search Through a Large Source File

For a large program, temporarily isolate sections instead of reading thousands of lines at once.

Conceptual workflow:

1. Back up the source.
2. Disable or remove half of the suspect code.
3. Compile.
4. Determine which half still contains the failure.
5. Repeat until the failing block is small.

Do not leave temporary debugging edits in the production source.

## Use a Tiny Compiler Test

When verifying uncertain syntax, create a dedicated test source.

Example:

```text
test_array.mps
```

with only:

```pascal
Var
  Values : Array[1..3] of Integer;

Begin
  Values[1] := 10;
  WriteLn(Int2Str(Values[1]));
End.
```

A focused compiler test is better evidence than assuming a large application failed because of that feature.

## Preserve the Failing Source

Before changing a difficult compiler failure, save the failing version.

Example:

```bash
cp -a example.mps example.mps.failed
```

or use version control.

This allows later comparison between:

```text
failing source
working source
```

and prevents the original evidence from being lost.

## Search for Historical Constructs

When upgrading old MPL source, search the entire project.

Examples:

```bash
grep -RniE 'EndIf|WEnd|FEnd|ElseIf|\{\$include|#include' .
```

Array-call syntax may require a more targeted review because parentheses also appear in function calls.

Also search for renamed built-ins documented in the repository's Mystic change history.

## Build One Program at a Time First

Bulk compilation is useful only after individual programs are reasonably clean.

A safer migration workflow is:

```text
Compile one source
        ↓
Fix first error
        ↓
Compile again
        ↓
Run test
        ↓
Move to next source
```

After single-file builds are stable, use supported MPLC bulk options.

See [Include Files and Compiler Directives](Include-Files).

## Compiler Error Diagnostic Workflow

Use this order:

1. Record Mystic and MPLC versions.
2. Capture the exact command.
3. Capture the complete compiler output.
4. Identify the first error.
5. Identify the actual source file: main or include.
6. Inspect the reported line.
7. Inspect the previous line.
8. Check the current `Begin` / `End` structure.
9. Check declarations and scope.
10. Check parameter count and types.
11. Check include order and paths.
12. Check whether the syntax belongs to another Mystic version.
13. Reduce to a minimal test if needed.
14. Compile again after fixing only the first confirmed error.
15. Confirm a new `.mpx` was generated.
16. Runtime-test the new `.mpx` separately.

## Compiler Error Capture Template

```text
Problem:

Mystic version:
MPLC version:
Operating system:
Architecture:
Compiler path:
Working directory:
Compiler command:

Main source:
Included source, if applicable:

Exact compiler output:

First reported error:
Reported file:
Reported line:
Reported column:

Previous source line:
Reported source line:
Following source line:

Expected syntax:
Version reference checked:

Change made:
Compiler result after change:

MPX timestamp updated:
Runtime test result:

Notes:
```

## Common Error Checklist

When MPLC fails, check:

- [ ] Missing semicolon
- [ ] Missing quote
- [ ] Missing parenthesis
- [ ] Missing `Then`
- [ ] Missing `Do`
- [ ] Missing `Begin`
- [ ] Missing `End`
- [ ] Wrong final block termination
- [ ] Unknown identifier
- [ ] Declaration appears after use
- [ ] Scope problem
- [ ] Duplicate declaration
- [ ] Type mismatch
- [ ] Wrong parameter count
- [ ] Wrong parameter type
- [ ] Invalid `Var` argument
- [ ] Wrong array syntax
- [ ] Wrong record syntax
- [ ] Missing include
- [ ] Wrong include order
- [ ] Filename case mismatch
- [ ] Historical MPL syntax
- [ ] Unsupported version-specific feature
- [ ] Renamed built-in
- [ ] Stale `.mpx` mistaken for a new build

## Verification Checklist

Before marking this page fully verified, record real MPLC output for:

- Missing semicolon
- Missing `Then`
- Missing `Do`
- Unbalanced `Begin` / `End`
- Unterminated string
- Unknown identifier
- Duplicate identifier
- Type mismatch
- Wrong parameter count
- Wrong parameter type
- Invalid `Var` argument
- Invalid array access syntax
- Invalid record use
- Missing include file
- Error inside an include
- Wrong include order
- Historical syntax failure
- Unsupported-version syntax
- Record declaration initialization
- Array declaration initialization
- Multiple-error summary
- File/line/column reporting
- Compiler output redirection
- Exit status on failure
- Existing `.mpx` behavior after failure

Suggested verification record:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
Line reporting:
Column reporting:
Multiple errors:
Syntax error wording:
Unknown identifier wording:
Type mismatch wording:
Parameter error wording:
Include error wording:
Exit status:
Standard output/error behavior:
Old MPX after failure:
Notes:
```

## Version Reference

Compiler diagnostics changed substantially as MPLC matured.

### Mystic 1.10

The 1.10 redesign introduced a stricter parser and stronger declaration/type checking. Compiler errors became more structured and useful for locating problems, while old source constructs that earlier engines tolerated became visible as migration failures.

Repository references:

- [Mystic 1.10 Changes](../documents/mystic-changes/Mystic-1.10.md)
- [Mystic 1.10 Alpha 1–21](../documents/mystic-changes/1.10/Alpha-01-21.md)

### Mystic 1.12

Important compiler-diagnostic improvements included:

- Tracking multiple compiler errors
- Printing a summary after processing
- Better include lookup
- Improved compiler/editor integration
- MPLC output through standard I/O in later alpha development

Repository references:

- [Mystic 1.12 Changes](../documents/mystic-changes/Mystic-1.12.md)
- [Mystic 1.12 Alpha 13–24](../documents/mystic-changes/1.12/Alpha-13-24.md)
- [Mystic 1.12 Alpha 25–36](../documents/mystic-changes/1.12/Alpha-25-36.md)
- [MPL Change Index](../documents/mystic-changes/MPL-Change-Index.md)

## Related Pages

- [Compiler Behavior](Compiler-Behavior)
- [Troubleshooting](Troubleshooting)
- [Include Files and Compiler Directives](Include-Files)
- [Program Structure](Program-Structure)
- [Data Types](Data-Types)
- [Operators](Operators)
- [Conditional Logic](Conditional-Logic)
- [Loops](Loops)
- [Arrays](Arrays)
- [Records](Records)
- [Strings](Strings)
- [Procedures](Procedures)
- [Functions](Functions)
- [Installing and Compiling](Installing-and-Compiling)
- [Compiler Usage](Compiler-Usage)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- Repository: `documents/mystic-changes/Mystic-1.10.md`
- Repository: `documents/mystic-changes/1.10/Alpha-01-21.md`
- Repository: `documents/mystic-changes/Mystic-1.12.md`
- Repository: `documents/mystic-changes/1.12/Alpha-13-24.md`
- Repository: `documents/mystic-changes/1.12/Alpha-25-36.md`
- Repository: `documents/mystic-changes/MPL-Change-Index.md`

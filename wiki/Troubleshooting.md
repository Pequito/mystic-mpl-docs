# Troubleshooting

This page provides a structured approach for diagnosing MPL compilation and runtime problems.

> **Documentation status**
>
> Error messages and runtime behavior may vary by Mystic version and operating system. Capture the exact compiler output and environment details before drawing conclusions.

## Start with the Exact Error

Record the complete message before changing the source.

Include:

```text
Mystic version:
Operating system:
Compiler command:
Current directory:
Source filename:
Exact error text:
Line number, if reported:
```

Do not paraphrase the error when asking for help. Small wording differences can identify different causes.

## Confirm the Correct File

Verify that the intended source file exists:

```bash
pwd
ls -l hello.mps
```

Confirm that the editor did not create:

```text
hello.mps.txt
```

On case-sensitive systems, these are different filenames:

```text
hello.mps
Hello.mps
HELLO.MPS
```

## Compiler Not Found

### Symptom

```text
mplc: command not found
```

### Checks

Locate the compiler:

```bash
find /path/to/mystic -maxdepth 3 -type f -iname 'mplc*'
```

Run it with a relative or absolute path:

```bash
./mplc hello.mps
```

Or:

```bash
/path/to/mystic/mplc hello.mps
```

Also verify execute permission:

```bash
ls -l /path/to/mystic/mplc
```

## Permission Denied

### Symptom

```text
Permission denied
```

### Checks

Confirm execute permission on the compiler:

```bash
chmod u+x /path/to/mystic/mplc
```

Confirm read permission on source and include files, and write permission in the output directory:

```bash
ls -ld .
ls -l hello.mps
```

Avoid running the compiler as `root` merely to bypass a permissions problem. Correct ownership and mode instead.

## Source File Not Found

### Symptom

The compiler cannot locate the `.mps` file.

### Checks

Use an absolute path:

```bash
/path/to/mystic/mplc /full/path/to/hello.mps
```

Check for spaces, capitalization differences, and an unexpected working directory.

## Include File Not Found

### Symptom

The compiler reports that an include file cannot be located.

### Checks

- Verify the include filename exactly.
- Check capitalization.
- Confirm the file exists.
- Confirm the compiler's working directory.
- Test an explicit relative or absolute path if supported.
- Check whether the active Mystic version uses different include syntax.

Useful commands:

```bash
find /path/to/mystic -type f -iname 'filename.inc'
pwd
```

## Unknown Identifier

### Possible causes

- Misspelled variable, constant, procedure, or function name
- Declaration appears after use
- Required include file is missing
- Built-in identifier is unavailable in the current Mystic version
- Identifier name changed between releases
- Scope does not include the declaration

### Checks

Compare spelling and capitalization throughout the source. Confirm the declaration and required include files.

Fix the first unknown identifier before addressing later errors.

## Missing Semicolon

### Symptom

The compiler reports an unexpected token or points to the line after the actual problem.

### Checks

Inspect the preceding statement:

```pascal
UserName := 'Sysop';
WriteLn(UserName);
```

A missing semicolon often causes the next valid line to be reported as the error location.

## Unbalanced Begin and End

### Symptom

Errors such as unexpected `End`, unexpected end of file, or an `Else` in an invalid position.

### Checks

Indent blocks consistently:

```pascal
If IsActive Then
Begin
  WriteLn('Active');
End;
```

Count nested `Begin` and `End` pairs. Review the nearest conditional or loop before the reported location.

## Unexpected End of File

### Common causes

- Missing `End`
- Missing final period
- Unterminated string
- Unterminated comment
- Incomplete procedure or function
- Missing parenthesis

Review the end of the source and the final block terminator.

## Unterminated String

Incorrect:

```pascal
WriteLn('Hello);
```

Correct:

```pascal
WriteLn('Hello');
```

Check for apostrophes inside string literals and verify how the compiler escapes or represents them.

## Type Mismatch

### Possible causes

- Assigning text to a numeric variable
- Assigning multiple characters to `Char`
- Passing the wrong type to a function
- Concatenating a number without conversion
- Comparing incompatible values

Example:

```pascal
Age := '25';
```

Use a compatible value or an appropriate conversion function.

See [Data Types](Data-Types) and [Operators](Operators).

## Incorrect Parameter Count

### Symptom

A procedure or function call has too many or too few arguments.

### Checks

- Confirm the function signature.
- Check the Mystic version.
- Verify parameter order.
- Verify optional-parameter support.
- Check whether a built-in function changed between releases.

## Compilation Appears Successful but No MPX Exists

### Checks

- Review all compiler output.
- Check the compiler exit status.
- Search for the output in another directory.
- Confirm output filename capitalization.
- Check write permissions.
- Determine whether the compiler retained an older output.

```bash
find . -maxdepth 3 -type f -iname 'hello.mpx' -ls
```

## Old Program Still Runs

An older `.mpx` file may remain after a failed compilation.

Compare timestamps:

```bash
ls -l hello.mps hello.mpx
```

Search for duplicate compiled files:

```bash
find /path/to/mystic -type f -iname 'hello.mpx' -ls
```

Confirm which script directory the active theme uses.

## Mystic Cannot Find the Program

### Checks

- Confirm the `.mpx` file exists.
- Confirm the menu command is correct.
- Confirm the menu data matches the program name.
- Check the active theme's script directory.
- Check fallback script directories.
- Verify filename capitalization.
- Remove the extension only when Mystic expects the base program name.

For a `GX` menu command, a typical data value is:

```text
hello
```

## Program Runs but Displays Nothing

### Checks

- Add a simple `WriteLn` at the start.
- Confirm the program is actually being executed.
- Confirm output is not immediately cleared.
- Check display codes and theme behavior.
- Verify the correct `.mpx` file is running.
- Test with plain ASCII text before using ANSI or pipe codes.

## Program Exits Immediately

Add a pause display code where appropriate:

```pascal
WriteLn('|PA');
```

The exact pause behavior depends on the Mystic theme and language configuration.

## Infinite Loop

### Symptoms

- Session appears frozen
- CPU usage increases
- Repeated output continues indefinitely

### Checks

Verify that the loop condition can change:

```pascal
While Count < 10 Do
Begin
  Count := Count + 1;
End;
```

For `Repeat ... Until`, verify that the exit condition eventually becomes true.

Test loop boundaries with small values first.

## Division by Zero

Validate the divisor before division:

```pascal
If Count > 0 Then
Begin
  Result := Total Div Count;
End;
```

Do not depend on logical short-circuit behavior unless it has been verified for the compiler version.

## Incorrect String Comparison

Potential causes:

- Case-sensitive comparison
- Trailing spaces
- Different encodings or code pages
- Input includes control characters
- Input was not normalized

During testing, display delimiters around the value:

```pascal
WriteLn('[' + UserInput + ']');
```

This can expose leading or trailing spaces.

## ANSI or Pipe Codes Display Literally

### Symptoms

The user sees text such as:

```text
|PA
|07
```

### Checks

- Confirm the output routine processes Mystic display codes.
- Confirm the code is valid for the active Mystic version.
- Check whether raw text output was used.
- Confirm the correct pipe character is present.
- Check source encoding.

See [ANSI and Screen Output](ANSI-and-Screen-Output).

## Path Problems on Linux

Common causes include:

- Incorrect capitalization
- Windows-style backslashes
- Relative paths resolved from an unexpected directory
- Missing permissions
- Symbolic-link assumptions

Prefer explicit paths during diagnosis:

```text
/path/to/mystic/themes/default/scripts/hello.mpx
```

## Works on Windows but Not Linux

Compare:

- Filename capitalization
- Path separators
- Line endings
- Source encoding
- Executable permissions
- Current working directory
- Include-file names
- Mystic build version

Do not assume the two installations use identical directories or theme settings.

## Works in One Mystic Version but Not Another

Record both versions and compare:

- Compiler messages
- Built-in function names
- Include syntax
- Data types
- `Case` behavior
- Menu execution behavior
- Script path resolution

Consult [Version Compatibility](Version-Compatibility) and update the page with a specific version note.

## Minimal Reproduction

Reduce the problem to the smallest source file that still fails.

Example:

```pascal
WriteLn('Test');
WriteLn('|PA');
```

Then add one declaration or statement at a time until the failure returns.

This isolates whether the problem is caused by:

- Syntax
- A declaration
- A built-in function
- An include file
- A path
- A runtime integration point

## Diagnostic Build Log

Capture compiler output:

```bash
./mplc hello.mps >hello-build.log 2>&1
printf 'exit_status=%s\n' "$?" >>hello-build.log
```

Record file timestamps:

```bash
ls -l hello.mps hello.mpx >>hello-build.log 2>&1
```

Remove credentials and private paths before sharing logs publicly.

## Support Request Template

```text
Problem summary:

Mystic version:
Operating system:
Compiler path:
Compiler command:
Current directory:
Source filename:
Expected result:
Actual result:
Exact compiler or runtime error:

Relevant source:

Steps already attempted:

Does a minimal program compile? Yes/No
Does the issue occur from a menu? Yes/No
Does the issue occur from command-line execution? Yes/No
```

## Related Pages

- [Compiler Behavior](Compiler-Behavior)
- [Compiler Usage](Compiler-Usage)
- [Installing and Compiling](Installing-and-Compiling)
- [Program Structure](Program-Structure)
- [Data Types](Data-Types)
- [Operators](Operators)
- [Conditional Logic](Conditional-Logic)
- [Loops](Loops)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

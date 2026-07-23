# Compiler Behavior

This page documents how the Mystic Programming Language compiler processes source files and produces executable MPL programs.

> **Documentation status**
>
> Compiler behavior can differ between Mystic releases and operating systems. Treat this page as a working reference until each item has been verified against a specific Mystic version.

## Source and Output Files

MPL source files normally use the `.mps` extension.

```text
hello.mps
```

A successful compilation normally produces an `.mpx` file:

```text
hello.mpx
```

The `.mps` file is the editable source. The `.mpx` file is the compiled program executed by Mystic BBS.

## Basic Compilation

A typical Linux or Unix-like command is:

```bash
./mplc hello.mps
```

When the compiler is in the shell path:

```bash
mplc hello.mps
```

On Windows:

```text
MPLC.EXE hello.mps
```

The exact executable name and command-line options depend on the Mystic release and operating system.

## Working Directory

The compiler may resolve source files, include files, and output paths relative to the current working directory.

Before compiling, confirm the current location:

```bash
pwd
ls -l
```

Using absolute paths can reduce ambiguity:

```bash
/path/to/mystic/mplc /path/to/source/hello.mps
```

Verify where the compiler writes the resulting `.mpx` file.

## Include File Resolution

MPL programs may reference include files. Include resolution can depend on:

- The source file directory
- The current working directory
- Mystic's configured script or include directories
- Compiler-specific search paths
- Filename capitalization on case-sensitive systems

When an include cannot be found, record:

```text
Source file path:
Include statement:
Current working directory:
Compiler location:
Expected include path:
Actual error:
```

Do not assume Linux and Windows resolve include filenames identically.

## Case Sensitivity

Language keywords may be case-insensitive, but filesystem paths and filenames may be case-sensitive on Linux and other Unix-like systems.

These may refer to different files on a case-sensitive filesystem:

```text
Common.inc
common.inc
COMMON.INC
```

Use consistent filename capitalization throughout source code, build commands, and Mystic configuration.

## Compilation Stages

A typical compilation process includes:

1. Reading the `.mps` source file.
2. Resolving include files.
3. Tokenizing and parsing the source.
4. Checking declarations, types, and statement structure.
5. Generating the compiled `.mpx` output.
6. Reporting warnings or errors.

The compiler may stop after the first error or continue and report multiple errors. This behavior should be verified by version.

## Compiler Errors

Compiler errors prevent creation of a valid output file.

Common categories include:

- Unknown identifier
- Missing semicolon
- Unbalanced `Begin` and `End`
- Invalid type assignment
- Incorrect parameter count
- Unknown function or procedure
- Include file not found
- Unexpected end of file
- Invalid token or character

Always correct the earliest reported error first. Later errors may be consequences of the first syntax problem.

## Warnings

Some compiler versions may report warnings that do not stop output generation.

Potential warning categories include:

- Declared but unused variables
- Unreachable statements
- Implicit conversion
- Truncated strings
- Duplicate declarations

Warning support and severity should be documented for each compiler version.

## Output Replacement

When an existing `.mpx` file is present, determine whether the compiler:

- Replaces it only after a successful compilation
- Removes it before compiling
- Leaves the older file when compilation fails
- Writes a temporary output and renames it on success

This matters because an old `.mpx` file may continue to run after a failed build.

Always compare timestamps:

```bash
ls -l hello.mps hello.mpx
```

## Exit Status

On operating systems that support process exit codes, determine whether the compiler returns a nonzero status on failure.

Example:

```bash
./mplc hello.mps
echo $?
```

A reliable nonzero failure status allows the compiler to be used safely in scripts and automated build processes.

Do not assume the exit code is reliable until tested.

## Standard Output and Error Output

Determine whether compiler diagnostics are written to:

- Standard output
- Standard error
- Both
- A separate log file

This affects redirection:

```bash
./mplc hello.mps >build.log 2>&1
```

## File Encoding

Source encoding can affect string literals, comments, ANSI-related text, and special characters.

Verify whether the compiler expects:

- ASCII
- CP437
- Another DOS code page
- UTF-8 without a byte-order mark
- UTF-8 with restrictions

For maximum portability, begin with plain ASCII source unless the target encoding has been verified.

## Line Endings

Compiler behavior may differ for:

- Unix line endings: `LF`
- Windows line endings: `CRLF`

Most modern tools handle both, but this should be tested before documenting cross-platform compatibility.

## Program Size and Resource Limits

Compiler or runtime limits may apply to:

- Source file size
- Compiled program size
- String length
- Number of variables
- Procedure or function count
- Include nesting depth
- Array size
- Recursion depth

Do not publish fixed limits without compiler tests or authoritative documentation.

## Version-Specific Behavior

Compiler behavior may change between Mystic releases. Potential differences include:

- Added or renamed built-in functions
- Changed include syntax
- New data types
- Expanded `Case` support
- Different path resolution
- Updated error messages
- Different output formats

Every verified page should record the Mystic version used.

## Safe Build Workflow

A cautious build process is:

```bash
set -e

src="hello.mps"
out="hello.mpx"

cp -p "$out" "$out.bak" 2>/dev/null || true
./mplc "$src"
test -f "$out"
```

Do not deploy automatically until the compiler exit status and output behavior have been verified.

## Verification Checklist

Record the following for each supported environment:

```text
Mystic version:
Operating system:
Compiler executable:
Compiler path:
Source extension:
Output extension:
Output directory behavior:
Include search behavior:
Keyword case sensitivity:
Filename case sensitivity:
Source encoding:
Line-ending support:
Exit status on success:
Exit status on failure:
Output replacement behavior:
Warnings supported:
Date tested:
Notes:
```

## Related Pages

- [Installing and Compiling](Installing-and-Compiling)
- [Compiler Usage](Compiler-Usage)
- [Troubleshooting](Troubleshooting)
- [Program Structure](Program-Structure)
- [Data Types](Data-Types)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

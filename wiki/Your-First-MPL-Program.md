# Your First MPL Program

This guide creates, compiles, and runs a small Mystic Programming Language program.

The example displays a short message and then pauses before returning control to Mystic BBS.

> **Compatibility note**
>
> MPL syntax and compiler behavior can vary between Mystic releases. Record the Mystic version used when testing this example.

## What You Will Create

Create a source file named:

```text
hello.mps
```

The compiler will generate:

```text
hello.mpx
```

Mystic executes the compiled `.mpx` file rather than the editable `.mps` source file.

## Program Source

Add the following content to `hello.mps`:

```pascal
WriteLn('Hello from Mystic MPL!');
WriteLn('|PA');
```

## How the Program Works

### Displaying text

```pascal
WriteLn('Hello from Mystic MPL!');
```

`WriteLn` displays the supplied text and advances to the next line.

The text is enclosed in single quotation marks. The statement ends with a semicolon.

### Pausing before exit

```pascal
WriteLn('|PA');
```

`|PA` is a Mystic display code that presents the configured pause prompt. This gives the user time to read the output before the script returns to the menu.

The exact prompt appearance depends on the active Mystic theme and language configuration.

## Save the Source File

Save the file as plain text using the `.mps` extension:

```text
hello.mps
```

Confirm that your editor did not add another extension such as:

```text
hello.mps.txt
```

On Linux and other case-sensitive systems, keep the filename capitalization consistent.

## Compile the Program

Run the Mystic MPL compiler against the source file:

```bash
mplc hello.mps
```

Depending on your operating system and installation, the executable may be named differently or require a relative path, for example:

```bash
./mplc hello.mps
```

On Windows:

```text
MPLC.EXE hello.mps
```

A successful compilation should create:

```text
hello.mpx
```

The compiler supplied with Mystic accepts `.mps` source files and produces executable `.mpx` programs.

## Check the Compiled File

On Linux or another Unix-like system:

```bash
ls -l hello.mps hello.mpx
```

On Windows:

```text
dir hello.mps hello.mpx
```

Confirm that:

- `hello.mps` contains your source code.
- `hello.mpx` exists after compilation.
- The `.mpx` timestamp reflects the latest build.

Recompile after every source-code change.

## Install the Program

Place `hello.mpx` in the script directory used by the active Mystic theme or in the default script directory available through theme fallback.

A common location is similar to:

```text
mystic/themes/default/scripts/
```

The exact path depends on your Mystic installation and theme configuration.

Keep the original `hello.mps` source file in a development or source directory when possible. Do not rely on the compiled `.mpx` file as the only copy of your work.

## Run the Program from a Menu

In the Mystic menu editor, add a menu command using:

```text
Command: GX
Data:    hello
```

`GX` executes a Mystic Programming Language program. When no extension is supplied, Mystic resolves the corresponding compiled MPL file.

The expected result is:

```text
Hello from Mystic MPL!
```

Mystic should then display the configured pause prompt and return to the menu.

## Optional Command-Line Test

Mystic can also execute an MPL program from the command line with the `-X` option. This requires a valid Mystic username and password because the program runs in a user session.

Example form:

```bash
./mystic -uUSERNAME -pPASSWORD -xhello
```

Avoid placing real credentials in shell history, shared scripts, documentation, or screenshots. A temporary restricted test account is preferable for command-line testing.

Menu-based testing is generally safer for a first program.

## Expected Result

A successful test confirms that:

1. The source file was saved correctly.
2. The MPL compiler located and compiled the source.
3. The `.mpx` file was created.
4. Mystic found the compiled program.
5. The `GX` menu command executed it.
6. Mystic display codes were processed.
7. Control returned to the menu after the pause.

## Common Problems

### `mplc: command not found`

The compiler is not in your shell path.

Run it from the Mystic directory or supply its full path:

```bash
/path/to/mystic/mplc hello.mps
```

### The compiler cannot find the source file

Confirm your current directory and filename:

```bash
pwd
ls -l hello.mps
```

Also verify filename capitalization.

### No `.mpx` file is created

Review the compiler output for syntax errors. Correct the source and compile it again.

Do not assume compilation succeeded merely because the command returned to the prompt.

### Mystic cannot find `hello`

Check that:

- `hello.mpx` is in the active theme's script directory or an enabled fallback script directory.
- The menu command is `GX`.
- The menu data is `hello` or an appropriate path.
- The filename matches exactly on case-sensitive systems.

### The text appears but there is no pause

Confirm that the second line contains:

```pascal
WriteLn('|PA');
```

Also verify that the active theme and language configuration provide the expected pause behavior.

### Changes do not appear when testing

You may be running an older compiled file.

Recompile and compare timestamps:

```bash
ls -l hello.mps hello.mpx
```

Then confirm that Mystic is loading the same `hello.mpx` file you just built.

## A Slightly Expanded Example

After the basic program works, try adding another line:

```pascal
WriteLn('Hello from Mystic MPL!');
WriteLn('This is my first compiled MPL program.');
WriteLn('|PA');
```

Compile and test it again:

```bash
mplc hello.mps
```

This reinforces the edit, compile, install, and test cycle used throughout MPL development.

## Verification Record

Record the environment where the program was tested:

```text
Mystic version:
Operating system:
Compiler path:
Theme:
Script directory:
Date tested:
Compilation result:
Execution result:
```

Once confirmed, the page can be updated with a verified compatibility note.

## Next Steps

Continue with:

1. [Program Structure](Program-Structure)
2. [Comments](Comments)
3. [Variables](Variables)
4. [Conditional Logic](Conditional-Logic)
5. [Loops](Loops)

For compiler options and directory behavior, see [Compiler Usage](Compiler-Usage).

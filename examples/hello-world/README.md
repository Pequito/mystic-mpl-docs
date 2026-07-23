# Hello World

Status: **Candidate**

This is the first program in the verified MPL examples library. It displays a short message and then invokes Mystic's configured pause prompt.

## Concepts Demonstrated

- Calling `WriteLn`
- Displaying literal text
- Using the Mystic `|PA` pause code
- Compiling an `.mps` source file into an `.mpx` program
- Running an MPL program through a Mystic menu command

## Source

```pascal
WriteLn('Hello from Mystic MPL!');
WriteLn('|PA');
```

The source file is:

```text
hello.mps
```

## Compile

From the directory containing the source file:

```bash
mplc hello.mps
```

When the compiler is not in the shell path:

```bash
/path/to/mystic/mplc hello.mps
```

A successful build should produce:

```text
hello.mpx
```

## Install

Place `hello.mpx` in the script directory used by the active Mystic theme or another script directory available through Mystic's fallback behavior.

A common layout is similar to:

```text
mystic/themes/default/scripts/hello.mpx
```

The exact directory must be confirmed for the local Mystic installation.

## Run from a Mystic Menu

Configure a menu entry with:

```text
Command: GX
Data:    hello
```

## Expected Output

```text
Hello from Mystic MPL!
```

Mystic should then display the configured pause prompt and return to the calling menu.

## Verification Status

This example is currently a **Candidate**. It must not be labeled verified until `verification.md` contains a completed test record.

## Verification Procedure

1. Confirm that `hello.mps` matches the committed source.
2. Remove or rename any older `hello.mpx` file.
3. Compile `hello.mps`.
4. Confirm that a new `hello.mpx` file is created.
5. Record compiler output and exit status when available.
6. Install the resulting `.mpx` file.
7. Run it using the `GX` menu command.
8. Confirm that the output matches this page.
9. Confirm that the pause prompt appears.
10. Confirm that control returns to the menu normally.
11. Complete `verification.md`.
12. Change the status in this file and the wiki index to **Verified**.

## Known Limitations

- The `|PA` display code depends on Mystic's active language and theme configuration.
- Compiler command paths vary by operating system and installation.
- Filename capitalization matters on case-sensitive filesystems.
- This example does not test input, variables, procedures, file access, or user data.

## Related Documentation

- [Your First MPL Program](../../wiki/Your-First-MPL-Program.md)
- [Compiler Behavior](../../wiki/Compiler-Behavior.md)
- [Troubleshooting](../../wiki/Troubleshooting.md)

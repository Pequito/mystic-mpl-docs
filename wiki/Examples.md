# Verified Examples

This library collects small MPL programs that are intended to be compiled, executed, and verified against specific Mystic BBS releases.

An example is not considered verified merely because it looks correct. It must include a completed verification record showing the Mystic version, operating system, compiler, test date, compilation result, and execution result.

## Status Levels

| Status | Meaning |
|---|---|
| **Candidate** | Source exists but has not been tested. |
| **Compiles** | The source compiled successfully, but runtime behavior has not been confirmed. |
| **Verified** | The source compiled and executed successfully in a recorded environment. |
| **Version-specific** | Verified only for the listed Mystic release or platform. |
| **Needs review** | Previously verified, but the result should be retested. |

## Example Index

| Example | Category | Status | Source |
|---|---|---|---|
| Hello World | Beginner | Candidate | [Repository source](https://github.com/Pequito/mystic-mpl-docs/tree/main/examples/hello-world) |

The Hello World example is the first candidate in the library. It should remain marked **Candidate** until a completed verification record is committed.

## Required Files

Each example should use this structure:

```text
examples/
└── example-name/
    ├── README.md
    ├── program.mps
    └── verification.md
```

### `README.md`

The example documentation should include:

- Purpose
- Concepts demonstrated
- Source filename
- Compilation command
- Installation or script path
- Mystic menu command
- Expected output
- Known limitations
- Verification status

### MPL source file

The source file should:

- Be complete rather than a disconnected snippet
- Use a descriptive filename
- Contain only the code needed for the example
- Avoid undocumented functions unless clearly labeled
- Avoid embedded credentials, private paths, or personal data

### `verification.md`

The verification record should include:

```text
Status:
Mystic version:
Operating system:
Architecture:
Compiler executable:
Compiler path:
Compile command:
Compile result:
Runtime method:
Runtime result:
Date tested:
Tested by:
Notes:
```

## Verification Requirements

Before changing an example to **Verified**:

1. Start from the committed `.mps` source.
2. Compile it with the recorded MPL compiler.
3. Confirm that a new `.mpx` file is produced.
4. Record the compiler output and exit status when available.
5. Execute the program through Mystic.
6. Confirm that the actual output matches the documented output.
7. Test return behavior to the calling menu.
8. Record the exact Mystic version and operating system.
9. Commit the completed verification record.
10. Update this index from **Candidate** to **Verified**.

## Adding an Example

Use a lowercase directory name with hyphens:

```text
examples/read-user-input/
examples/basic-condition/
examples/counting-loop/
```

Prefer one concept per beginner example.

## Planned Beginner Examples

- Displaying text
- Reading a menu key
- String variables
- Integer arithmetic
- Basic `If` statement
- `If` and `Else`
- Counting with a `For` loop
- Repeating with a `While` loop
- Writing a procedure
- Returning a value from a function

## Planned Mystic Integration Examples

- Displaying an ANSI file
- Reading current-user information
- Running an MPL program with `GX`
- Reading a text file
- Writing a text file
- Displaying a prompt
- Using pipe codes

## Contribution Standard

A contribution should explain:

1. What the example demonstrates.
2. How to compile it.
3. How to run it.
4. What output to expect.
5. Which Mystic version was tested.
6. Any platform-specific behavior.

See [Contributing](Contributing), [Compiler Behavior](Compiler-Behavior), and [Troubleshooting](Troubleshooting).

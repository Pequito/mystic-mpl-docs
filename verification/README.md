# MPL Verification

This directory contains reproducible verification evidence for Mystic Programming Language documentation.

The goal is to distinguish three separate questions:

1. **Language** — Does this MPL syntax compile?
2. **Compiler** — How does MPLC process, report, and emit compiled programs?
3. **Runtime** — What happens when successfully compiled code executes?

Verification results should be based on a recorded Mystic/MPLC environment and should not be treated as portable to another release until retested.

## Structure

```text
verification/
├── environments/
├── language/
├── compiler/
└── runtime/
```

### environments/

Records the Mystic, MPLC, operating-system, architecture, and build information used for verification.

### language/

Contains focused source cases that verify one MPL language feature at a time.

### compiler/

Contains checks for MPLC behavior such as diagnostics, include resolution, command-line behavior, output handling, and compiled-file behavior.

### runtime/

Contains checks whose important result occurs after successful compilation, such as array-bound behavior, file I/O, record persistence, or string boundary behavior.

## Evidence Rules

- Keep each verification case small.
- Test one primary behavior per source file.
- Do not encode PASS or FAIL into filenames.
- Preserve the exact source that was tested.
- Record both compile and runtime results where applicable.
- Record the exact verification environment.
- Do not commit generated `.mpx` files.
- Do not update Wiki claims until the corresponding result has been reviewed.

## Case Naming

Use stable numeric prefixes:

```text
001-feature.mps
002-feature.mps
003-feature.mps
```

The number controls documentation and execution order. The filename describes the syntax being tested, not the expected result.

## Result States

Use:

```text
NOT RUN
PASS
FAIL
PARTIAL
BLOCKED
```

A compile PASS does not automatically mean runtime behavior is verified.

# Verification Environments

Each verification run should identify the exact Mystic/MPLC environment used.

Environment files provide a stable identifier that result files can reference instead of repeating the full platform description for every case.

## Naming

Use:

```text
mystic-<version>-<build>-<platform>.md
```

Example:

```text
mystic-1.12-a49-linux64.md
```

## Required Information

Record:

```text
Mystic version/build:
MPLC version/build:
MPLC build date:
Operating system:
Architecture:
Compiler path:
Mystic root:
Date verified:
Status:
Notes:
```

Do not mark an environment VERIFIED until the values have been confirmed on the machine used for testing.

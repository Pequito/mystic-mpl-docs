# Program Structure Verification

This is the first MPL language verification topic.

The objective is to establish the smallest valid source structures accepted by the target MPLC build before relying on more complex language tests.

## Environment

Initial target:

```text
verification/environments/mystic-1.12-a49-linux64.md
```

The environment is currently pending machine verification.

## Cases

| Case | Question |
|---|---|
| `001-statement-only.mps` | Does a statement-only MPL source compile? |
| `002-main-block.mps` | Does a Pascal-style main `Begin` / `End.` block compile? |
| `003-program-header.mps` | Is `Program Name;` accepted by MPLC? |
| `004-final-end-semicolon.mps` | Is a final `End;` accepted instead of `End.`? |

## Procedure

For each case:

1. Verify the MPLC environment.
2. Compile the unchanged source.
3. Capture the exact compiler output.
4. Record the compiler exit status.
5. Confirm whether a new `.mpx` was generated.
6. If compilation succeeds, execute the resulting program through an appropriate Mystic test path.
7. Record the observed runtime result.
8. Update `RESULTS.md`.

Do not change a case after testing it without recording why the source changed.

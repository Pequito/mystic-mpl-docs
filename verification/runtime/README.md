# Runtime Verification

This area records behavior that can only be determined after an MPL program compiles successfully and executes.

Examples include:

- Array out-of-range behavior
- Sized-string truncation
- Record persistence
- File I/O behavior
- Comparison behavior
- Runtime limits

A successful compile is not sufficient evidence for a runtime claim.

Every runtime result must identify the verification environment and the exact source case used.

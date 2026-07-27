# MPL Cross-Version Change Index

This page tracks Mystic changes that directly affect MPL source code, compiler behavior, runtime behavior, or documentation.

Status: **Initial index**

## Language Syntax

| Feature | Introduced or changed | Notes | Detailed file |
|---|---|---|---|
| Pascal-style syntax modernization | Mystic 1.10 | Replaced several older MPL control-flow forms and expanded modern Pascal-style syntax | [Mystic 1.10](Mystic-1.10.md) |
| Local declarations | Mystic 1.10 | Added local variables and related declaration improvements | [Mystic 1.10](Mystic-1.10.md) |
| Nested procedures and functions | Mystic 1.10 | Added local routine definitions | [Mystic 1.10](Mystic-1.10.md) |
| Recursive procedures | Mystic 1.10 | Added documented procedure recursion | [Mystic 1.10](Mystic-1.10.md) |
| `Var` parameters | Mystic 1.10 | Added reference-style procedure and function parameters | [Mystic 1.10](Mystic-1.10.md) |
| Multidimensional arrays | Mystic 1.10 | Added arrays with multiple dimensions | [Mystic 1.10](Mystic-1.10.md) |
| Block comments | Mystic 1.10 | Added Pascal-style `(* ... *)` comments | [Mystic 1.10](Mystic-1.10.md) |
| Standard expression precedence | Mystic 1.10 | Added modern precedence and parentheses behavior | [Mystic 1.10](Mystic-1.10.md) |
| Record function results | Mystic 1.11 | Functions can return record types | [Mystic 1.11](Mystic-1.11.md) |
| Arrays and nested records in records | Mystic 1.12 | Expanded complex record support | [Mystic 1.12](Mystic-1.12.md) |

## Control Flow

| Feature | Introduced or changed | Notes | Detailed file |
|---|---|---|---|
| Modern `Else If` form | Mystic 1.10 | Replaced legacy `ElseIf` usage | [Mystic 1.10](Mystic-1.10.md) |
| Modern loop blocks | Mystic 1.10 | Replaced older terminators such as `FEnd` and `WEnd` | [Mystic 1.10](Mystic-1.10.md) |
| `Case` enhancements | Mystic 1.10 | Expanded Pascal-style `Case` support | [Mystic 1.10](Mystic-1.10.md) |
| `Break` and `Continue` | Mystic 1.10 | Added loop-control statements | [Mystic 1.10](Mystic-1.10.md) |

## Functions and Procedures

| Symbol or area | Version | Change | Detailed file |
|---|---|---|---|
| Function result may be ignored | Mystic 1.10 | Functions can be called for side effects without storing the result | [Mystic 1.10](Mystic-1.10.md) |
| File-related names | Mystic 1.10 | Several older names were modernized, including `fExist` to `FileExist` | [Mystic 1.10](Mystic-1.10.md) |
| `DispFile` result behavior | Mystic 1.10 | Boolean result reduced the need for older file-status helpers | [Mystic 1.10](Mystic-1.10.md) |

## Compiler Behavior

| Area | Version | Change | Detailed file |
|---|---|---|---|
| Expression parsing | Mystic 1.10 | Standardized precedence and parentheses | [Mystic 1.10](Mystic-1.10.md) |
| Local scope | Mystic 1.10 | Compiler accepts local declarations and nested routines | [Mystic 1.10](Mystic-1.10.md) |
| Complex records | Mystic 1.11–1.12 | Record returns, arrays in records, and nested records expanded | [Mystic 1.11](Mystic-1.11.md), [Mystic 1.12](Mystic-1.12.md) |

## Documentation Follow-Up

When a change is added here, review these wiki pages as applicable:

- `MPL-Syntax.md`
- `Variables.md`
- `Procedures.md`
- `Functions.md`
- `Conditional-Logic.md`
- `Loops.md`
- `Arrays.md`
- `Records.md`
- `Compiler-Usage.md`
- `Version-Compatibility.md`

## Verification Rule

An entry in this index means the upstream change has been identified. It does not mean the behavior has been compiled or runtime tested by this project. Test results should be recorded in the corresponding version file.

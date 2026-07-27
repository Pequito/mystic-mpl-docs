# Mystic 1.10 Changes

Status: **Initial MPL summary**

Mystic 1.10 is a major compatibility point for MPL because it modernized and expanded the language syntax.

## MPL Language Changes

| Type | Change | Documentation impact | Verification |
|---|---|---|---|
| `+` | Standard mathematical precedence and parentheses | Update operators and expression examples | Source identified |
| `+` | Local variable declarations | Update variables, procedures, and functions | Source identified |
| `+` | Variables initialized during declaration | Update variables and local-declaration examples | Source identified |
| `+` | `Var` reference parameters | Update procedures and functions | Source identified |
| `+` | Nested procedures and functions | Update procedure and function scope documentation | Source identified |
| `+` | Recursive procedures | Add recursion guidance and safety notes | Source identified |
| `+` | Multidimensional arrays | Update array documentation | Source identified |
| `+` | Pascal-style block comments | Update comments and syntax pages | Source identified |
| `+` | Functions may be called without using their result | Document ignored return values and side effects | Source identified |
| `+` | Expanded `Case` behavior | Update conditional-logic documentation | Source identified |
| `+` | Loop-control support including `Break` and `Continue` | Update loops documentation | Source identified |

## Syntax Modernization

Mystic 1.10 standardized several older MPL forms toward modern Pascal-style syntax.

Review older source for forms such as:

```text
ElseIf
EndIf
FEnd
WEnd
```

Modern documentation should prefer:

```pascal
Else If Condition Then
Begin
  // Statements
End;
```

and normal `Begin` / `End` loop blocks.

## Built-In Routine Changes

Known areas requiring detailed extraction include renamed file routines and functions whose return behavior changed.

Examples commonly associated with this release include:

| Older name or behavior | Newer name or behavior | Impact |
|---|---|---|
| `fExist` | `FileExist` | Older source may require migration |
| `fErase` | `FileErase` | Older source may require migration |
| `fCopy` | `FileCopy` | Older source may require migration |
| `CLS` | `ClrScr` | Update examples and compatibility notes |
| Separate missing-display-file checks | Boolean result from `DispFile` | Returned result can indicate whether a file displayed |

These entries should be checked against the exact alpha build and official `WHATSNEW.TXT` before being marked fully extracted.

## Wiki Pages Affected

- `MPL-Syntax.md`
- `Comments.md`
- `Variables.md`
- `Operators.md`
- `Conditional-Logic.md`
- `Loops.md`
- `Arrays.md`
- `Procedures.md`
- `Functions.md`
- `Compiler-Usage.md`
- `Version-Compatibility.md`

## Verification Tasks

- [ ] Identify the exact 1.10 alpha build for each change
- [ ] Compile local declarations
- [ ] Compile declaration initialization
- [ ] Test `Var` parameters
- [ ] Compile nested procedures and functions
- [ ] Test recursive procedures
- [ ] Compile multidimensional arrays
- [ ] Test block comments
- [ ] Test ignored function results
- [ ] Test `Case`, `Break`, and `Continue`
- [ ] Verify renamed built-in routines
- [ ] Record runtime behavior

## Sources

- Official changes page: https://wiki.mysticbbs.com/doku.php?id=whats_new_110
- History index: https://wiki.mysticbbs.com/doku.php?id=whats_new_intro
- Historical source repository: https://github.com/fidosoft/mysticbbs

## Notes

Mystic 1.10 should be treated as a language-version boundary when documenting or migrating older MPL source.

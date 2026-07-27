# Mystic 1.11 Changes

Status: **Initial MPL summary**

## MPL-Relevant Changes

Mystic 1.11 expanded MPL record handling, including support for records as function result types.

```pascal
Type
  UserSummary = Record
    Name : String[30];
    Calls : LongInt;
  End;

Function GetDefaultUser : UserSummary;
Var
  User : UserSummary;
Begin
  User.Name := 'Guest';
  User.Calls := 0;

  GetDefaultUser := User;
End;
```

## Documentation Impact

Review and update:

- `Functions.md`
- `Records.md`
- `Data-Types.md`
- `Variables.md`
- `Compiler-Usage.md`
- `Version-Compatibility.md`

## Compatibility Notes

- Record-returning functions should not be presented as portable to older Mystic releases.
- Record assignment, nested fields, and large records should be tested with the exact compiler build.
- Records containing arrays may depend on later 1.12 compiler improvements.

## Verification Tasks

- [ ] Identify the exact 1.11 alpha build that added record results
- [ ] Compile a function returning a small record
- [ ] Assign the returned record to a variable
- [ ] Pass the returned record to another routine
- [ ] Test records containing strings
- [ ] Test interaction with nested records
- [ ] Record compiler and runtime results

## Additional Extraction Needed

- Built-in functions added or renamed
- Compiler diagnostics and fixes
- Runtime-variable changes
- User, message-base, file-base, and configuration record changes
- Platform-specific MPL behavior

## Sources

- Official changes page: https://wiki.mysticbbs.com/doku.php?id=whats_new_111
- History index: https://wiki.mysticbbs.com/doku.php?id=whats_new_intro
- Historical source repository: https://github.com/fidosoft/mysticbbs

## Notes

Summarize relevant upstream entries rather than copying the complete release notes.

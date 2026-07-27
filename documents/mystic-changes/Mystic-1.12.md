# Mystic 1.12 Changes

Status: **Initial MPL summary**

Mystic 1.12 continues to expand and correct MPL compiler behavior. Alpha-build differences should be recorded precisely because a feature or fix may apply only after a particular build.

## MPL-Relevant Changes

Known areas include:

| Type | Change | Documentation impact | Verification |
|---|---|---|---|
| `+` | Arrays inside records | Update records and arrays documentation | Source identified |
| `+` | Nested records | Update records documentation | Source identified |
| `!` | Ongoing compiler and runtime fixes | Add exact alpha-build notes to affected examples | Extraction required |

Example structure:

```pascal
Type
  ScoreRecord = Record
    Name : String[30];
    Scores : Array[1..5] of Integer;
  End;
```

Nested-record structure:

```pascal
Type
  AddressRecord = Record
    City : String[30];
    State : String[2];
  End;

  UserRecord = Record
    Name : String[30];
    Address : AddressRecord;
  End;
```

## Documentation Impact

Review and update:

- `Variables.md`
- `Arrays.md`
- `Records.md`
- `Functions.md`
- `Data-Types.md`
- `Compiler-Usage.md`
- `Version-Compatibility.md`

## Alpha-Build Tracking

For every extracted entry, record:

```text
Mystic version: 1.12
Alpha build:
Change marker:
Affected compiler or runtime:
First known working build:
Source:
Verification result:
```

Do not attribute a change to all of Mystic 1.12 when the source identifies a specific alpha build.

## Compatibility Notes

- Complex records may compile differently across 1.12 alpha builds.
- Record declaration initialization remains more restricted than scalar initialization.
- Arrays and nested records should be tested before examples are marked verified.
- Current documentation should identify the MPLC build used for every complex-record test.

## Verification Tasks

- [ ] Extract every MPLC-related 1.12 entry by alpha build
- [ ] Compile a record containing an array
- [ ] Compile a record containing another record
- [ ] Test record assignment
- [ ] Test records passed to procedures and functions
- [ ] Test records returned from functions
- [ ] Test `FillChar` and `SizeOf` with complex records
- [ ] Record 32-bit and 64-bit differences when applicable
- [ ] Update the MPL cross-version index

## Additional Extraction Needed

- New built-in functions and procedures
- Renamed or removed routines
- Compiler error-message changes
- Runtime-variable and record-field changes
- File, message-base, and user-record behavior changes
- Platform-specific changes

## Sources

- Official changes page: https://wiki.mysticbbs.com/doku.php?id=whats_new_112
- History index: https://wiki.mysticbbs.com/doku.php?id=whats_new_intro
- Historical source repository: https://github.com/fidosoft/mysticbbs

## Notes

Summarize relevant upstream entries rather than copying the complete release notes. Preserve exact symbol names, alpha versions, and compiler diagnostics where necessary.

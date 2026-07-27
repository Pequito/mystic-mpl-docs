# Mystic 1.11 Changes

Status: **Documented — needs compiler and runtime verification**

## Release Summary

Mystic 1.11 extended the modernized MPL environment introduced in 1.10. Its most important scripting changes involved richer record handling, records returned from functions, nested records and arrays, and additional date and timing capabilities. Mystic software development also continued across servers, configuration, messaging, files, utilities, and platform support.

## Mystic Software Changes

Mystic 1.11 continued refining the core BBS after the large 1.10 transition. SysOps should expect changes across:

- Message and file processing
- Networked message handling
- Server and connection behavior
- Configuration and maintenance tools
- Display and prompt handling
- Platform-specific reliability

The exact maintenance behavior can vary by build, so production systems should record the specific 1.11 build rather than only the major version.

## MPL Changes

### Functions Returning Records

MPL functions could return a user-defined record type.

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

This allowed several related values to be returned as one result instead of requiring multiple global or `Var` parameters.

### Nested Records

Record types could contain other record types, enabling more structured data models.

```pascal
Type
  AddressInfo = Record
    City : String[30];
    State : String[2];
  End;

  UserInfo = Record
    Name : String[30];
    Address : AddressInfo;
  End;
```

### Arrays in Records

Records could contain arrays, allowing a single record to hold lists or multidimensional data.

```pascal
Type
  ScoreRecord = Record
    Name : String[30];
    Scores : Array[1..5] of Integer;
  End;
```

Complex combinations should be tested with the exact compiler because record layout and assignment behavior can be version-sensitive.

### Date and Timing Support

Mystic 1.11 added or expanded date and timing functionality available to scripts. This supported more precise runtime measurements and date manipulation than older MPL environments.

Scripts using packed dates, elapsed time, or timers should record the exact function names, units, and return types verified with the target build.

## MPL Compatibility Impact

Most 1.10 source should remain conceptually compatible, but complex records require careful testing.

Potential problem areas include:

- Assigning one record to another
- Returning large records from functions
- Records containing arrays
- Nested records
- Passing records through value or `Var` parameters
- Clearing records with `FillChar`
- Differences between 32-bit and 64-bit builds

A successful compile does not guarantee that record data is laid out or copied as expected at runtime.

## MPY and Python Changes

**MPY was not available in Mystic 1.11.**

The embedded Python engine and `.mpy` script format were introduced during Mystic 1.12 development.

## Upgrade Actions

1. Back up MPL source and all Mystic data touched by record-writing scripts.
2. Recompile existing 1.10 source with the target 1.11 compiler.
3. Add isolated tests for record assignment and function returns.
4. Test records containing arrays and nested records.
5. Verify date and timing units before using them in production logic.
6. Test every script that writes user, message, file, or configuration records.

## Documentation Impact

- `wiki/Data-Types.md`
- `wiki/Variables.md`
- `wiki/Functions.md`
- `wiki/Procedures.md`
- `wiki/User-Data.md`
- `wiki/Version-Compatibility.md`

## Verification Record

```text
Mystic version/build: 1.11
Operating system:
Architecture:
MPLC version:
Record return test:
Nested record test:
Array-in-record test:
Record assignment test:
Date function test:
Timer function test:
Runtime result:
Notes:
```

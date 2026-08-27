# Records

Records group several related fields into one structured MPL data type. Unlike an array, whose elements share one type, a record can contain fields with different data types.

Records are useful when several values describe one logical item, such as a user entry, menu item, message summary, configuration record, or application state.

> **Documentation status**
>
> Basic record declaration and field access are established MPL features. Record capabilities expanded significantly across Mystic 1.10, 1.11, and 1.12, so advanced uses such as record parameters, record-returning functions, arrays inside records, arrays of records, and embedded records should be matched to the target Mystic/MPLC version.

## Basic Record Declaration

Define a record type in a `Type` section:

```pascal
Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;
```

Then declare a variable using that type:

```pascal
Var
  UserInfo : TUserInfo;
```

The record type describes the structure. The variable stores one instance of that structure.

## Record Fields

Each value inside a record is called a field.

In this record:

```pascal
Type
  TUserInfo = Record
    Name     : String[30];
    Level    : Integer;
    IsActive : Boolean;
  End;
```

the fields are:

```text
Name
Level
IsActive
```

Each field has its own data type.

## Accessing Fields

Use a period between the record variable and field name:

```pascal
UserInfo.Name := 'Example User';
UserInfo.Level := 20;
UserInfo.IsActive := True;
```

Read fields the same way:

```pascal
WriteLn(UserInfo.Name);
WriteLn(Int2Str(UserInfo.Level));

If UserInfo.IsActive Then
  WriteLn('Active');
```

General form:

```text
RecordVariable.FieldName
```

## Complete Basic Example

```pascal
Type
  TUserInfo = Record
    Name     : String[30];
    Level    : Integer;
    IsActive : Boolean;
  End;

Var
  UserInfo : TUserInfo;

Begin
  UserInfo.Name := 'Example User';
  UserInfo.Level := 20;
  UserInfo.IsActive := True;

  WriteLn('Name: ' + UserInfo.Name);
  WriteLn('Level: ' + Int2Str(UserInfo.Level));

  If UserInfo.IsActive Then
    WriteLn('Status: Active')
  Else
    WriteLn('Status: Inactive');
End.
```

## Why Use a Record?

Without a record, related data may be stored in separate variables:

```pascal
Var
  UserName     : String[30];
  UserLevel    : Integer;
  UserIsActive : Boolean;
```

A record groups those values:

```pascal
Type
  TUserInfo = Record
    Name     : String[30];
    Level    : Integer;
    IsActive : Boolean;
  End;

Var
  UserInfo : TUserInfo;
```

This makes the relationship explicit:

```pascal
UserInfo.Name
UserInfo.Level
UserInfo.IsActive
```

## Record Naming

A clear convention is to prefix custom record types with `T`:

```pascal
Type
  TMenuItem = Record
    Key   : Char;
    Text  : String[40];
    Level : Integer;
  End;
```

The prefix is a convention, not a documented MPL requirement.

Choose names that make the distinction between a type and a variable obvious:

```pascal
Type
  TMenuItem = Record
    Key  : Char;
    Text : String[40];
  End;

Var
  MenuItem : TMenuItem;
```

## Field Types

Record fields can use normal MPL data types.

```pascal
Type
  TExample = Record
    Enabled : Boolean;
    Key     : Char;
    Name    : String[40];
    Count   : Byte;
    Level   : Integer;
    Total   : LongInt;
    Ratio   : Real;
  End;
```

The exact supported combinations should be compiler-tested when using less common or version-sensitive types.

See [Data Types](Data-Types).

## Records Containing Arrays

Records can contain array fields.

```pascal
Type
  TScoreSet = Record
    Name   : String[30];
    Scores : Array[1..5] of Integer;
  End;

Var
  Player : TScoreSet;
```

Use the record field first, followed by the array index:

```pascal
Player.Name := 'Example';
Player.Scores[1] := 90;
Player.Scores[2] := 95;
```

Read an element:

```pascal
WriteLn(Int2Str(Player.Scores[1]));
```

Mystic 1.11 included a fix for multidimensional arrays inside records, so complex array fields are version-sensitive.

See [Arrays](Arrays).

## Multidimensional Arrays Inside Records

Example structure:

```pascal
Type
  TGridData = Record
    Name : String[30];
    Grid : Array[1..10, 1..5] of String[9];
  End;

Var
  Data : TGridData;
```

Element access uses both the record field and array indexes:

```pascal
Data.Grid[1, 1] := 'ABC123';
WriteLn(Data.Grid[1, 1]);
```

Because Mystic 1.11 specifically corrected multidimensional-array behavior within records, this should be part of compiler verification when older versions are supported.

## Arrays of Records

Mystic 1.10 added arrays of records.

```pascal
Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;

Var
  Users : Array[1..10] of TUserInfo;
```

Access an individual field through the array element:

```pascal
Users[1].Name := 'Alpha';
Users[1].Level := 10;

Users[2].Name := 'Bravo';
Users[2].Level := 20;
```

Loop through the records:

```pascal
Var
  Index : Integer;

Begin
  For Index := 1 To 10 Do
  Begin
    WriteLn(Users[Index].Name);
  End;
End.
```

This pattern is useful for small fixed collections of structured values.

## Record-to-Record Assignment

Mystic 1.10 added support for assigning compatible base record variables to one another.

Conceptual example:

```pascal
Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;

Var
  SourceUser : TUserInfo;
  TargetUser : TUserInfo;

Begin
  SourceUser.Name := 'Example';
  SourceUser.Level := 20;

  TargetUser := SourceUser;
End.
```

After assignment, `TargetUser` should contain the copied field values.

Record assignment should be tested when records contain arrays or other complex fields.

## Record Variables Are Not Declaration-Time Initialized

Do not treat a record variable like a scalar declaration with a default value.

Avoid assuming this form is valid:

```pascal
Var
  UserInfo : TUserInfo = ...;
```

Mystic 1.12 explicitly corrected MPLC so attempts to assign a default value to a record variable produce a syntax error rather than compiling incorrectly.

Initialize fields in executable code instead:

```pascal
Begin
  UserInfo.Name := '';
  UserInfo.Level := 0;
  UserInfo.IsActive := False;
End.
```

A helper procedure or function can reduce repetitive initialization.

## Initialization Procedure

```pascal
Procedure ClearUser(Var UserInfo : TUserInfo);
Begin
  UserInfo.Name := '';
  UserInfo.Level := 0;
  UserInfo.IsActive := False;
End;
```

Then:

```pascal
ClearUser(UserInfo);
```

Passing records with `Var` requires Mystic 1.11 or a compatible later build.

## Passing Records to Procedures

Mystic 1.11 added the ability to pass records to procedures by value and by reference.

### By Value

```pascal
Procedure ShowUser(UserInfo : TUserInfo);
Begin
  WriteLn(UserInfo.Name);
  WriteLn(Int2Str(UserInfo.Level));
End;
```

The procedure receives a value copy of the record.

Use:

```pascal
ShowUser(UserInfo);
```

Changes made to a by-value parameter should not be relied on to modify the caller's record.

### By Reference with `Var`

Use `Var` when the procedure should modify the original record:

```pascal
Procedure SetLevel(Var UserInfo : TUserInfo; NewLevel : Integer);
Begin
  UserInfo.Level := NewLevel;
End;
```

Call it:

```pascal
SetLevel(UserInfo, 50);
```

After the call, the original `UserInfo.Level` is changed.

## Choosing Value or `Var`

Use a value parameter when the routine only needs to inspect the record:

```pascal
Procedure DisplayUser(UserInfo : TUserInfo);
```

Use `Var` when the routine should modify the caller's record:

```pascal
Procedure UpdateUser(Var UserInfo : TUserInfo);
```

For large records, also verify the performance and memory behavior of value parameters with the target compiler.

## Functions Returning Records

Mystic 1.11 added record types as function results.

Example:

```pascal
Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;

Function NewUserInfo : TUserInfo;
Var
  ResultInfo : TUserInfo;
Begin
  ResultInfo.Name := 'None';
  ResultInfo.Level := 0;

  NewUserInfo := ResultInfo;
End;
```

Use the returned record:

```pascal
Var
  UserInfo : TUserInfo;

Begin
  UserInfo := NewUserInfo;

  WriteLn(UserInfo.Name);
End.
```

This is a useful way to build a complete record in one routine.

## Embedded Records

Mystic 1.12 added embedded records, allowing one record type to contain another record type directly.

```pascal
Type
  TAddress = Record
    City  : String[30];
    State : String[20];
  End;

Type
  TUserInfo = Record
    Name    : String[30];
    Address : TAddress;
  End;

Var
  UserInfo : TUserInfo;
```

Access nested fields with repeated period notation:

```pascal
UserInfo.Name := 'Example User';
UserInfo.Address.City := 'Albuquerque';
UserInfo.Address.State := 'New Mexico';
```

Read them the same way:

```pascal
WriteLn(UserInfo.Address.City);
```

Embedded records are a Mystic 1.12-era feature and should not be presented as portable to 1.10 or 1.11 without testing.

## Records Containing Arrays and Embedded Records

A larger structured type can combine features:

```pascal
Type
  TAddress = Record
    City : String[30];
    Zip  : String[10];
  End;

Type
  TUserInfo = Record
    Name     : String[30];
    Address  : TAddress;
    Scores   : Array[1..5] of Integer;
    IsActive : Boolean;
  End;
```

This is convenient, but each additional layer increases compiler-version sensitivity and memory use.

Test complex structures with the exact MPLC build used for deployment.

## Local Record Variables

Modern MPL supports local variables inside procedures and functions.

A local variable can conceptually use a record type defined earlier:

```pascal
Procedure BuildUser;
Var
  UserInfo : TUserInfo;
Begin
  UserInfo.Name := 'Temporary';
  UserInfo.Level := 10;
End;
```

Local records should be compiler-tested when they contain arrays, embedded records, or large strings.

## Record Types and Scope

Define a record type before declaring variables that use it.

Correct order:

```pascal
Type
  TPoint = Record
    X : Integer;
    Y : Integer;
  End;

Var
  Position : TPoint;
```

Do not declare a variable using a type that has not yet been defined.

## Using Constants with Records

Constants make record logic easier to maintain.

```pascal
Const
  MinimumLevel = 20;

Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;
```

Then:

```pascal
If UserInfo.Level >= MinimumLevel Then
  WriteLn('Access granted');
```

See [Constants](Constants).

## Comparing Record Fields

Compare individual fields normally:

```pascal
If UserInfo.Level >= 20 Then
  WriteLn('Validated');

If UserInfo.Name = 'Sysop' Then
  WriteLn('Sysop account');
```

Do not assume that complete records can be compared with `=` or another comparison operator unless the exact compiler behavior is verified.

Prefer explicit field comparisons when equality matters.

## Searching an Array of Records

```pascal
Const
  MaxUsers = 5;

Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;

Var
  Users : Array[1..MaxUsers] of TUserInfo;
  Index : Integer;
  Found : Boolean = False;

Begin
  Users[1].Name := 'Alpha';
  Users[2].Name := 'Bravo';
  Users[3].Name := 'Charlie';
  Users[4].Name := 'Delta';
  Users[5].Name := 'Echo';

  For Index := 1 To MaxUsers Do
  Begin
    If Users[Index].Name = 'Charlie' Then
    Begin
      Found := True;
      Break;
    End;
  End;

  If Found Then
    WriteLn('Record found');
End.
```

## Updating an Array of Records

```pascal
For Index := 1 To MaxUsers Do
Begin
  If Users[Index].Level < 10 Then
    Users[Index].Level := 10;
End;
```

This modifies each matching record in place because the array element itself is being accessed directly.

## Copying One Array Element to Another

When the record types are compatible:

```pascal
Users[2] := Users[1];
```

This combines array-of-record support with same-type record assignment. Verify complex fields when using this pattern.

## `SizeOf` and Records

Mystic 1.10 added the `SizeOf` function, which can be useful when investigating record storage.

Conceptual use:

```pascal
WriteLn(Int2Str(SizeOf(UserInfo)));
```

The exact return type and behavior should be verified for the target compiler, especially for:

- Strings
- Arrays inside records
- Embedded records
- Platform or architecture differences

Do not assume a record's memory layout matches Free Pascal, C, or an on-disk Mystic record format.

## Records and File I/O

A custom MPL record can also be stored in a binary data file. The file then contains one or more serialized record values.

A custom record file is **not** automatically compatible with Mystic's internal user, message, file-base, configuration, or other database files. The record declaration used by the MPL program must match the data that was written.

### Example Record File

Define a record:

```pascal
Type
  TUserInfo = Record
    Name     : String[30];
    Level    : Integer;
    IsActive : Boolean;
  End;
```

A file containing three values of this record can be visualized as:

```text
users.dat
│
├── Record 1
│   ├── Name      : String[30]
│   ├── Level     : Integer
│   └── IsActive  : Boolean
│
├── Record 2
│   ├── Name      : String[30]
│   ├── Level     : Integer
│   └── IsActive  : Boolean
│
└── Record 3
    ├── Name      : String[30]
    ├── Level     : Integer
    └── IsActive  : Boolean
```

The actual file is binary. The diagram shows the logical record layout rather than literal text stored in the file.

### Writing a Record File

Late Mystic 1.10 added `fWriteRec` for writing an entire record to an opened MPL `File`.

Example:

```pascal
Type
  TUserInfo = Record
    Name     : String[30];
    Level    : Integer;
    IsActive : Boolean;
  End;

Var
  DataFile : File;
  UserInfo : TUserInfo;

Begin
  UserInfo.Name := 'Example User';
  UserInfo.Level := 20;
  UserInfo.IsActive := True;

  fAssign(DataFile, 'users.dat', 66);
  fReWrite(DataFile);

  If IoResult <> 0 Then
  Begin
    WriteLn('Unable to create users.dat');
    Exit;
  End;

  fWriteRec(DataFile, UserInfo);

  If IoResult <> 0 Then
    WriteLn('Unable to write record');

  fClose(DataFile);
End.
```

This creates or rewrites `users.dat` and writes one complete `TUserInfo` record.

### Writing Multiple Records

The same open file can receive additional records:

```pascal
UserInfo.Name := 'Alpha';
UserInfo.Level := 10;
UserInfo.IsActive := True;
fWriteRec(DataFile, UserInfo);

UserInfo.Name := 'Bravo';
UserInfo.Level := 25;
UserInfo.IsActive := True;
fWriteRec(DataFile, UserInfo);

UserInfo.Name := 'Charlie';
UserInfo.Level := 50;
UserInfo.IsActive := False;
fWriteRec(DataFile, UserInfo);
```

Conceptually:

```text
users.dat
├── Alpha
├── Bravo
└── Charlie
```

Each entry is stored using the same `TUserInfo` record structure.

### Reading a Record File

Use `fReset` to open an existing file and `fReadRec` to read a complete record.

```pascal
Type
  TUserInfo = Record
    Name     : String[30];
    Level    : Integer;
    IsActive : Boolean;
  End;

Var
  DataFile : File;
  UserInfo : TUserInfo;

Begin
  fAssign(DataFile, 'users.dat', 66);
  fReset(DataFile);

  If IoResult <> 0 Then
  Begin
    WriteLn('Unable to open users.dat');
    Exit;
  End;

  fReadRec(DataFile, UserInfo);

  If IoResult = 0 Then
  Begin
    WriteLn('Name: ' + UserInfo.Name);
    WriteLn('Level: ' + Int2Str(UserInfo.Level));

    If UserInfo.IsActive Then
      WriteLn('Status: Active')
    Else
      WriteLn('Status: Inactive');
  End;

  fClose(DataFile);
End.
```

This example reads the first record from the file into `UserInfo`.

Additional sequential reads can be used to process additional records. End-of-file handling should be tested and documented with the target MPLC build before using a production loop.

### Record Size

Mystic 1.10 added `SizeOf`, which can be used when working with record storage:

```pascal
WriteLn('Record size: ' + Int2Str(SizeOf(UserInfo)));
```

For lower-level access, `fRead` and `fWrite` can address individual values or blocks using explicit sizes.

For example, the 1.10 file-I/O redesign documents these storage sizes:

```text
Byte, Char    = 1 byte
Integer, Word = 2 bytes
LongInt       = 4 bytes
String[n]     = n + 1 bytes
```

When an entire compatible record can be read or written with `fReadRec` / `fWriteRec`, that is usually clearer than manually reproducing every field size.

### Historical 1.10 Note

During the early Mystic 1.10 parser/file-I/O redesign, the old `fReadRec` and `fWriteRec` functions were temporarily removed in favor of `fRead` and `fWrite`.

Later in the 1.10 development cycle, `fReadRec` and `fWriteRec` were added back so complete records and arrays of records could be handled directly.

For that reason, code using these functions should be associated with the exact Mystic 1.10 alpha/final build or a later compatible release.

### Record File Compatibility

A record file is only useful when the reader expects the same layout that the writer used.

Changing the record definition can change the binary layout.

For example, changing:

```pascal
Type
  TUserInfo = Record
    Name  : String[30];
    Level : Integer;
  End;
```

to:

```pascal
Type
  TUserInfo = Record
    Name     : String[50];
    Level    : Integer;
    IsActive : Boolean;
  End;
```

changes the stored record structure.

An older `users.dat` file should therefore not be assumed to remain compatible after the record definition changes.

For persistent application data, document:

- Record type name
- Field order
- Field types
- String lengths
- Record size
- File format version
- Mystic version
- MPLC version

Example metadata:

```text
File: users.dat
Format version: 1
Record type: TUserInfo
Record size: verified with SizeOf
Mystic version:
MPLC version:
Architecture:
Date verified:
```

See [File Handling](File-Handling) for the broader MPL file API.

## Records and Mystic Runtime Data

Mystic exposes many BBS-specific values and records through built-ins and runtime variables.

Those integration records should be documented separately from the core MPL `Record` language feature.

Examples of separate integration topics include:

- User data
- Message-base data
- File-base data
- Configuration data
- Network data

Keeping these topics separate prevents the language reference from becoming tied to one specific Mystic database structure.

## Complete Example

```pascal
Const
  MaxUsers = 3;
  MinimumLevel = 20;

Type
  TUserInfo = Record
    Name     : String[30];
    Level    : Integer;
    IsActive : Boolean;
  End;

Var
  Users : Array[1..MaxUsers] of TUserInfo;
  Index : Integer;

Procedure SetUser(
  Var UserInfo : TUserInfo;
  Name : String;
  Level : Integer;
  Active : Boolean
);
Begin
  UserInfo.Name := Name;
  UserInfo.Level := Level;
  UserInfo.IsActive := Active;
End;

Function HasAccess(UserInfo : TUserInfo) : Boolean;
Begin
  HasAccess := UserInfo.IsActive And
               (UserInfo.Level >= MinimumLevel);
End;

Procedure DisplayUser(UserInfo : TUserInfo);
Begin
  WriteLn('Name: ' + UserInfo.Name);
  WriteLn('Level: ' + Int2Str(UserInfo.Level));

  If HasAccess(UserInfo) Then
    WriteLn('Access: Granted')
  Else
    WriteLn('Access: Denied');
End;

Begin
  SetUser(Users[1], 'Alpha', 10, True);
  SetUser(Users[2], 'Bravo', 25, True);
  SetUser(Users[3], 'Charlie', 50, False);

  For Index := 1 To MaxUsers Do
  Begin
    DisplayUser(Users[Index]);
    WriteLn('');
  End;

  WriteLn('|PA');
End.
```

This example demonstrates:

- A custom record type
- An array of records
- A `Var` record parameter
- A record passed by value
- A Boolean function using record fields
- Loop-based processing of records
- Constants controlling capacity and access logic

The procedure-parameter and function-parameter examples require Mystic 1.11 or a compatible later build.

## Common Errors

### Forgetting to Define the Type First

Incorrect concept:

```pascal
Var
  UserInfo : TUserInfo;

Type
  TUserInfo = Record
    Name : String[30];
  End;
```

Define the type before declaring a variable that uses it.

### Forgetting the Field Name

Incorrect:

```pascal
UserInfo := 'Sysop';
```

Correct:

```pascal
UserInfo.Name := 'Sysop';
```

unless assigning another compatible complete record.

### Treating an Array Field Like a Scalar

For:

```pascal
Scores : Array[1..5] of Integer;
```

incorrect:

```pascal
UserInfo.Scores := 100;
```

Correct:

```pascal
UserInfo.Scores[1] := 100;
```

### Assuming Record Declaration Initialization

Do not assume:

```pascal
Var
  UserInfo : TUserInfo = ...;
```

Initialize fields in executable code or use a supported helper routine.

### Using Record Parameters on an Older Compiler

Record procedure parameters were added in Mystic 1.11.

Do not assume this syntax is valid on Mystic 1.10:

```pascal
Procedure ShowUser(UserInfo : TUserInfo);
```

### Assuming Embedded Records Work Before 1.12

Direct record fields whose type is another record are a Mystic 1.12-era feature.

### Assuming Complete Record Equality

Do not assume this is supported:

```pascal
If User1 = User2 Then
```

Compare the required fields explicitly unless compiler testing proves complete-record comparison behavior.

### Confusing Language Records with Mystic Database Records

A custom MPL record:

```pascal
Type
  TExample = Record
    ...
  End;
```

does not automatically represent Mystic's internal user, message, file, or configuration record format.

## Verification Checklist

Before marking this page fully verified, test and record:

- Basic `Type ... Record ... End` declaration
- Scalar record fields
- String fields
- Field read/write access
- Record-to-record assignment
- Arrays of records
- Arrays inside records
- Multidimensional arrays inside records
- Copying array elements containing records
- Local record variables
- Record size with `SizeOf`
- Record declaration initialization rejection
- Record procedure parameters by value
- Record procedure parameters with `Var`
- Record function parameters
- Functions returning records
- Embedded records
- Arrays inside embedded records
- Complete-record comparison behavior
- Complex record assignment behavior
- 32-bit versus 64-bit differences

Suggested verification record:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
Basic record:
Record assignment:
Array of records:
Array inside record:
Multidimensional array inside record:
Local record:
Value parameter:
Var parameter:
Record return:
Embedded record:
SizeOf:
Record initialization rejected:
Complex assignment:
Notes:
```

## Version Reference

Record support expanded across several Mystic releases.

### Mystic 1.10

Important record changes include:

- More capable record handling
- Arrays of records
- Assignment between compatible base record variables
- `SizeOf`
- Broader combinations of arrays and records

Repository references:

- [Mystic 1.10 Changes](../documents/mystic-changes/Mystic-1.10.md)
- [Mystic 1.10 Alpha 1–21](../documents/mystic-changes/1.10/Alpha-01-21.md)

### Mystic 1.11

Important record changes include:

- Records passed to procedures by value
- Records passed to procedures by reference with `Var`
- Functions returning record types
- Fixes for multidimensional arrays inside records

Repository reference:

- [Mystic 1.11 Changes](../documents/mystic-changes/Mystic-1.11.md)

### Mystic 1.12

Important record changes include:

- Embedded records
- Improved combinations of records and arrays
- Compiler rejection of invalid declaration-time record initialization
- Continued fixes for nested records and arrays

Repository reference:

- [Mystic 1.12 Changes](../documents/mystic-changes/Mystic-1.12.md)

Cross-version reference:

- [MPL Change Index](../documents/mystic-changes/MPL-Change-Index.md)

## Related Pages

- [Program Structure](Program-Structure)
- [Variables](Variables)
- [Constants](Constants)
- [Data Types](Data-Types)
- [Arrays](Arrays)
- [Operators](Operators)
- [Conditional Logic](Conditional-Logic)
- [Loops](Loops)
- [Procedures](Procedures)
- [Functions](Functions)
- [File Handling](File-Handling)
- [Compiler Behavior](Compiler-Behavior)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- Repository: `documents/mystic-changes/Mystic-1.10.md`
- Repository: `documents/mystic-changes/Mystic-1.11.md`
- Repository: `documents/mystic-changes/Mystic-1.12.md`
- Repository: `documents/mystic-changes/MPL-Change-Index.md`

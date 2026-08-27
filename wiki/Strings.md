# Strings

Strings store text in MPL. They are used throughout Mystic programs for names, paths, menu text, prompts, commands, file data, message text, parameters, and other user-visible or system-facing values.

Modern MPL supports normal strings, explicitly sized strings, character indexing, concatenation, comparison, and a set of string- and word-processing functions.

> **Documentation status**
>
> Core string syntax is established in the repository's Mystic 1.10 change history. Exact truncation behavior, comparison case sensitivity, and some helper-function signatures should still be verified with the target Mystic and MPLC build.

## Declaring a String

A normal string variable is declared with `String`:

```pascal
Var
  UserName : String;
```

Assign text with single quotation marks:

```pascal
UserName := 'Mystic User';
```

A string can also be initialized when declared:

```pascal
Var
  UserName : String = 'Mystic User';
```

## String Literals

Text written directly in source code is a string literal:

```pascal
'Hello'
'Mystic BBS'
'Press a key'
```

String literals use single quotes.

Example:

```pascal
WriteLn('Welcome to Mystic BBS');
```

## Empty Strings

An empty string contains no text:

```pascal
UserName := '';
```

Empty strings are useful when clearing a value or checking whether text was supplied.

Conceptual example:

```pascal
If UserName = '' Then
  WriteLn('No user name was supplied');
```

String comparison behavior should be verified with the target compiler, especially for case sensitivity and trailing spaces.

## Default String Length

The current project reference documents an unsized `String` as having a default maximum length of 255 characters:

```pascal
Var
  Text : String;
```

Mystic 1.10 also documents string storage as the declared string length plus one byte.

For exact storage and boundary behavior, verify with the target MPLC build.

## Sized Strings

Mystic 1.10 added explicit maximum string lengths.

Example:

```pascal
Var
  UserName : String[30];
```

Other examples:

```pascal
Var
  MenuText : String[40];
  FileName : String[80];
  ShortKey : String[5];
```

A sized string documents the intended maximum length and can reduce unnecessary storage in large arrays and records.

## Assigning Text to a Sized String

```pascal
Var
  Name : String[10];

Begin
  Name := 'Mystic';
End.
```

When the source text is longer than the declared string size, do not assume whether MPLC will reject, truncate, or otherwise handle the value.

Test this explicitly:

```pascal
Var
  Name : String[5];

Begin
  Name := 'Mystic BBS';
End.
```

The truncation/boundary result belongs in the compiler verification record.

## Concatenating Strings

MPL uses `+` to join strings.

```pascal
Var
  FirstName : String = 'Mystic';
  LastName  : String = 'User';
  FullName  : String;

Begin
  FullName := FirstName + ' ' + LastName;
  WriteLn(FullName);
End.
```

Several pieces can be combined:

```pascal
WriteLn('User: ' + UserName + ' logged in');
```

See [Operators](Operators).

## Converting Numbers Before Concatenation

Do not assume automatic number-to-string conversion.

Instead of:

```pascal
WriteLn('Level: ' + UserLevel);
```

use an explicit conversion:

```pascal
WriteLn('Level: ' + Int2Str(UserLevel));
```

This makes the intended type conversion clear.

See [Functions](Functions) for conversion helpers.

## Character Indexing

Mystic 1.10 added the ability to access characters inside a string using square brackets.

```pascal
Var
  Text : String;

Begin
  Text := 'Mystic';

  If Text[1] = 'M' Then
    WriteLn('The first character is M');
End.
```

String indexing follows Pascal-style one-based positioning in the documented 1.10 behavior.

For:

```text
Mystic
```

the positions are conceptually:

```text
Position:  1 2 3 4 5 6
Character: M y s t i c
```

Do not use C-style zero-based assumptions.

## Reading a Character

A character retrieved from a string can be used where a `Char` value is expected.

```pascal
Var
  Text  : String;
  First : Char;

Begin
  Text := 'Mystic';
  First := Text[1];

  WriteLn(First);
End.
```

The exact behavior when indexing an empty string or a position beyond the current string length should be compiler-tested.

## Writing a Character by Index

Modern MPL documentation indicates that strings can be treated as character collections in many contexts.

Conceptual example:

```pascal
Var
  Text : String;

Begin
  Text := 'Mystic';
  Text[1] := 'm';
End.
```

Because indexed assignment can be more compiler-sensitive than indexed reading, verify this form with the target MPLC build before relying on it.

## Numeric Character Codes

Mystic 1.10 added numeric character references using `#`.

Example:

```pascal
#32
```

represents the character whose numeric code is 32.

Example:

```pascal
Var
  Ch : Char;

Begin
  Ch := #32;

  If Ch = #32 Then
    WriteLn('Space character');
End.
```

These values are useful for control characters and characters that are awkward to type directly.

## String and Char

A `String` stores text of variable length.

A `Char` stores one character.

```pascal
Var
  Text : String;
  Key  : Char;
```

Examples:

```pascal
Text := 'ABC';
Key := 'A';
```

A string containing one character and a `Char` are related concepts but should not automatically be treated as identical in every function call or assignment.

## String Comparison

Strings can be compared in conditions.

```pascal
If UserName = 'Sysop' Then
  WriteLn('Sysop account');
```

A not-equal comparison depends on the operator syntax supported by the target MPLC build; see [Operators](Operators) for the documented `<>` versus `!=` version conflict.

Other comparisons may be accepted:

```pascal
If NameA < NameB Then
  WriteLn('NameA sorts first');
```

Do not assume lexical ordering, case handling, or trailing-space behavior without testing.

## Case Sensitivity

The documentation project should not assume whether these always compare as equal:

```text
Mystic
MYSTIC
mystic
```

Test:

```pascal
If 'Mystic' = 'MYSTIC' Then
  WriteLn('Equal')
Else
  WriteLn('Different');
```

Record the result for the exact MPLC version.

## Strings in Conditions

Strings are commonly used with conditional logic.

```pascal
If Command = 'HELP' Then
  WriteLn('Displaying help')
Else If Command = 'QUIT' Then
  WriteLn('Leaving');
```

See [Conditional Logic](Conditional-Logic).

## Arrays of Strings

Strings can be stored in arrays.

```pascal
Var
  Names : Array[1..5] of String[30];
```

Assign elements:

```pascal
Names[1] := 'Alpha';
Names[2] := 'Bravo';
Names[3] := 'Charlie';
```

Process them with a loop:

```pascal
For Index := 1 To 3 Do
  WriteLn(Names[Index]);
```

See [Arrays](Arrays).

## Strings in Records

Strings are common record fields.

```pascal
Type
  TUserInfo = Record
    Name  : String[30];
    Email : String[80];
    Level : Integer;
  End;
```

Access the fields:

```pascal
UserInfo.Name := 'Example User';
UserInfo.Email := 'user@example.invalid';
```

See [Records](Records).

## Passing Strings to Procedures

Strings can be passed to procedures.

```pascal
Procedure ShowMessage(Message String);
Begin
  WriteLn(Message);
End;
```

Use:

```pascal
ShowMessage('Hello from MPL');
```

A local string can also be used inside the routine:

```pascal
Procedure DisplayName(Name String);
Var
  LabelText : String;
Begin
  LabelText := 'User: ' + Name;
  WriteLn(LabelText);
End;
```

## String Parameters with `Var`

Modern MPL supports `Var` reference parameters.

Conceptual example:

```pascal
Procedure ClearText(Var Text String);
Begin
  Text := '';
End;
```

Then:

```pascal
ClearText(UserName);
```

Verify exact parameter punctuation and syntax against the target MPLC build.

See [Procedures](Procedures).

## Functions Returning Strings

Functions can return strings.

Conceptual example:

```pascal
Function MakeLabel(Name String) : String;
Begin
  MakeLabel := 'User: ' + Name;
End;
```

Use:

```pascal
WriteLn(MakeLabel(UserName));
```

See [Functions](Functions).

## Word-Oriented String Functions

Mystic 1.10 Alpha 16 expanded string-processing support with word-oriented helpers.

The repository history identifies:

| Function | Purpose |
|---|---|
| `WordCount` | Count delimited words or fields |
| `WordGet` | Return a selected word or field |
| `WordPos` | Locate a word or field |
| `Replace` | Replace matching text |
| `StrWrap` | Wrap text to a target width |
| `StrComma` | Format numeric text with separators |

Exact signatures and delimiter behavior should be documented on [Functions](Functions) or compiler-tested before examples are treated as authoritative.

## `WordCount`

Typical use is to count fields or words in text.

Conceptual example:

```pascal
Count := WordCount(Text, ' ');
```

Do not assume the exact argument order or delimiter rules until verified.

Useful cases include:

- Space-delimited text
- Command parameters
- Comma-separated values
- Pipe-delimited values
- Menu or configuration fields

## `WordGet`

`WordGet` returns a selected word or field from delimited text.

Conceptual operation:

```text
Input:  ALPHA|BRAVO|CHARLIE
Field:  2
Result: BRAVO
```

Exact MPL syntax should be confirmed before copying a call into production code.

## `WordPos`

`WordPos` locates a word or field within delimited text.

This can be useful when checking whether a command, option, or token exists in a list.

Exact return conventions—especially whether zero means "not found"—should be verified.

## `Replace`

`Replace` performs text replacement.

Conceptual operation:

```text
Input:       Hello USER
Find:        USER
Replacement: Sysop
Result:      Hello Sysop
```

Verify whether replacement is case-sensitive and whether all matches or only one match are replaced.

## `StrWrap`

`StrWrap` wraps text to a target width.

This is useful for:

- Terminal displays
- Message text
- Help screens
- Menu descriptions
- Generated status output

Terminal width and Mystic display codes can affect practical wrapping behavior.

## `StrComma`

`StrComma` formats numeric text with separators.

Conceptual result:

```text
1000000
↓
1,000,000
```

Verify accepted input types and return type with the target compiler.

## Path-Related String Helpers

Mystic 1.10 Alpha 16 also documents path-oriented helpers:

```text
JustPath
JustFile
JustFileName
JustFileExt
```

These functions operate on path and filename strings and belong primarily on [Functions](Functions) or [File Handling](File-Handling).

They are relevant here because their inputs and results are strings.

## Environment Strings

The same 1.10 development period added `ReadEnv`, allowing MPL code to retrieve environment-variable values as text.

Conceptual use:

```pascal
Value := ReadEnv('HOME');
```

The exact signature and platform-specific behavior should be verified.

## Building Display Text

A common MPL pattern is to construct a string before displaying it:

```pascal
Var
  LabelText : String;

Begin
  LabelText := 'User: ' + UserName;
  WriteLn(LabelText);
End.
```

This becomes useful when the same text is displayed, logged, stored, or passed to another routine.

## Parsing Program Parameters

MPL runtime data such as `ProgParams` is string-based.

Conceptually:

```pascal
Begin
  WriteLn('Parameters: ' + ProgParams);
End.
```

Word-oriented functions are useful for parsing multiple values passed through a parameter string.

See [Program Structure](Program-Structure).

## Complete Example

```pascal
Const
  MaxNames = 3;

Var
  Names : Array[1..MaxNames] of String[30];
  Index : Integer;
  LabelText : String;

Procedure SetNames;
Begin
  Names[1] := 'Alpha';
  Names[2] := 'Bravo';
  Names[3] := 'Charlie';
End;

Procedure DisplayName(Name String);
Var
  FirstCharacter : Char;
Begin
  If Name = '' Then
  Begin
    WriteLn('Empty name');
    Exit;
  End;

  FirstCharacter := Name[1];
  LabelText := 'Name: ' + Name;

  WriteLn(LabelText);
  WriteLn('First character: ' + FirstCharacter);
End;

Begin
  SetNames;

  For Index := 1 To MaxNames Do
  Begin
    DisplayName(Names[Index]);
  End;

  WriteLn('|PA');
End.
```

This example demonstrates:

- Sized strings
- Arrays of strings
- String parameters
- Empty-string checks
- One-based character indexing
- String concatenation
- Local `Char` extraction
- Loop-based string processing

The `String + Char` concatenation shown in the final display line should be included in compiler verification. If the target MPLC build requires conversion, adjust that example accordingly.

## Common Errors

### Using Double Quotes

Do not assume:

```text
"Hello"
```

Use the documented MPL string form:

```pascal
'Hello'
```

### Using Index Zero

Incorrect assumption:

```pascal
Text[0]
```

Documented modern MPL indexing begins with the first character at:

```pascal
Text[1]
```

### Indexing an Empty String

Avoid reading:

```pascal
Text[1]
```

when `Text` may be empty.

Check first:

```pascal
If Text <> '' Then
  First := Text[1];
```

Use the not-equal operator form verified for the target compiler.

### Exceeding a Sized String

For:

```pascal
Var
  Name : String[5];
```

do not assume how this behaves:

```pascal
Name := 'Longer than five';
```

Test truncation and compiler/runtime handling.

### Assuming Automatic Numeric Conversion

Potentially invalid:

```pascal
WriteLn('Count: ' + Count);
```

Preferred:

```pascal
WriteLn('Count: ' + Int2Str(Count));
```

### Assuming Comparison Is Case-Insensitive

Do not assume:

```pascal
'Mystic' = 'MYSTIC'
```

Test it.

### Treating a String as an Array Declaration

A string can be indexed:

```pascal
Text[1]
```

but it is still a `String`, not:

```pascal
Array[...] of Char
```

See [Arrays](Arrays).

## Verification Checklist

Before marking this page fully verified, test and record:

- Unsized `String`
- Default maximum length
- `String[n]`
- Empty-string assignment
- Declaration-time initialization
- String concatenation
- String and numeric conversion
- String and `Char` concatenation
- One-based character indexing
- Indexed character reads
- Indexed character writes
- Empty-string indexing
- Out-of-range indexing
- Numeric character codes with `#nn`
- String equality
- String not-equal syntax
- Case sensitivity
- Trailing-space comparison behavior
- Lexical `<` / `>` behavior
- Sized-string overflow/truncation
- String procedure parameters
- `Var` string parameters
- String function results
- `WordCount`
- `WordGet`
- `WordPos`
- `Replace`
- `StrWrap`
- `StrComma`
- `JustPath`
- `JustFile`
- `JustFileName`
- `JustFileExt`
- `ReadEnv`
- 32-bit versus 64-bit behavior where relevant

Suggested verification record:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
String:
String[n]:
Default length:
Concatenation:
Char concatenation:
Index read:
Index write:
Empty indexing:
Out-of-range indexing:
Case sensitivity:
Trailing spaces:
Sized-string truncation:
Word functions:
Replace:
StrWrap:
Path helpers:
ReadEnv:
Notes:
```

## Version Reference

Mystic 1.10 was the major modernization point for MPL strings.

Important documented changes include:

- Explicit `String[n]` lengths
- Numeric character references such as `#32`
- One-based character indexing with square brackets
- Expanded string and word parsing
- `WordCount`
- `WordGet`
- `WordPos`
- `Replace`
- `StrWrap`
- `StrComma`
- Path-related string helpers
- Environment-variable access

Repository references:

- [Mystic 1.10 Changes](../documents/mystic-changes/Mystic-1.10.md)
- [Mystic 1.10 Alpha 1–21](../documents/mystic-changes/1.10/Alpha-01-21.md)
- [MPL Change Index](../documents/mystic-changes/MPL-Change-Index.md)

## Related Pages

- [Program Structure](Program-Structure)
- [Variables](Variables)
- [Constants](Constants)
- [Data Types](Data-Types)
- [Operators](Operators)
- [Conditional Logic](Conditional-Logic)
- [Loops](Loops)
- [Arrays](Arrays)
- [Records](Records)
- [Procedures](Procedures)
- [Functions](Functions)
- [File Handling](File-Handling)
- [Compiler Behavior](Compiler-Behavior)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- Repository: `documents/mystic-changes/Mystic-1.10.md`
- Repository: `documents/mystic-changes/1.10/Alpha-01-21.md`
- Repository: `documents/mystic-changes/MPL-Change-Index.md`

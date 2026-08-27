# Arrays

Arrays store multiple values of the same element type under one variable name. Each value is accessed by an index.

MPL supports fixed-range arrays and multidimensional arrays. Modern MPL uses square brackets for both array declarations and element access.

> **Documentation status**
>
> Core array syntax is documented in the current Mystic MPL reference and in the Mystic 1.10 change history. Exact boundary behavior, parameter passing, complex record combinations, and compiler-specific restrictions should still be verified with the target MPLC build.

## Basic Array Declaration

A one-dimensional array uses this form:

```pascal
Var
  Scores : Array[1..10] of Integer;
```

The declaration contains:

```text
Array[lower-bound..upper-bound] of element-type
```

In this example:

- The array variable is named `Scores`.
- Valid indexes are intended to run from `1` through `10`.
- Every element stores an `Integer`.

## Accessing an Array Element

Use square brackets to select one element:

```pascal
Scores[1] := 90;
Scores[2] := 85;

WriteLn(Int2Str(Scores[1]));
```

Mystic 1.10 changed array element addressing from parentheses to square brackets.

Older source may contain a form similar to:

```text
Scores(1)
```

Modern MPL uses:

```pascal
Scores[1]
```

Do not mix the historical and modern forms in new source.

## Array Bounds

The lower bound does not have to be `1`.

The current MPL reference includes declarations such as:

```pascal
Var
  Names : Array[5..10] of String;
```

The declared range determines which indexes belong to the array.

For:

```pascal
Var
  Values : Array[5..10] of Integer;
```

code should remain within that declared range:

```pascal
Values[5] := 100;
Values[10] := 200;
```

Access outside the declared bounds should be treated as unsafe unless the exact compiler/runtime behavior has been tested.

## Array Element Types

Array elements can use normal MPL data types.

Examples:

```pascal
Var
  Flags  : Array[1..10] of Boolean;
  Keys   : Array[1..26] of Char;
  Counts : Array[1..20] of Integer;
  Names  : Array[1..25] of String;
```

Strings may also use an explicit length:

```pascal
Var
  UserNames : Array[1..20] of String[30];
```

See [Data Types](Data-Types) for the supported scalar types and their version notes.

## Assigning Values

Elements are assigned individually:

```pascal
Var
  Scores : Array[1..3] of Integer;

Begin
  Scores[1] := 90;
  Scores[2] := 85;
  Scores[3] := 100;
End.
```

Each element behaves like a variable of the declared element type.

For a Boolean array:

```pascal
Flags[1] := True;
Flags[2] := False;
```

For a string array:

```pascal
Names[1] := 'Sysop';
Names[2] := 'Guest';
```

## Declaration-Time Initialization

The current Mystic MPL reference states that variables can be initialized when declared **except arrays**.

This is valid for a scalar variable:

```pascal
Var
  Count : Integer = 10;
```

Do not assume this form is valid for an array:

```pascal
Var
  Scores : Array[1..3] of Integer = ...;
```

Initialize array elements explicitly at runtime unless the target compiler has been tested with another supported form.

## Processing an Array with a `For` Loop

A `For` loop is the usual way to process a fixed array range.

```pascal
Var
  Scores : Array[1..5] of Integer;
  Index  : Integer;

Begin
  For Index := 1 To 5 Do
  Begin
    Scores[Index] := 0;
  End;
End.
```

To display the elements:

```pascal
For Index := 1 To 5 Do
Begin
  WriteLn(Int2Str(Scores[Index]));
End;
```

The loop range should match the declared array bounds.

See [Loops](Loops).

## Using a Non-One-Based Range

If the array begins at another lower bound, use that same range when iterating.

```pascal
Var
  Values : Array[5..10] of Integer;
  Index  : Integer;

Begin
  For Index := 5 To 10 Do
  Begin
    Values[Index] := Index * 2;
  End;
End.
```

Hard-coding `1` as the starting index would be incorrect for this declaration.

## Multidimensional Arrays

Mystic 1.10 added multidimensional arrays up to three dimensions.

Two-dimensional declaration:

```pascal
Var
  Grid : Array[1..10, 1..10] of Integer;
```

Three-dimensional declaration:

```pascal
Var
  Cube : Array[1..10, 1..10, 1..10] of String[30];
```

The official Mystic 1.10 history specifically documents support for arrays up to three levels deep.

## Accessing Multidimensional Elements

A multidimensional element uses one index for each declared dimension.

Example form:

```pascal
Grid[2, 4] := 100;
Cube[1, 3, 5] := 'Example';
```

Because this syntax should be verified against the exact compiler used by the BBS, include multidimensional element access in the project test suite before marking the page fully verified.

## Processing a Two-Dimensional Array

Nested loops map naturally to a two-dimensional array.

```pascal
Var
  Grid   : Array[1..3, 1..4] of Integer;
  Row    : Integer;
  Column : Integer;

Begin
  For Row := 1 To 3 Do
  Begin
    For Column := 1 To 4 Do
    Begin
      Grid[Row, Column] := 0;
    End;
  End;
End.
```

The outer loop processes one dimension and the inner loop processes the other.

## Processing a Three-Dimensional Array

A three-dimensional array normally requires three nested loops when every element must be visited.

```pascal
Var
  Data : Array[1..2, 1..3, 1..4] of Integer;
  X    : Integer;
  Y    : Integer;
  Z    : Integer;

Begin
  For X := 1 To 2 Do
  Begin
    For Y := 1 To 3 Do
    Begin
      For Z := 1 To 4 Do
      Begin
        Data[X, Y, Z] := 0;
      End;
    End;
  End;
End.
```

Nested loops multiply the number of operations. Large arrays combined with terminal output, file access, or Mystic record functions should be tested for performance.

## Arrays of Strings

String arrays are useful for menu labels, filenames, names, commands, or other repeated text values.

```pascal
Var
  MenuText : Array[1..4] of String[40];

Begin
  MenuText[1] := 'View messages';
  MenuText[2] := 'List files';
  MenuText[3] := 'User settings';
  MenuText[4] := 'Quit';
End.
```

Process them with a loop:

```pascal
For Index := 1 To 4 Do
  WriteLn(MenuText[Index]);
```

## Arrays of Records

Mystic 1.10 development added support for arrays of records.

Example structure:

```pascal
Type
  TUserItem = Record
    Name  : String[30];
    Level : Integer;
  End;

Var
  Users : Array[1..10] of TUserItem;
```

Individual record fields are then selected through the array element:

```pascal
Users[1].Name := 'Example User';
Users[1].Level := 20;
```

Arrays of records combine two advanced features. Test field-access syntax and record assignment behavior with the target MPLC build before relying on them in production code.

See the planned [Records](Records) page.

## Strings Are Also Indexable

Modern MPL allows strings to be accessed in many contexts as arrays of characters.

```pascal
Var
  Text : String;

Begin
  Text := 'Hello';

  If Text[1] = 'H' Then
    WriteLn('First character is H');
End.
```

A string is still a `String`, not an array variable declared with `Array[...] of Char`.

Keep these concepts separate:

```pascal
Var
  Text  : String;
  Chars : Array[1..10] of Char;
```

Both support bracketed element access, but they are different types with different storage and language behavior.

## Searching an Array

A common pattern is to scan until a matching element is found.

```pascal
Var
  Names : Array[1..5] of String[30];
  Index : Integer;
  Found : Boolean = False;

Begin
  Names[1] := 'Alpha';
  Names[2] := 'Bravo';
  Names[3] := 'Charlie';
  Names[4] := 'Delta';
  Names[5] := 'Echo';

  For Index := 1 To 5 Do
  Begin
    If Names[Index] = 'Charlie' Then
    Begin
      Found := True;
      Break;
    End;
  End;

  If Found Then
    WriteLn('Name found');
End.
```

See [Conditional Logic](Conditional-Logic) and [Loops](Loops).

## Counting Matching Elements

```pascal
Var
  Scores    : Array[1..5] of Integer;
  Index     : Integer;
  HighCount : Integer = 0;

Begin
  Scores[1] := 90;
  Scores[2] := 75;
  Scores[3] := 95;
  Scores[4] := 80;
  Scores[5] := 100;

  For Index := 1 To 5 Do
  Begin
    If Scores[Index] >= 90 Then
      HighCount := HighCount + 1;
  End;

  WriteLn('High scores: ' + Int2Str(HighCount));
End.
```

## Array Bounds and Off-by-One Errors

For:

```pascal
Var
  Values : Array[1..10] of Integer;
```

this loop matches the declaration:

```pascal
For Index := 1 To 10 Do
  Values[Index] := 0;
```

This loop attempts to use an index outside that declared range:

```pascal
For Index := 0 To 10 Do
  Values[Index] := 0;
```

The safest rule is simple:

> Make the loop limits match the array's declared lower and upper bounds.

Do not assume out-of-range access will always produce a clean compiler or runtime error.

## Fixed Size Versus Dynamic Size

The documented MPL array syntax defines bounds in the declaration:

```pascal
Array[1..100] of Integer
```

The current core reference documents fixed-range arrays. Dynamic-array allocation and resizing should not be assumed unless a specific MPLC version or Mystic API explicitly documents it.

When the required item count varies, common approaches include:

- Declare a maximum fixed capacity
- Track the number of elements currently in use
- Use Mystic records or file structures when the data set is persistent
- Redesign around a different supported storage mechanism when the upper bound is not practical

Example:

```pascal
Const
  MaxItems = 100;

Var
  Items     : Array[1..MaxItems] of String[40];
  ItemCount : Integer = 0;
```

Only indexes from `1` through `ItemCount` contain active values.

## Using a Constant for the Upper Bound

A named constant avoids repeating a numeric limit throughout the source.

```pascal
Const
  MaxScores = 20;

Var
  Scores : Array[1..MaxScores] of Integer;
  Index  : Integer;

Begin
  For Index := 1 To MaxScores Do
    Scores[Index] := 0;
End.
```

This is easier to maintain than repeating `20` in several declarations and loops.

See [Constants](Constants).

## Array Scope

Like other variables, an array can exist at program scope or in a supported local scope.

Program-level example:

```pascal
Var
  Scores : Array[1..10] of Integer;
```

A locally declared array should be tested with the target compiler, especially when the element type is a record or the array is large.

Mystic 1.10 significantly expanded local variable support, but complex local structures deserve explicit compiler verification.

## Passing Arrays to Procedures and Functions

Do not assume ordinary Pascal array-parameter syntax is accepted by MPL.

The exact supported syntax for:

- Passing an entire array by value
- Passing an array with `Var`
- Passing multidimensional arrays
- Passing arrays of records
- Returning an array from a function

should be compiler-tested before being documented as portable MPL.

When only one element is required, passing the element itself avoids this uncertainty:

```pascal
Procedure ShowScore(Score Integer);
Begin
  WriteLn(Int2Str(Score));
End;

Begin
  ShowScore(Scores[1]);
End.
```

## Complete Example

```pascal
Const
  MaxScores = 5;

Var
  Scores    : Array[1..MaxScores] of Integer;
  Index     : Integer;
  Total     : LongInt = 0;
  HighScore : Integer = 0;

Procedure LoadScores;
Begin
  Scores[1] := 90;
  Scores[2] := 75;
  Scores[3] := 100;
  Scores[4] := 85;
  Scores[5] := 95;
End;

Procedure ProcessScores;
Begin
  Total := 0;
  HighScore := Scores[1];

  For Index := 1 To MaxScores Do
  Begin
    Total := Total + Scores[Index];

    If Scores[Index] > HighScore Then
      HighScore := Scores[Index];
  End;
End;

Procedure DisplayScores;
Begin
  For Index := 1 To MaxScores Do
    WriteLn('Score: ' + Int2Str(Scores[Index]));

  WriteLn('Total: ' + Int2Str(Total));
  WriteLn('High score: ' + Int2Str(HighScore));
End;

Begin
  LoadScores;
  ProcessScores;
  DisplayScores;
  WriteLn('|PA');
End.
```

This example demonstrates:

- A constant used as an array bound
- A one-dimensional integer array
- Explicit element initialization
- Array indexing
- Loop-based processing
- Comparison of array elements
- Procedures operating on a program-level array

## Common Errors

### Using Parentheses for Modern Array Access

Historical form:

```text
Scores(1)
```

Modern form:

```pascal
Scores[1]
```

### Using an Index Outside the Declared Range

Declaration:

```pascal
Var
  Scores : Array[1..5] of Integer;
```

Incorrect:

```pascal
Scores[0] := 100;
Scores[6] := 100;
```

### Starting a Loop at the Wrong Bound

Declaration:

```pascal
Var
  Values : Array[5..10] of Integer;
```

Incorrect loop:

```pascal
For Index := 1 To 10 Do
  Values[Index] := 0;
```

Correct range:

```pascal
For Index := 5 To 10 Do
  Values[Index] := 0;
```

### Assuming Declaration Initialization

Do not assume this is supported:

```pascal
Var
  Values : Array[1..3] of Integer = ...;
```

Initialize the elements explicitly.

### Supplying the Wrong Number of Dimensions

For:

```pascal
Var
  Grid : Array[1..10, 1..10] of Integer;
```

use an index for each dimension:

```pascal
Grid[Row, Column]
```

### Mixing String Indexing with Array Semantics

```pascal
Text[1]
```

and:

```pascal
Chars[1]
```

may look similar, but one accesses a character within a string and the other accesses an element of an explicitly declared array.

## Verification Checklist

Before marking this page fully verified, test and record:

- One-dimensional array declaration
- Non-one lower bounds
- Element reads and writes
- Out-of-range behavior
- Arrays of `Boolean`
- Arrays of `Char`
- Arrays of numeric types
- Arrays of `String`
- Explicit-length string arrays such as `String[30]`
- Two-dimensional arrays
- Three-dimensional arrays
- Multidimensional element access
- Maximum supported dimensions
- Arrays of records
- Record-field access through an array
- Program-level arrays
- Local arrays
- Array declaration initialization rejection or behavior
- Passing arrays to procedures
- Passing arrays with `Var`
- Arrays as function results
- Compiler behavior for large arrays

Suggested verification record:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
1D arrays:
Non-one lower bound:
2D arrays:
3D arrays:
Array initialization:
Arrays of strings:
Arrays of records:
Local arrays:
Array parameters:
Out-of-range behavior:
Notes:
```

## Version Reference

Modern array behavior is strongly tied to the Mystic 1.10 MPL redesign.

Important 1.10 changes include:

- Array element access changed from parentheses to square brackets
- Multidimensional arrays were added up to three levels deep
- Arrays of records were added during the 1.10 development cycle
- Strings became indexable with square brackets in many contexts

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
- [Procedures](Procedures)
- [Functions](Functions)
- [Records](Records)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- Repository: `documents/mystic-changes/Mystic-1.10.md`

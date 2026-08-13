# Data Types

Data types define what kind of value an MPL variable can store, how much storage it uses, and which operations are valid for that value.

Modern MPL uses Pascal-style declarations and supports scalar types, strings, arrays, records, and a `File` type used by the MPL file-I/O system.

> **Documentation status**
>
> The core types on this page are documented by the Mystic MPL reference and Mystic 1.10 change history. Some published range information is inconsistent or appears to contain typographical errors, so boundary values should be compiler-tested before being treated as authoritative for every Mystic build.

## Supported Scalar Types

The current Mystic MPL reference documents these scalar types:

| Type | Purpose | Documented form |
|---|---|---|
| `Boolean` | Logical state | `True` / `False` |
| `Char` | One character | `'A'` |
| `String` | Text | `'Hello'` |
| `Byte` | Small unsigned integer | `0..255` |
| `Integer` | Signed integer | approximately 16-bit |
| `Word` | Unsigned integer | `0..65535` |
| `LongInt` | Larger signed integer | 4-byte integer |
| `Cardinal` | Larger unsigned integer | `0..4294967295` in the current reference |
| `Real` | Floating-point number | decimal value |

Mystic 1.10 documents the following storage sizes:

```text
Byte, Char    = 1 byte
Integer, Word = 2 bytes
LongInt       = 4 bytes
String        = declared length + 1 byte
```

The current quick-reference page contains a likely typographical error in the published `LongInt` range. For that reason, this wiki records the documented storage size and recommends testing exact minimum and maximum values with the target MPLC build.

## Declaring Variables

Modern MPL follows Pascal-style variable declarations:

```pascal
Var
  UserName : String;
  Initial  : Char;
  Age      : Integer;
  IsActive : Boolean;
```

Multiple variables of the same type can share a declaration:

```pascal
Var
  X, Y : Byte;
```

Mystic 1.10 standardized this syntax and also added local variable declarations inside procedures and functions.

See [Variables](Variables) for declaration scope and initialization rules.

## Variable Initialization

Modern MPL allows most variables to be initialized when declared.

```pascal
Var
  Count    : Integer = 10;
  UserName : String = 'Sysop';
  IsReady  : Boolean = True;
```

Variables can also be initialized from a function result:

```pascal
Var
  TotalBases : LongInt = GetMBaseTotal(False);
```

The current MPL reference states that arrays are an exception to declaration-time initialization.

## `Boolean`

A `Boolean` stores a logical state:

```pascal
True
False
```

Example:

```pascal
Var IsEnabled : Boolean = True;

Begin
  If IsEnabled Then
    WriteLn('Enabled');
End;
```

Boolean values are primarily used with conditional logic and logical operators.

See [Conditional Logic](Conditional-Logic) and [Operators](Operators).

## `Char`

A `Char` stores one character.

```pascal
Var MenuChoice : Char = 'A';
```

Mystic 1.10 also added numeric character references using `#` followed by an ASCII value:

```pascal
If MenuChoice = #32 Then
  WriteLn('Space was pressed');
```

A `Char` occupies one byte according to the Mystic 1.10 change history.

## `String`

A `String` stores text.

```pascal
Var UserName : String = 'Mystic User';
```

String literals use single quotes:

```pascal
'Hello World'
```

### String Length

Mystic 1.10 added explicit string lengths:

```pascal
Var ShortName : String[10];
```

When no length is supplied, the official documentation states that the default maximum is 255 characters:

```pascal
Var FullText : String;
```

The documented storage requirement is the declared string length plus one byte.

### Empty String

An empty string contains no characters:

```pascal
UserName := '';
```

### String Indexing

Modern MPL allows strings to be accessed as an array of characters in most places:

```pascal
Var S : String;

Begin
  S := 'Hello';

  If S[1] = 'H' Then
    WriteLn('First character is H');
End;
```

The indexing convention should be treated as one-based unless a compiler test demonstrates otherwise for a specific context.

### String Concatenation

Strings can be joined with `+`:

```pascal
WriteLn('Hello, ' + UserName);
```

Numeric values normally require explicit conversion before being concatenated with text:

```pascal
WriteLn('Count: ' + Int2Str(Count));
```

## `Byte`

A `Byte` is a small unsigned integer.

The current MPL reference documents:

```text
0..255
```

Example:

```pascal
Var ColorNumber : Byte = 7;
```

Common uses include:

- ANSI color values
- Small counters
- Menu indexes
- X/Y coordinates
- Bit-field values

## `Integer`

`Integer` is a signed whole-number type.

Mystic 1.10 documents it as a 2-byte value and added support for negative numbers.

```pascal
Var
  Count      : Integer = 20;
  Difference : Integer = -5;
```

The current quick reference lists a range close to a standard signed 16-bit integer. Boundary behavior should be tested before relying on the extreme values.

## `Word`

`Word` is an unsigned 2-byte integer type.

The current MPL reference documents:

```text
0..65535
```

Example:

```pascal
Var PortNumber : Word = 2323;
```

Use `Word` when negative values are not required and the needed range exceeds `Byte`.

## `LongInt`

`LongInt` is a signed 4-byte integer type.

```pascal
Var TotalRecords : LongInt = 100000;
```

It is commonly used for:

- Record counts
- File-related values
- Julian dates
- Larger counters
- Function results that exceed `Integer`

The current Mystic reference contains an apparent typo in the displayed upper range for `LongInt`. This wiki therefore does not silently substitute a corrected boundary without compiler verification.

## `Cardinal`

The current MPL reference lists `Cardinal` as a numeric type with this range:

```text
0..4294967295
```

That corresponds to an unsigned 32-bit range.

Example form:

```pascal
Var LargeUnsigned : Cardinal;
```

Because `Cardinal` is less common in older MPL examples, compiler support should be confirmed on older Mystic releases before using it in portable code.

## `Real`

Mystic 1.10 added the `Real` type for floating-point calculations.

```pascal
Var Average : Real;

Begin
  Average := 12.5;
End;
```

Use `Real` when fractional values are required.

The exact precision, storage size, rounding behavior, and conversion rules should be tested with the target compiler.

## `File`

Mystic 1.10 added a `File` type as part of the redesigned file-I/O system.

The type is used with functions such as:

```text
fAssign
fReset
fReWrite
fRead
fWrite
fClose
```

Conceptual declaration:

```pascal
Var DataFile : File;
```

The file-I/O redesign also changed error handling so programs could inspect `IoResult` instead of having all file errors automatically terminate the MPL program.

See [File Handling](File-Handling) for the file API rather than treating `File` as a normal scalar value.

## Arrays

Arrays store multiple values of the same type.

One-dimensional array:

```pascal
Var Scores : Array[1..10] of Integer;
```

Element access uses square brackets:

```pascal
Scores[1] := 100;
```

Mystic 1.10 changed array element syntax from parentheses to square brackets and added multidimensional arrays up to three dimensions.

Two-dimensional example:

```pascal
Var Grid : Array[1..10, 1..10] of Byte;
```

Three-dimensional example:

```pascal
Var Cube : Array[1..10, 1..10, 1..10] of String[30];
```

Arrays deserve their own page because bounds, memory use, indexing, records inside arrays, and procedure/function parameters require additional testing.

See [Arrays](Arrays).

## Records

Records group values of different types into one structured type.

Example:

```pascal
Type
  UserRec = Record
    Name  : String[30];
    Level : Byte;
  End;

Var UserInfo : UserRec;
```

Fields are accessed with a period:

```pascal
UserInfo.Name := 'Sysop';
UserInfo.Level := 100;
```

Records can also contain arrays and other supported field types.

See [Records](Records) for detailed record syntax and version-specific behavior.

## Hexadecimal Numeric Literals

Mystic 1.10 added hexadecimal numeric values.

A hexadecimal value begins with `$`:

```pascal
Value := $10;
```

Hexadecimal values can also be used in numeric constants:

```pascal
Const
  UserDeleted = $00000004;
```

See [Constants](Constants) and [Operators](Operators).

## Literals and Types

Common literal forms include:

```pascal
'Hello'     // String
'A'         // Char-sized text literal when context expects Char
#32         // Character by ASCII value
42          // Decimal integer
-10         // Negative numeric literal
$1F         // Hexadecimal numeric literal
12.5        // Real literal
True        // Boolean
False       // Boolean
```

The compiler determines whether a literal is compatible with the destination type.

## Type Conversion

Do not assume automatic conversion between unrelated types.

For display, numeric values are commonly converted to strings explicitly:

```pascal
WriteLn('Count: ' + Int2Str(Count));
```

Other conversion functions should be documented by their exact signatures on [Functions](Functions).

Important conversion areas include:

- Integer to string
- String to integer
- Character to numeric code
- Numeric code to character
- Real and integer conversion
- Boolean display text

## Choosing a Numeric Type

Use a type whose range matches the value being represented.

| Requirement | Typical type |
|---|---|
| `0..255` | `Byte` |
| Small signed whole number | `Integer` |
| `0..65535` | `Word` |
| Larger signed whole number | `LongInt` |
| Larger unsigned whole number | `Cardinal` |
| Fractional number | `Real` |

Do not choose a narrow type merely to save memory unless the range restriction is intentional.

## Overflow and Boundary Values

When a value exceeds the range of its type, results may depend on compiler checks and runtime behavior.

Potential issues include:

- Compile-time rejection
- Runtime overflow
- Value wrapping
- Truncation
- Incorrect comparisons

Boundary behavior should be explicitly tested for the target Mystic and MPLC release.

## Common Errors

### Assigning Text to a Numeric Type

Incorrect:

```pascal
Age := '25';
```

The quoted value is text.

### Assigning Several Characters to `Char`

Incorrect:

```pascal
Choice := 'YES';
```

Use `String` for multiple characters.

### Exceeding an Array Bound

```pascal
Var Values : Array[1..10] of Byte;
```

This is outside the declared range:

```pascal
Values[11] := 1;
```

### Assuming Numeric-to-String Conversion

Potentially invalid:

```pascal
WriteLn('Count: ' + Count);
```

Safer documented pattern:

```pascal
WriteLn('Count: ' + Int2Str(Count));
```

### Assuming Every Pascal Type Exists

MPL is Pascal-like but not a complete Pascal implementation.

Do not assume support for types such as:

```text
Int64
Double
Extended
Pointer
Set
Variant
Object
Class
```

unless the target MPLC documentation or compiler test confirms them.

## Suggested Compiler Tests

Test at least:

```text
Boolean declaration and assignment
Char declaration
#nn character references
String default length
String[n] declaration
String indexing
Byte minimum and maximum
Integer negative value
Integer boundaries
Word boundaries
LongInt boundaries
Cardinal boundaries
Real arithmetic
Hexadecimal assignment
File declaration
One-dimensional array
Two-dimensional array
Three-dimensional array
Record declaration
Record containing an array
Declaration-time initialization
Function-result initialization
Numeric-to-string conversion
Overflow behavior
```

Record the environment:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
Scalar types confirmed:
String behavior confirmed:
Numeric boundaries confirmed:
Real behavior confirmed:
File type confirmed:
Arrays confirmed:
Records confirmed:
Notes:
```

## Version Reference

Mystic 1.10 was a major data-type and declaration milestone for MPL. It documented or introduced:

- Pascal-style variable declarations
- Negative numbers
- `Real`
- Explicit `String[n]` lengths
- Local variable declarations
- Declaration-time initialization
- `File`
- Square-bracket array indexing
- Multidimensional arrays up to three dimensions
- Hexadecimal numeric values
- Revised variable memory handling

Repository references:

- [Mystic 1.10 Changes](../documents/mystic-changes/Mystic-1.10.md)
- [MPL Change Index](../documents/mystic-changes/MPL-Change-Index.md)

## Documentation Status

Documented and suitable for reference:

- `Boolean`
- `Char`
- `String`
- `String[n]`
- `Byte`
- `Integer`
- `Word`
- `LongInt`
- `Cardinal`
- `Real`
- `File`
- Arrays up to three dimensions
- Record structures
- Hexadecimal numeric literals
- Declaration-time initialization for non-array variables

Still requiring targeted compiler verification:

- Exact signed `Integer` boundary values
- Exact signed `LongInt` boundary values
- `Real` precision and storage
- Overflow behavior
- Implicit conversion rules
- `Cardinal` support across older Mystic versions
- Array initialization restrictions
- Complex record nesting limits
- 32-bit versus 64-bit compiler differences

## Related Pages

- [Variables](Variables)
- [Constants](Constants)
- [Operators](Operators)
- [Conditional Logic](Conditional-Logic)
- [Arrays](Arrays)
- [Records](Records)
- [Functions](Functions)
- [File Handling](File-Handling)
- [Compiler Behavior](Compiler-Behavior)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- [Mystic BBS Wiki: Mystic Programming Language](https://wiki.mysticbbs.com/doku.php?id=mpl)
- [Mystic BBS Wiki: Mystic 1.10 Changes](https://wiki.mysticbbs.com/doku.php?id=whats_new_110)

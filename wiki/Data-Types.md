# Data Types

Data types define what kind of value a variable can store and which operations can be performed on that value.

MPL follows a Pascal-like syntax, but the exact set of supported types, size limits, conversion behavior, and version-specific restrictions should be verified against the Mystic BBS release being used.

> **Documentation status**
>
> This page is an initial reference and requires compiler verification. Numeric ranges, string limits, and less common types should not be treated as final until tested and documented for a specific Mystic version.

## Common Data Types

The following types are commonly encountered in MPL source code:

| Type | Purpose | Example value |
|---|---|---|
| `String` | Stores text | `'Hello'` |
| `Char` | Stores a single character | `'A'` |
| `Byte` | Stores a small non-negative integer | `25` |
| `Integer` | Stores a whole number | `-42` |
| `LongInt` | Stores a larger whole number, where supported | `100000` |
| `Boolean` | Stores a logical state | `True` |

Additional or specialized types may be available depending on the Mystic version and compiler.

## Declaring Variables

Variables are declared in a `Var` section before the main program block or within another supported scope.

```pascal
Var
  UserName : String;
  Initial  : Char;
  Age      : Integer;
  IsActive : Boolean;

Begin
  UserName := 'Mystic User';
  Initial  := 'M';
  Age      := 25;
  IsActive := True;
End.
```

Each declaration contains:

```text
VariableName : DataType;
```

The assignment operator is:

```pascal
:=
```

See [Variables](Variables) for variable naming and declaration rules.

## String

A `String` stores text.

```pascal
Var
  MessageText : String;

Begin
  MessageText := 'Welcome to Mystic BBS';
  WriteLn(MessageText);
End.
```

String literals are normally enclosed in single quotation marks:

```pascal
'Example text'
```

### Empty strings

An empty string contains no characters:

```pascal
MessageText := '';
```

### Combining strings

MPL commonly uses the plus operator to join strings:

```pascal
WriteLn('Hello, ' + UserName);
```

String concatenation behavior should be tested with the active compiler, especially when combining strings with numeric or Boolean values.

### String limits

The maximum supported string length may depend on the compiler and Mystic version. Do not document a fixed limit until it has been verified.

## Char

A `Char` stores one character.

```pascal
Var
  MenuChoice : Char;

Begin
  MenuChoice := 'A';
End.
```

A character literal is enclosed in single quotation marks, similar to a string literal:

```pascal
'Y'
```

The compiler determines whether a quoted value is valid for a `Char` variable based on its length.

## Byte

A `Byte` stores a small non-negative integer.

```pascal
Var
  ColorNumber : Byte;

Begin
  ColorNumber := 7;
End.
```

Use `Byte` when the value is known to remain within the compiler-supported byte range.

The exact range should be verified before relying on boundary values.

## Integer

An `Integer` stores whole numbers, including negative values where supported.

```pascal
Var
  UserCount : Integer;
  Difference : Integer;

Begin
  UserCount := 42;
  Difference := -5;
End.
```

Integer values can commonly be used with arithmetic and comparison operators:

```pascal
UserCount := UserCount + 1;

If UserCount > 10 Then
  WriteLn('More than ten users');
```

See [Operators](Operators) for arithmetic and comparison behavior.

## LongInt

`LongInt` is used for larger whole-number values in Pascal-like languages and may be available in MPL.

```pascal
Var
  LargeValue : LongInt;

Begin
  LargeValue := 100000;
End.
```

Support, storage size, and value limits must be verified against the target Mystic compiler before using this type in portable examples.

## Boolean

A `Boolean` stores one of two logical values:

```pascal
True
False
```

Example:

```pascal
Var
  IsEnabled : Boolean;

Begin
  IsEnabled := True;

  If IsEnabled Then
    WriteLn('The feature is enabled');
End.
```

Boolean values are commonly used with conditional statements and logical operators.

See [Conditional Logic](Conditional-Logic) and [Operators](Operators).

## Assigning Values

The assigned value must be compatible with the variable's declared type.

Valid examples:

```pascal
UserName := 'Sysop';
Age := 30;
IsActive := True;
```

Potential type mismatches:

```pascal
Age := 'Thirty';
IsActive := 1;
Initial := 'AB';
```

The compiler may reject incompatible assignments or require an explicit conversion function.

## Type Conversion

Converting a value from one type to another should be done with the appropriate MPL function rather than assuming automatic conversion.

Common conversion tasks include:

- Converting a number to text for display
- Converting text input to a number
- Converting a character code to a character
- Converting a Boolean condition into display text

The exact function names and accepted parameter types should be documented on the [Functions](Functions) page after verification.

Avoid relying on implicit conversion until compiler behavior has been tested.

## Comparisons

Values of compatible types can be compared.

```pascal
If UserName = 'Sysop' Then
  WriteLn('Welcome, Sysop');

If UserCount >= 10 Then
  WriteLn('User count is at least ten');
```

Comparing incompatible types may produce a compiler error or unexpected behavior.

## Constants and Literal Values

A literal is a value written directly in source code:

```pascal
'Hello'
'A'
42
-10
True
False
```

Constants are named values that do not change during execution.

See [Constants](Constants) for constant declarations and usage.

## Choosing a Type

Use the narrowest clear type that correctly represents the value:

| Value being stored | Suggested type |
|---|---|
| User-visible text | `String` |
| One menu key | `Char` |
| Small non-negative counter | `Byte`, if the verified range is sufficient |
| General whole number | `Integer` |
| Larger whole number | `LongInt`, if supported and required |
| Enabled or disabled state | `Boolean` |

Do not use a smaller numeric type merely to save space unless its range has been verified and the restriction is intentional.

## Common Errors

### Assigning text to a numeric variable

```pascal
Age := '25';
```

The quoted value is text, not a number.

### Assigning multiple characters to a `Char`

```pascal
MenuChoice := 'Yes';
```

Use a `String` for multiple characters.

### Exceeding a numeric range

A value outside the supported range may produce a compiler error, overflow, or incorrect runtime behavior.

Verify numeric limits before using boundary values.

### Assuming automatic conversion

```pascal
WriteLn('Count: ' + UserCount);
```

This may require conversion of `UserCount` to a string first. Confirm the correct conversion function for the active compiler.

### Confusing assignment and comparison

Assignment uses:

```pascal
:=
```

Comparison commonly uses:

```pascal
=
```

## Verification Checklist

Before marking this page as verified, test and record:

- Supported data-type names
- Whether type names are case-sensitive
- Numeric ranges for `Byte`, `Integer`, and `LongInt`
- Maximum `String` length
- Character encoding used by `Char`
- Boolean literal spelling and capitalization
- Assignment compatibility rules
- Available conversion functions
- Overflow behavior
- Behavior across supported Mystic versions

Suggested verification record:

```text
Mystic version:
Operating system:
Compiler path or build:
Date tested:
Types confirmed:
Numeric ranges confirmed:
String limit confirmed:
Conversion functions confirmed:
Notes:
```

## Related Pages

- [Variables](Variables)
- [Constants](Constants)
- [Operators](Operators)
- [Conditional Logic](Conditional-Logic)
- [Functions](Functions)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

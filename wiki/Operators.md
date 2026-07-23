# Operators

Operators perform calculations, comparisons, assignments, and logical tests on MPL values.

MPL uses Pascal-like operator syntax. The exact operator set, precedence rules, string behavior, and support for less common operators should be verified against the Mystic BBS version being documented.

> **Documentation status**
>
> This page is an initial reference. Core arithmetic, comparison, assignment, and Boolean operators should be tested with the active MPL compiler before the page is marked verified.

## Operator Categories

| Category | Purpose |
|---|---|
| Assignment | Stores a value in a variable |
| Arithmetic | Performs numeric calculations |
| Comparison | Compares two values |
| Logical | Combines or reverses Boolean expressions |
| String | Joins or compares text |
| Grouping | Controls evaluation order |

## Assignment Operator

MPL uses `:=` to assign a value to a variable.

```pascal
UserName := 'Sysop';
UserCount := 10;
IsEnabled := True;
```

Assignment changes the value stored in the variable on the left.

Do not confuse assignment with equality comparison:

```pascal
UserCount := 10;    { assignment }
UserCount = 10      { comparison expression }
```

## Arithmetic Operators

Arithmetic operators work with numeric values.

| Operator | Purpose | Example |
|---|---|---|
| `+` | Addition | `Total := A + B;` |
| `-` | Subtraction | `Total := A - B;` |
| `*` | Multiplication | `Total := A * B;` |
| `/` | Division | `Result := A / B;` |
| `Div` | Integer division, where supported | `Result := A Div B;` |
| `Mod` | Remainder, where supported | `Result := A Mod B;` |

### Addition

```pascal
Var
  Total : Integer;

Begin
  Total := 10 + 5;
  WriteLn('Calculation complete');
End.
```

### Subtraction

```pascal
Remaining := Maximum - Used;
```

### Multiplication

```pascal
Area := Width * Height;
```

### Division

```pascal
Average := Total / Count;
```

Division behavior depends on the operand types and compiler implementation. Verify whether `/` returns an integer, a real-number value, or is restricted to particular numeric types.

### Integer division

Where supported, `Div` discards the remainder:

```pascal
Result := 7 Div 2;
```

The expected mathematical result is `3`, but compiler support must be confirmed.

### Remainder

Where supported, `Mod` returns the remainder after integer division:

```pascal
Remainder := 7 Mod 2;
```

The expected result is `1`, but compiler support must be confirmed.

## Unary Operators

A unary operator acts on one value.

### Positive sign

```pascal
Value := +10;
```

### Negative sign

```pascal
Value := -10;
```

### Boolean negation

```pascal
IsDisabled := Not IsEnabled;
```

Support for unary `+` should be verified. Unary `-` and `Not` are more commonly used.

## Comparison Operators

Comparison operators produce a Boolean result.

| Operator | Meaning | Example |
|---|---|---|
| `=` | Equal to | `UserName = 'Sysop'` |
| `<>` | Not equal to | `UserName <> 'Guest'` |
| `<` | Less than | `Count < 10` |
| `>` | Greater than | `Count > 10` |
| `<=` | Less than or equal to | `Count <= 10` |
| `>=` | Greater than or equal to | `Count >= 10` |

### Equality

```pascal
If UserName = 'Sysop' Then
  WriteLn('Welcome, Sysop');
```

### Inequality

```pascal
If UserName <> 'Guest' Then
  WriteLn('Registered user');
```

### Numeric comparisons

```pascal
If UserCount >= 100 Then
  WriteLn('User milestone reached');
```

The values being compared should use compatible data types.

## Logical Operators

Logical operators combine Boolean values or expressions.

| Operator | Purpose | Example |
|---|---|---|
| `And` | True when both expressions are true | `(A > 0) And (B > 0)` |
| `Or` | True when either expression is true | `(Choice = 'Y') Or (Choice = 'y')` |
| `Not` | Reverses a Boolean value | `Not IsLocked` |
| `Xor` | True when exactly one expression is true, where supported | `A Xor B` |

### And

```pascal
If IsActive And IsValidated Then
  WriteLn('Access granted');
```

Both expressions must evaluate to `True` for the complete condition to be true.

### Or

```pascal
If (Choice = 'Y') Or (Choice = 'y') Then
  WriteLn('Confirmed');
```

The complete condition is true when either expression is true.

### Not

```pascal
If Not IsLocked Then
  WriteLn('The feature is available');
```

### Xor

`Xor` may be available for Boolean or bitwise use, depending on the compiler. Do not rely on it until support has been verified.

## String Operators

### Concatenation

MPL commonly uses `+` to join strings:

```pascal
FullMessage := 'Hello, ' + UserName;
```

Multiple strings can be combined:

```pascal
WriteLn('User: ' + UserName + ' is online.');
```

When combining text with a number or Boolean value, convert the non-string value first if required by the compiler.

Potentially invalid without conversion:

```pascal
WriteLn('Count: ' + UserCount);
```

### String comparison

Strings can commonly be compared with the standard comparison operators:

```pascal
If Command = 'HELP' Then
  WriteLn('Displaying help');
```

Verify:

- Whether comparisons are case-sensitive
- Whether trailing spaces are significant
- Whether locale or code-page settings affect comparison
- Whether `<` and `>` perform lexical comparison

## Grouping with Parentheses

Parentheses make evaluation order explicit.

```pascal
Result := (A + B) * C;
```

Without parentheses:

```pascal
Result := A + B * C;
```

Multiplication would normally be evaluated before addition, but the exact precedence table should be verified.

Parentheses are strongly recommended in compound Boolean conditions:

```pascal
If (Age >= 18) And (IsActive = True) Then
  WriteLn('Access allowed');
```

## Operator Precedence

A typical Pascal-like evaluation order is:

1. Parentheses
2. Unary operators such as `Not` and unary `-`
3. Multiplication, division, `Div`, `Mod`, and possibly `And`
4. Addition, subtraction, `Or`, and possibly `Xor`
5. Comparison operators

This table is provisional. MPL precedence must be verified with compiler tests before it is considered authoritative.

When an expression could be interpreted more than one way, use parentheses.

## Compound Expressions

Example:

```pascal
If ((UserLevel >= 10) And IsActive) Or IsSysop Then
  WriteLn('Access granted');
```

This condition is true when either:

- The user level is at least 10 and the account is active, or
- The user is a sysop

Parentheses make the intended grouping visible.

## Short-Circuit Evaluation

Some languages stop evaluating a logical expression as soon as the final result is known.

Example:

```pascal
If (Count > 0) And (Total Div Count > 10) Then
  WriteLn('Average is greater than ten');
```

If MPL does not short-circuit `And`, the second expression may still be evaluated when `Count` is zero, causing a division error.

Short-circuit behavior must be tested. Until verified, do not depend on it for safety checks.

A safer structure is:

```pascal
If Count > 0 Then
Begin
  If Total Div Count > 10 Then
    WriteLn('Average is greater than ten');
End;
```

## Incrementing and Decrementing

MPL may not support C-style increment and decrement operators such as:

```text
++
--
```

Use explicit assignment:

```pascal
Count := Count + 1;
Count := Count - 1;
```

Do not assume compound assignment operators such as `+=` or `-=` are available.

## Common Errors

### Using `=` for assignment

Incorrect:

```pascal
Count = 10;
```

Correct:

```pascal
Count := 10;
```

### Using `==` for comparison

Incorrect in Pascal-like syntax:

```pascal
If Count == 10 Then
```

Expected form:

```pascal
If Count = 10 Then
```

### Omitting parentheses in mixed conditions

Unclear:

```pascal
If A Or B And C Then
```

Clearer:

```pascal
If A Or (B And C) Then
```

### Dividing by zero

```pascal
Result := Total Div Count;
```

Validate `Count` before division.

### Combining incompatible types

```pascal
MessageText := 'Count: ' + UserCount;
```

Convert the number to text first if required.

### Assuming unsupported operators

Do not assume MPL supports:

```text
++
--
+=
-=
==
!=
&&
||
```

Use documented Pascal-style operators unless compiler testing confirms otherwise.

## Example Program

```pascal
Var
  FirstValue  : Integer;
  SecondValue : Integer;
  Total       : Integer;
  IsGreater   : Boolean;

Begin
  FirstValue := 10;
  SecondValue := 5;

  Total := FirstValue + SecondValue;
  IsGreater := FirstValue > SecondValue;

  If IsGreater And (Total = 15) Then
    WriteLn('The operator test passed');

  WriteLn('|PA');
End.
```

This example demonstrates:

- Assignment with `:=`
- Addition with `+`
- Comparison with `>` and `=`
- Logical combination with `And`
- Parentheses for explicit grouping

Compile and test the example before marking it verified for a Mystic release.

## Verification Checklist

Before marking this page as verified, test and record:

- Assignment operator syntax
- Arithmetic operator support
- `/` result type and division behavior
- `Div` support
- `Mod` support
- Comparison operators
- Boolean `And`, `Or`, and `Not`
- `Xor` support
- String concatenation
- String comparison behavior
- Operator precedence
- Short-circuit evaluation
- Numeric overflow behavior
- Division-by-zero behavior
- Type-conversion requirements

Suggested verification record:

```text
Mystic version:
Operating system:
Compiler path or build:
Date tested:
Arithmetic operators confirmed:
Comparison operators confirmed:
Logical operators confirmed:
String behavior confirmed:
Precedence confirmed:
Short-circuit behavior confirmed:
Notes:
```

## Related Pages

- [Data Types](Data-Types)
- [Variables](Variables)
- [Constants](Constants)
- [Conditional Logic](Conditional-Logic)
- [Loops](Loops)
- [Functions](Functions)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

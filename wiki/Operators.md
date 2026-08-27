# Operators

Operators perform assignments, calculations, comparisons, logical tests, string concatenation, and bitwise operations on MPL values.

Mystic 1.10 substantially changed MPL expression handling. It added proper arithmetic order of operations, parentheses, a power operator, hexadecimal values, and full bitwise operations, while also changing some historical operator spellings.

> **Documentation status**
>
> Core operators on this page are taken from the Mystic MPL reference and Mystic 1.10 change history. There are conflicts between the current quick-reference page and versioned 1.10 notes for some syntax, especially the not-equal operator. Those conflicts are called out instead of being silently resolved.

## Operator Categories

| Category | Purpose |
|---|---|
| Assignment | Stores a value |
| Arithmetic | Performs numeric calculations |
| Comparison | Compares values |
| Logical | Combines or reverses Boolean expressions |
| Bitwise | Manipulates individual bits |
| String | Concatenates text |
| Grouping | Controls evaluation order |

## Assignment Operator

MPL uses `:=` for assignment:

```pascal
Count := 10;
UserName := 'Sysop';
IsReady := True;
```

The value on the right is stored in the variable on the left.

Do not confuse assignment with equality comparison:

```pascal
Count := 10;      // assignment

If Count = 10 Then
  WriteLn('Ten'); // comparison
```

## Arithmetic Operators

Modern MPL documentation supports these arithmetic operators:

| Operator | Meaning | Example |
|---|---|---|
| `+` | Addition | `A + B` |
| `-` | Subtraction | `A - B` |
| `*` | Multiplication | `A * B` |
| `/` | Division | `A / B` |
| `%` | Modulus / remainder | `A % B` |
| `^` | Power | `A ^ B` |

### Addition

```pascal
Total := A + B;
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

Division behavior depends on operand types. Test integer-versus-`Real` results with the target compiler rather than assuming standard Pascal behavior.

Always protect against division by zero.

### Modulus

Mystic 1.10 explicitly changed the old `MOD` operator to `%`:

```pascal
Remainder := 10 % 8;
```

Historical form:

```text
10 MOD 8
```

Modern 1.10 form:

```pascal
10 % 8
```

Do not document `Mod` as the preferred modern MPL form unless a target compiler test confirms it.

### Power

Mystic 1.10 added the `^` power operator:

```pascal
Value := 2 ^ 3;
```

The mathematical result is 8.

Exact type promotion and `Real` behavior should be compiler-tested.

## Negative Numbers and Unary Minus

Mystic 1.10 added support for negative numbers.

```pascal
Difference := -5;
```

Unary minus can also be used in expressions:

```pascal
Result := -Value;
```

Unary plus should not be assumed unless verified.

## Arithmetic Order of Operations

Mystic 1.10 added proper arithmetic order of operations and support for parentheses.

```pascal
Result := 2 + 3 * 4;
```

Multiplication is evaluated before addition.

Parentheses override the normal grouping:

```pascal
Result := (2 + 3) * 4;
```

This change is important when migrating older MPL programs because pre-1.10 expression evaluation could produce different results.

### Recommended Practice

Even with precedence support, use parentheses when an expression is not immediately obvious:

```pascal
Result := (BaseValue + Adjustment) * Multiplier;
```

The 1.10 notes establish proper arithmetic precedence, but they do not provide a complete authoritative precedence table for every Boolean and bitwise combination. Complex mixed expressions should therefore use explicit parentheses.

## Comparison Operators

Comparison operators produce a Boolean result.

Common comparison forms include:

| Operator | Meaning |
|---|---|
| `=` | Equal |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal |
| `>=` | Greater than or equal |

Examples:

```pascal
If Count = 10 Then
  WriteLn('Ten');

If Count < Maximum Then
  WriteLn('Below maximum');

If UserLevel >= 20 Then
  WriteLn('Validated');
```

## Not-Equal Operator: Version Conflict

The Mystic documentation is internally inconsistent about the not-equal operator.

The Mystic 1.10 Alpha 34 change history states that MPL/IPL not-equal syntax changed from:

```text
<>
```

to:

```text
!=
```

However, the current Mystic MPL quick-reference page still contains examples using:

```pascal
<>
```

For example, current reference material shows bit checks and file-I/O examples using `<> 0`.

Because both forms appear in official Mystic documentation, this wiki does **not** declare one form universally correct without a compiler test.

For the target MPLC build, test both:

```pascal
If A <> B Then
  WriteLn('Not equal');
```

and:

```pascal
If A != B Then
  WriteLn('Not equal');
```

Record which form compiles and behaves correctly.

## Logical Operators

Boolean expressions use logical operators such as:

| Operator | Meaning |
|---|---|
| `And` | Both conditions are true |
| `Or` | At least one condition is true |
| `Not` | Reverses a Boolean value |

### `And`

```pascal
If IsActive And IsValidated Then
  WriteLn('Access granted');
```

### `Or`

```pascal
If (Choice = 'Y') Or (Choice = 'y') Then
  WriteLn('Confirmed');
```

### `Not`

```pascal
If Not IsLocked Then
  WriteLn('Available');
```

See [Conditional Logic](Conditional-Logic) for compound conditions and short-circuit safety.

## Short-Circuit Evaluation

Do not assume `And` or `Or` short-circuits unless the target compiler has been tested.

Potentially unsafe:

```pascal
If (Count > 0) And ((Total / Count) > 10) Then
  WriteLn('Average is high');
```

If both sides are evaluated when `Count = 0`, division by zero may still occur.

Safer:

```pascal
If Count > 0 Then
Begin
  If (Total / Count) > 10 Then
    WriteLn('Average is high');
End;
```

## Bitwise Operators

Mystic 1.10 added full bitwise math.

The documented operators are:

| Operator | Meaning |
|---|---|
| `AND` | Bitwise AND |
| `OR` | Bitwise OR |
| `XOR` | Bitwise exclusive OR |
| `SHL` | Shift left |
| `SHR` | Shift right |

MPL is generally case-insensitive for language keywords, so documentation may show these as uppercase or title case. Compiler behavior should still be tested if exact token casing matters in a particular build.

## Bitwise `AND`

A common use is testing a flag:

```pascal
Const
  UserDeleted = $00000004;

Begin
  GetThisUser;

  If (UserFlags And UserDeleted) <> 0 Then
    WriteLn('User is deleted');
End;
```

Because of the not-equal documentation conflict described above, verify the comparison spelling with the target compiler.

## Bitwise `OR`

Bitwise `OR` combines set bits:

```pascal
CombinedFlags := FlagA Or FlagB;
```

Test assignment and operand type rules before using this in a verified example.

## Bitwise `XOR`

Bitwise exclusive OR is documented by Mystic 1.10:

```pascal
Result := ValueA Xor ValueB;
```

Exact Boolean-versus-integer overload behavior should be tested.

## Shift Left and Shift Right

Mystic 1.10 documents:

```text
SHL - bitwise shift left
SHR - bitwise shift right
```

Example forms:

```pascal
Value := Value Shl 1;
Value := Value Shr 1;
```

Verify accepted operand types, shift counts, signed-value behavior, and overflow before marking these examples verified.

## Bit Helper Functions

Mystic 1.10 also introduced helper functions for bit manipulation:

```text
BitCheck
BitToggle
BitSet
```

Example:

```pascal
If BitCheck(3, UserFlags) Then
  WriteLn('User is marked deleted');
```

These functions can be easier to read than manual bitwise expressions for known Mystic record flags.

See [Functions](Functions) for exact signatures.

## Hexadecimal Values

Hexadecimal numeric literals begin with `$`:

```pascal
Value := $10;
```

They can participate in numeric comparisons:

```pascal
If Value = $10 Then
  WriteLn('Value is 16 decimal');
```

And constant declarations:

```pascal
Const
  Mask = $1F;
```

See [Constants](Constants).

## String Concatenation

MPL uses `+` to concatenate strings:

```pascal
FullMessage := 'Hello, ' + UserName;
```

Several strings can be combined:

```pascal
WriteLn('User: ' + UserName + ' is online.');
```

Numeric values normally require explicit string conversion:

```pascal
WriteLn('Count: ' + Int2Str(Count));
```

Do not assume automatic conversion of numeric or Boolean values to strings.

## String Comparison

Strings can be compared for equality:

```pascal
If Command = 'HELP' Then
  WriteLn('Displaying help');
```

Version-specific testing should determine:

- Case sensitivity
- Trailing-space behavior
- Ordering with `<` and `>`
- Code-page effects
- Behavior with fixed-length `String[n]` values

## Character Comparison

Characters can be compared directly:

```pascal
If Choice = 'Q' Then
  WriteLn('Quit');
```

Mystic 1.10 also allows numeric character references:

```pascal
If Choice = #32 Then
  WriteLn('Space');
```

## Parentheses

Parentheses are supported in modern MPL expressions:

```pascal
Result := (A + B) * C;
```

They are strongly recommended in compound Boolean and bitwise expressions:

```pascal
If ((UserLevel >= 20) And IsActive) Or IsSysop Then
  WriteLn('Access granted');
```

```pascal
If (UserFlags And UserDeleted) <> 0 Then
  WriteLn('Deleted');
```

## Pascal Syntax and Alternate Syntax

Mystic 1.10 added a second MPL syntax mode that is closer to C and modeled after Iniquity's programming language.

This wiki uses **Pascal-style MPL as the primary documented syntax** unless a page explicitly discusses the alternate mode.

Do not mix C-like operators into Pascal examples merely because the alternate syntax exists.

Examples that should not be assumed in Pascal mode without verification include:

```text
==
&&
||
++
--
+=
-=
```

The `!=` operator is an exception that requires special attention because the Mystic 1.10 Alpha 34 history explicitly says the MPL/IPL not-equal operator changed to `!=`.

## Compound Assignment

Do not assume operators such as these are available in Pascal-style MPL:

```text
+=
-=
*=
/=
```

Use explicit assignment:

```pascal
Count := Count + 1;
Total := Total + Amount;
```

## Increment and Decrement

Do not assume C-style increment or decrement operators:

```text
++
--
```

Use:

```pascal
Count := Count + 1;
Count := Count - 1;
```

## Common Errors

### Using `=` for Assignment

Incorrect:

```pascal
Count = 10;
```

Correct:

```pascal
Count := 10;
```

### Using Historical `MOD`

Older form:

```text
A MOD B
```

Mystic 1.10 documented replacement:

```pascal
A % B
```

### Assuming `Div` Exists

Standard Pascal commonly provides `Div`, but the current Mystic quick reference documents `/` and `%`, not `Div`.

Do not use:

```pascal
A Div B
```

in reference examples until compiler support has been verified.

### Assuming a Not-Equal Spelling

Official Mystic documentation contains both:

```text
<>
!=
```

Test the target MPLC build and record the result.

### Dividing by Zero

Unsafe:

```pascal
Average := Total / Count;
```

when `Count` can be zero.

Validate first.

### Mixing Strings and Numbers

Potentially invalid:

```pascal
WriteLn('Count: ' + Count);
```

Use explicit conversion:

```pascal
WriteLn('Count: ' + Int2Str(Count));
```

### Relying on Undocumented Precedence

Even though Mystic 1.10 added proper arithmetic order of operations, mixed Boolean and bitwise precedence is not fully documented in the quick reference.

Prefer:

```pascal
If ((Flags And Mask) <> 0) And IsActive Then
```

rather than relying on implicit grouping.

## Suggested Compiler Tests

Test at least:

```text
:= assignment
+ addition
- subtraction
unary -
* multiplication
/ division with Integer operands
/ division with Real operands
% modulus
^ power
= equality
< and >
<= and >=
<> not equal
!= not equal
And Boolean
Or Boolean
Not Boolean
AND bitwise
OR bitwise
XOR bitwise
SHL
SHR
hexadecimal operands
string concatenation
string equality
character comparison
parentheses
arithmetic precedence
Boolean precedence
bitwise precedence
short-circuit behavior
division by zero
Div keyword acceptance
compound assignment rejection or support
alternate syntax behavior
```

Record the environment:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
Arithmetic confirmed:
% confirmed:
^ confirmed:
<> result:
!= result:
Bitwise confirmed:
String behavior confirmed:
Arithmetic precedence confirmed:
Boolean precedence confirmed:
Short-circuit behavior confirmed:
Notes:
```

## Version Reference

Mystic 1.10 was the key modernization release for MPL operators and expressions. It documented:

- Proper arithmetic order of operations
- Parentheses
- Negative numbers
- `Real` arithmetic
- `%` replacing historical `MOD`
- `^` power operator
- Hexadecimal numeric values
- Full bitwise `AND`, `OR`, `XOR`, `SHL`, and `SHR`
- A later 1.10 Alpha 34 change from `<>` to `!=` for not-equal syntax
- An alternate C-like syntax mode

Repository references:

- [Mystic 1.10 Changes](../documents/mystic-changes/Mystic-1.10.md)
- [MPL Change Index](../documents/mystic-changes/MPL-Change-Index.md)

## Documentation Status

Documented and suitable for reference:

- `:=`
- `+`
- `-`
- `*`
- `/`
- `%`
- `^`
- `=`
- `<`
- `>`
- `<=`
- `>=`
- Boolean `And`, `Or`, `Not`
- Bitwise `AND`, `OR`, `XOR`, `SHL`, `SHR`
- Parentheses
- Arithmetic precedence
- Hexadecimal operands
- String concatenation with `+`

Still requiring targeted compiler verification:

- `<>` versus `!=` in the target MPLC build
- Exact full precedence table
- Short-circuit behavior
- Integer division result behavior with `/`
- `Real` promotion rules
- `Div` support
- Bitwise behavior on signed values
- Shift overflow behavior
- String ordering comparisons
- Alternate syntax operators
- 32-bit versus 64-bit compiler differences

## Related Pages

- [Data Types](Data-Types)
- [Variables](Variables)
- [Constants](Constants)
- [Conditional Logic](Conditional-Logic)
- [Loops](Loops)
- [Functions](Functions)
- [Compiler Behavior](Compiler-Behavior)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References


# Constants

Constants give a name to a fixed value that is intended to remain unchanged while an MPL program runs.

They are useful for values such as:

- Menu choices
- Numeric limits
- Bit masks and flag values
- Repeated text
- Configuration values that should not be modified by program logic

> **Documentation status**
>
> `Const` is a documented MPL language feature. Some details in the official MPL reference appear to preserve historical MPE-era limitations, so assignment behavior, declaration separators, supported constant value types, and scope should be verified with the exact MPLC build being documented.

## Basic Syntax

The official MPL reference documents constants with a `Const` declaration block.

String constant:

```pascal
Const
  SomeStr = 'Hello World!'
```

Numeric constant:

```pascal
Const
  SomeNum = 69
```

A constant is referenced by its identifier after declaration:

```pascal
WriteLn(SomeStr);
```

## Constants Compared with Variables

A variable stores a value that program logic may change:

```pascal
Var
  Count : Integer;

Begin
  Count := 1;
  Count := Count + 1;
End;
```

A constant represents a fixed named value:

```pascal
Const
  MaximumAttempts = 3
```

Use a constant when the value has a stable meaning throughout the program.

## String Constants

The documented string form is:

```pascal
Const
  ProgramName = 'Twinkle Utility'
```

A named string avoids repeating the same literal throughout a program.

Instead of:

```pascal
WriteLn('Twinkle Utility');
WriteLn('Starting Twinkle Utility');
```

A program can define the text once:

```pascal
Const
  ProgramName = 'Twinkle Utility'

Begin
  WriteLn(ProgramName);
  WriteLn('Starting ' + ProgramName);
End;
```

String concatenation involving constants should be tested with the target compiler before an example is marked verified.

## Numeric Constants

Numeric constants are documented directly with a numeric literal:

```pascal
Const
  MaximumAttempts = 3
```

They are especially useful for limits and fixed numeric meanings:

```pascal
Const
  MinimumLevel = 20

Begin
  If UserLevel >= MinimumLevel Then
    WriteLn('Access granted');
End;
```

## Hexadecimal Constants

Mystic 1.10 added hexadecimal values for numeric assignments, numeric evaluation, and numeric constants.

A hexadecimal value begins with `$`:

```pascal
Const
  MyHexValue = $1F;
```

The same notation can be used directly in a numeric expression:

```pascal
Value := $10;

If Value = $10 Then
  WriteLn('Value is 16 in decimal');
```

Hexadecimal constants are particularly useful for flags and bit masks.

## Constants as Bit Masks

The official MPL reference demonstrates constants with bitwise operations:

```pascal
Const
  UserDeleted = $00000004;

Begin
  GetThisUser;

  If (UserFlags And UserDeleted) <> 0 Then
    WriteLn('User is deleted');
End;
```

Using a named constant is clearer than embedding a raw hexadecimal value throughout the source.

Less descriptive:

```pascal
If (UserFlags And $00000004) <> 0 Then
```

Clearer:

```pascal
If (UserFlags And UserDeleted) <> 0 Then
```

See [Operators](Operators) for bitwise operators and [Conditional Logic](Conditional-Logic) for using bit masks in conditions.

## Multiple Constants

The current official MPL reference states that constant declarations can be separated with a comma:

```pascal
Const
  SomeNum = 69,
  SomeStr = 'Hello World!'
```

Because MPL syntax changed significantly during the Mystic 1.10 language redesign, declaration punctuation should be verified with the active MPLC version before establishing a project-wide style.

For documentation examples, prefer clear one-per-line declarations unless demonstrating a compiler-verified multi-declaration form.

## Naming Constants

Choose names that explain what the value means.

Poor:

```pascal
Const
  X = 3
```

Better:

```pascal
Const
  MaximumAttempts = 3
```

Useful naming patterns include:

```text
MaximumAttempts
MinimumUserLevel
UserDeleted
DefaultWidth
ProgramName
```

The wiki does not require all-uppercase constant names. Existing Mystic source and documentation use mixed naming conventions, so consistency within a project is more important than imposing a style not required by MPL.

## Constants in Conditions

A numeric constant can make a conditional expression easier to understand:

```pascal
Const
  MinimumLevel = 20

Begin
  If UserLevel >= MinimumLevel Then
    WriteLn('Access granted');
End;
```

A hexadecimal constant can make a flag check self-documenting:

```pascal
Const
  UserDeleted = $00000004;

Begin
  If (UserFlags And UserDeleted) <> 0 Then
    WriteLn('Deleted user');
End;
```

## Constants and Expressions

Constants are intended to replace literal values in expressions:

```pascal
If Attempts >= MaximumAttempts Then
  WriteLn('No attempts remaining');
```

The official MPL documentation contains a historical warning that constants are not recognized in certain assignment contexts. That warning originated with the older MPE implementation and remains present on the current MPL reference page.

Because of that documentation conflict, do not assume every use below is portable without testing:

```pascal
Value := SomeNum;
Text  := SomeStr;
```

Record the result with the exact compiler version before documenting direct constant-to-variable assignment as verified behavior.

## Historical Note

Constants were added to the older Mystic programming engine before the major MPL redesign. Early documentation explicitly described limitations on where constant values could be used.

Mystic 1.10 later overhauled MPL to follow Pascal syntax much more closely and added hexadecimal values to numeric constant declarations.

This history is important because some current reference text still contains older MPE terminology and limitations. When the current reference and a tested modern compiler disagree, record the compiler version and test result rather than silently assuming either behavior applies universally.

## Recommended Use

Good uses for constants include:

### Access levels

```pascal
Const
  ValidatedLevel = 20
```

### Retry limits

```pascal
Const
  MaximumAttempts = 3
```

### Display values

```pascal
Const
  DefaultWidth = 80
```

### Program text

```pascal
Const
  ProgramName = 'Twinkle Utility'
```

### Bit masks

```pascal
Const
  UserDeleted = $00000004;
```

## Avoid Magic Numbers

A magic number is a literal whose meaning is not obvious from the code.

Harder to understand:

```pascal
If Attempts >= 3 Then
  WriteLn('Locked');
```

Clearer:

```pascal
Const
  MaximumAttempts = 3

Begin
  If Attempts >= MaximumAttempts Then
    WriteLn('Locked');
End;
```

The named constant documents why the value exists.

## Constants and Data Types

MPL's documented `Const` syntax does not declare a type explicitly:

```pascal
Const
  SomeStr = 'Hello World!'
  SomeNum = 69
```

The literal determines how the compiler interprets the value.

Documented examples confirm at least:

- String constants
- Numeric constants
- Hexadecimal numeric constants

Additional constant forms such as Boolean, character, expression-based, or typed constants should be compiler-tested before being presented as supported language features.

See [Data Types](Data-Types) for MPL variable types and numeric ranges.

## Scope

The official quick-reference material does not clearly define all modern scoping rules for `Const` declarations.

Until tested, avoid making broad assumptions about:

- Constants declared inside procedures
- Constants declared inside functions
- Constants declared inside nested routines
- Shadowing a constant with another identifier
- Duplicate constant names in different scopes

A compiler test should determine which declaration locations are accepted by the target MPLC release.

## Common Mistakes

### Using `:=` in a Constant Declaration

Variable assignment uses `:=`:

```pascal
Count := 10;
```

Documented constant declarations use `=`:

```pascal
Const
  MaximumAttempts = 3
```

### Treating a Constant Like a Variable

Do not design program logic that expects a constant to change during execution.

If the value must change, use a variable instead.

### Repeating Raw Flag Values

Avoid:

```pascal
If (UserFlags And $00000004) <> 0 Then
```

Prefer:

```pascal
Const
  UserDeleted = $00000004;
```

Then:

```pascal
If (UserFlags And UserDeleted) <> 0 Then
```

### Assuming All Pascal Constant Syntax Works

MPL is Pascal-like but is not a complete Pascal implementation.

Do not assume support for features such as:

```text
Typed constants
Constant expressions
Enumerated constants
Set constants
Array constants
Record constants
Local constants
```

until those forms have been tested with MPLC.

### Ignoring Historical Compiler Limitations

The official MPL reference still warns that constants may not be recognized in some assignment contexts. Treat this as a verification requirement rather than deleting the warning from modern documentation without testing.

## Suggested Test Program

A small compiler test should cover each form independently:

```text
String constant declaration
Decimal numeric constant declaration
Hexadecimal constant declaration
Constant in WriteLn
Constant in numeric comparison
Constant in string expression
Constant in bitwise expression
Constant assigned to a numeric variable
Constant assigned to a string variable
Multiple constants in one Const block
Comma-separated constants
Semicolon-terminated constants
Const inside a procedure
Const inside a function
Duplicate identifier handling
```

Record the environment:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
String constant result:
Numeric constant result:
Hex constant result:
Assignment result:
Local declaration result:
Declaration separator result:
Notes:
```

## Version Reference

Constants predate Mystic 1.10, but the 1.10 MPL redesign is important to modern constant syntax and behavior.

Mystic 1.10 specifically documented hexadecimal values in numeric constant variables:

```pascal
Const
  MyHexValue = $1F;
```

Repository references:

- [Mystic 1.10 Changes](../documents/mystic-changes/Mystic-1.10.md)
- [MPL Change Index](../documents/mystic-changes/MPL-Change-Index.md)

## Documentation Status

Documented and suitable for reference:

- `Const` declaration block
- String constants
- Decimal numeric constants
- Hexadecimal numeric constants
- Constants used as named values in expressions
- Constants used as bit masks

Still requiring compiler verification:

- Direct constant-to-variable assignment in modern MPL
- Boolean constants
- Character constants
- Constant expressions
- Typed constants
- Local constant declarations
- Constant scope and shadowing
- Exact declaration separator rules
- Differences between historical MPE behavior and modern MPX/MPLC behavior
- 32-bit versus 64-bit compiler differences

## Related Pages

- [Variables](Variables)
- [Data Types](Data-Types)
- [Operators](Operators)
- [Conditional Logic](Conditional-Logic)
- [MPL Syntax](MPL-Syntax)
- [Compiler Behavior](Compiler-Behavior)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- [Mystic BBS Wiki: Mystic Programming Language](https://wiki.mysticbbs.com/doku.php?id=mpl)
- [Mystic BBS Wiki: Mystic 1.06 Changes](https://wiki.mysticbbs.com/doku.php?id=whats_new_106)
- [Mystic BBS Wiki: Mystic 1.10 Changes](https://wiki.mysticbbs.com/doku.php?id=whats_new_110)

# Conditional Logic

Conditional logic allows an MPL program to make decisions and execute different statements based on whether an expression evaluates to `True` or `False`.

MPL uses Pascal-style conditional syntax. The exact behavior of compound conditions, `Case` statements, string comparisons, and compiler-specific extensions should be verified against the Mystic BBS release being documented.

> **Documentation status**
>
> This page is an initial reference. Examples and edge cases should be tested with the active MPL compiler before the page is marked verified.

## Boolean Conditions

A condition is an expression that evaluates to a Boolean result.

Examples:

```pascal
UserLevel >= 10
UserName = 'Sysop'
IsActive = True
Not IsLocked
```

Conditions are commonly built with comparison and logical operators.

See [Operators](Operators) for operator details.

## If Then

Use `If ... Then` to execute a statement only when a condition is true.

```pascal
If UserLevel >= 10 Then
  WriteLn('Access granted');
```

When the condition is false, the statement is skipped.

## If Then with a Block

Use `Begin` and `End` when more than one statement belongs to the condition.

```pascal
If UserLevel >= 10 Then
Begin
  WriteLn('Access granted');
  WriteLn('Loading restricted menu');
End;
```

Using explicit blocks is recommended even when a condition currently contains one statement. It makes later changes safer and easier to read.

## If Else

Use `Else` to provide an alternate path when the condition is false.

```pascal
If UserLevel >= 10 Then
Begin
  WriteLn('Access granted');
End
Else
Begin
  WriteLn('Access denied');
End;
```

The `Else` belongs to the nearest unmatched `If`.

Use `Begin` and `End` to make the intended structure clear.

## Else If

Multiple conditions can be checked in sequence.

```pascal
If UserLevel >= 100 Then
Begin
  WriteLn('Sysop access');
End
Else If UserLevel >= 50 Then
Begin
  WriteLn('Co-sysop access');
End
Else If UserLevel >= 10 Then
Begin
  WriteLn('Member access');
End
Else
Begin
  WriteLn('Guest access');
End;
```

Conditions are evaluated from top to bottom. The first matching branch is executed.

Order the most specific or highest-priority checks first.

## Nested Conditions

An `If` statement can appear inside another conditional block.

```pascal
If IsActive Then
Begin
  If UserLevel >= 10 Then
  Begin
    WriteLn('Active member');
  End
  Else
  Begin
    WriteLn('Active guest');
  End;
End;
```

Deep nesting can become difficult to follow. Prefer smaller procedures or clearer compound conditions when practical.

## Compound Conditions

Logical operators combine conditions.

### And

Both expressions must be true.

```pascal
If IsActive And (UserLevel >= 10) Then
Begin
  WriteLn('Active member access granted');
End;
```

### Or

At least one expression must be true.

```pascal
If (Choice = 'Y') Or (Choice = 'y') Then
Begin
  WriteLn('Confirmed');
End;
```

### Not

`Not` reverses a Boolean value.

```pascal
If Not IsLocked Then
Begin
  WriteLn('The feature is available');
End;
```

Use parentheses to make mixed logical expressions explicit.

```pascal
If ((UserLevel >= 10) And IsActive) Or IsSysop Then
Begin
  WriteLn('Access granted');
End;
```

## Comparison Examples

### Numeric comparison

```pascal
If MessageCount > 0 Then
  WriteLn('You have messages');
```

### String comparison

```pascal
If UserName = 'Sysop' Then
  WriteLn('Welcome, Sysop');
```

### Character comparison

```pascal
If MenuChoice = 'Q' Then
  WriteLn('Exiting');
```

### Boolean test

```pascal
If IsEnabled Then
  WriteLn('Feature enabled');
```

A Boolean variable usually does not need to be compared explicitly with `True`.

This is clearer:

```pascal
If IsEnabled Then
```

Than:

```pascal
If IsEnabled = True Then
```

Both forms should be verified with the active compiler.

## Case Statements

A `Case` statement selects one branch based on a single expression.

```pascal
Case MenuChoice Of
  'A': WriteLn('Add selected');
  'E': WriteLn('Edit selected');
  'Q': WriteLn('Quit selected');
End;
```

`Case` is useful when several branches compare the same value.

## Case with Blocks

Use `Begin` and `End` when a branch contains multiple statements.

```pascal
Case MenuChoice Of
  'A':
  Begin
    WriteLn('Add selected');
    WriteLn('Opening add screen');
  End;

  'E':
  Begin
    WriteLn('Edit selected');
    WriteLn('Opening edit screen');
  End;
End;
```

## Case with Multiple Values

Some Pascal-like compilers allow multiple values to share a branch.

```pascal
Case MenuChoice Of
  'Y', 'y': WriteLn('Confirmed');
  'N', 'n': WriteLn('Cancelled');
End;
```

MPL support for multiple values in one branch must be verified.

## String Case Support

Modern MPL compilers may support string expressions in `Case` statements.

Example form:

```pascal
Case Command Of
  'HELP': WriteLn('Displaying help');
  'QUIT': WriteLn('Exiting');
End;
```

String `Case` behavior is version-specific and must be tested before being documented as portable.

Verify:

- Whether string values are supported
- Whether comparison is case-sensitive
- Whether duplicate values are rejected
- Whether an `Else` branch is supported

## Default or Else Branch

Some MPL compiler versions may support an `Else` branch inside a `Case` statement.

Example form:

```pascal
Case MenuChoice Of
  'A': WriteLn('Add selected');
  'E': WriteLn('Edit selected');
Else
  WriteLn('Unknown selection');
End;
```

This syntax must be verified before use in a tested example.

## Short-Circuit Behavior

Do not assume logical conditions stop evaluation as soon as the final result is known.

Potentially unsafe:

```pascal
If (Count > 0) And ((Total Div Count) > 10) Then
  WriteLn('Average is greater than ten');
```

If MPL evaluates both expressions, division by zero may still occur when `Count` is zero.

Safer structure:

```pascal
If Count > 0 Then
Begin
  If (Total Div Count) > 10 Then
  Begin
    WriteLn('Average is greater than ten');
  End;
End;
```

Short-circuit behavior should be tested and recorded for each supported compiler version.

## Common Errors

### Using assignment in a condition

Incorrect:

```pascal
If UserLevel := 10 Then
```

Correct:

```pascal
If UserLevel = 10 Then
```

### Using C-style comparison operators

Incorrect:

```pascal
If UserLevel == 10 Then
If UserLevel != 10 Then
```

Use Pascal-style operators:

```pascal
If UserLevel = 10 Then
If UserLevel <> 10 Then
```

### Omitting a block for multiple statements

Incorrect structure:

```pascal
If IsActive Then
  WriteLn('Active');
  WriteLn('Welcome');
```

Only the first statement is controlled by the condition.

Use a block:

```pascal
If IsActive Then
Begin
  WriteLn('Active');
  WriteLn('Welcome');
End;
```

### Ambiguous Else placement

Nested conditions without blocks can make it unclear which `If` owns the `Else`.

Prefer explicit blocks.

### Assuming case-insensitive string comparison

Do not assume these values compare equally:

```text
Sysop
SYSOP
sysop
```

Test and document the behavior of the active compiler.

## Example Program

```pascal
Var
  UserLevel : Integer;
  IsActive  : Boolean;

Begin
  UserLevel := 20;
  IsActive := True;

  If IsActive Then
  Begin
    If UserLevel >= 50 Then
    Begin
      WriteLn('Administrative access');
    End
    Else If UserLevel >= 10 Then
    Begin
      WriteLn('Member access');
    End
    Else
    Begin
      WriteLn('Guest access');
    End;
  End
  Else
  Begin
    WriteLn('Account is inactive');
  End;

  WriteLn('|PA');
End.
```

This example demonstrates:

- Boolean conditions
- Nested `If` statements
- `Else If`
- `Else`
- Comparison operators
- Explicit `Begin` and `End` blocks

## Verification Checklist

Before marking this page verified, test and record:

- `If ... Then` syntax
- `Else` syntax
- `Else If` syntax
- Nested-condition behavior
- `Begin` and `End` requirements
- Numeric comparisons
- String comparisons
- Character comparisons
- Boolean conditions
- `And`, `Or`, and `Not`
- Logical operator precedence
- Short-circuit behavior
- `Case` statement syntax
- Multiple values in one `Case` branch
- String `Case` support
- Default or `Else` branch support in `Case`

Suggested verification record:

```text
Mystic version:
Operating system:
Compiler path or build:
Date tested:
If/Else confirmed:
Logical operators confirmed:
Case syntax confirmed:
String Case confirmed:
Short-circuit behavior confirmed:
Notes:
```

## Related Pages

- [Operators](Operators)
- [Data Types](Data-Types)
- [Variables](Variables)
- [Loops](Loops)
- [Procedures](Procedures)
- [Functions](Functions)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

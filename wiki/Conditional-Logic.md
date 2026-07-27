# Conditional Logic

Conditional logic allows an MPL program to make decisions and execute different statements based on whether an expression evaluates to `True` or `False`.

This page covers:

- Boolean values and expressions
- Comparison and logical operators
- `If`, `Else If`, and `Else`
- Nested conditions and guard conditions
- `Case` statements
- Conditions using functions and bit flags
- Common control-flow mistakes

> **Documentation status**
>
> This page documents modern Pascal-style MPL syntax used by Mystic 1.10 and later. Compiler-sensitive behavior, including logical short-circuiting and string comparison details, should still be tested with the exact Mystic and MPLC release in use.

## Boolean Conditions

A condition is an expression that evaluates to a Boolean result.

```pascal
UserLevel >= 10
UserName = 'Sysop'
IsActive
Not IsLocked
```

MPL supports the Boolean values:

```pascal
True
False
```

A Boolean variable can be used directly:

```pascal
Var IsReady : Boolean = True;

Begin
  If IsReady Then
    WriteLn('Ready');
End;
```

An explicit comparison with `True` is normally unnecessary.

Prefer:

```pascal
If IsReady Then
```

Instead of:

```pascal
If IsReady = True Then
```

To test for `False`, use `Not`:

```pascal
If Not IsReady Then
  WriteLn('Not ready');
```

## Comparison Operators

Comparison operators compare two values and return a Boolean result.

| Operator | Meaning |
|---|---|
| `=` | Equal |
| `<>` | Not equal |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal |
| `>=` | Greater than or equal |

Examples:

```pascal
If Count = 0 Then
  WriteLn('Count is zero');

If Count <> 0 Then
  WriteLn('Count is not zero');

If UserLevel >= 20 Then
  WriteLn('Validated user');

If Attempts < MaximumAttempts Then
  WriteLn('Another attempt is allowed');
```

The assignment operator is different:

```pascal
Count := 10;
```

Do not use `=` when assigning a value.

## Logical Operators

Logical operators combine or reverse Boolean expressions.

| Operator | Meaning |
|---|---|
| `And` | Both expressions must be true |
| `Or` | At least one expression must be true |
| `Not` | Reverses a Boolean result |

### `And`

```pascal
If IsActive And HasAccess Then
  WriteLn('The user may continue');
```

Both expressions must evaluate to `True`.

### `Or`

```pascal
If (Choice = 'Y') Or (Choice = 'y') Then
  WriteLn('Yes selected');
```

At least one expression must evaluate to `True`.

### `Not`

```pascal
If Not FileExist(FileName) Then
  WriteLn('File not found');
```

`Not` reverses the Boolean result returned by `FileExist`.

## Parentheses and Evaluation Order

Use parentheses to make compound expressions clear and to control grouping.

```pascal
If (UserLevel >= 20) And (IsActive Or IsSysop) Then
  WriteLn('Access granted');
```

Without parentheses, a mixed expression may be difficult to read or may not be grouped as intended.

Harder to read:

```pascal
If UserLevel >= 20 And IsActive Or IsSysop And Not IsDeleted Then
  WriteLn('Access granted');
```

Clearer:

```pascal
If ((UserLevel >= 20) And IsActive) Or
   (IsSysop And Not IsDeleted) Then
Begin
  WriteLn('Access granted');
End;
```

A complex condition can also be divided into Boolean variables:

```pascal
Var
  HasNormalAccess : Boolean;
  HasSysopAccess : Boolean;

Begin
  HasNormalAccess := (UserLevel >= 20) And IsActive;
  HasSysopAccess := IsSysop And Not IsDeleted;

  If HasNormalAccess Or HasSysopAccess Then
    WriteLn('Access granted');
End;
```

## Short-Circuit Behavior

Do not assume that `And` and `Or` stop evaluating as soon as the final result is known unless that behavior has been verified with the target compiler.

Potentially unsafe:

```pascal
If (Count > 0) And ((Total / Count) > 10) Then
  WriteLn('Average is greater than ten');
```

If both sides are evaluated, `Count = 0` can still cause division by zero.

Safer:

```pascal
If Count > 0 Then
Begin
  If (Total / Count) > 10 Then
    WriteLn('Average is greater than ten');
End;
```

## Code Blocks

A condition can control one statement without a separate block.

```pascal
If IsReady Then
  WriteLn('Ready');
```

Use `Begin` and `End` when more than one statement belongs to the condition.

```pascal
If IsReady Then
Begin
  WriteLn('Ready');
  Attempts := Attempts + 1;
End;
```

Using explicit blocks is recommended when a branch may grow later.

## `If` Statement

An `If` statement executes code only when its condition is true.

```pascal
If Count = 0 Then
  WriteLn('No records found');
```

General form:

```pascal
If <BooleanExpression> Then
  <Statement>;
```

Block form:

```pascal
If <BooleanExpression> Then
Begin
  <Statement>;
  <Statement>;
End;
```

Example:

```pascal
If UserLevel >= 50 Then
Begin
  WriteLn('High access level');
  CanConfigure := True;
End;
```

## `If` and `Else`

Use `Else` to provide an alternate path when the condition is false.

```pascal
If IsOnline Then
  WriteLn('Status: Online')
Else
  WriteLn('Status: Offline');
```

Block form:

```pascal
If IsOnline Then
Begin
  WriteLn('Status: Online');
  WriteLn('The user is available.');
End
Else
Begin
  WriteLn('Status: Offline');
  WriteLn('The user is unavailable.');
End;
```

The `Else` applies to the nearest unmatched `If`. Use blocks when nested conditions could make that relationship unclear.

## `Else If`

Modern MPL uses two separate keywords:

```pascal
Else If
```

Older source may use the obsolete single keyword:

```text
ElseIf
```

Example:

```pascal
If Count = 0 Then
  WriteLn('No items')
Else If Count = 1 Then
  WriteLn('One item')
Else
  WriteLn('Several items');
```

Conditions are evaluated from top to bottom. The first matching branch executes, and the remaining branches are skipped.

Place more specific conditions before broader conditions.

Incorrect order:

```pascal
If Score >= 50 Then
  Grade := 'Pass'
Else If Score >= 90 Then
  Grade := 'Excellent';
```

A score of 95 matches the first condition, so the second branch is never reached.

Correct order:

```pascal
If Score >= 90 Then
  Grade := 'Excellent'
Else If Score >= 50 Then
  Grade := 'Pass'
Else
  Grade := 'Fail';
```

## Nested Conditions

An `If` statement can appear inside another conditional block.

```pascal
If IsActive Then
Begin
  If UserLevel >= 20 Then
    WriteLn('Access granted')
  Else
    WriteLn('Access level too low');
End
Else
Begin
  WriteLn('Account is inactive');
End;
```

Deep nesting can become difficult to follow.

Instead of:

```pascal
If IsActive Then
Begin
  If Not IsDeleted Then
  Begin
    If UserLevel >= 20 Then
    Begin
      If HasTime Then
        WriteLn('Access granted');
    End;
  End;
End;
```

Consider combining related conditions:

```pascal
If IsActive And
   Not IsDeleted And
   (UserLevel >= 20) And
   HasTime Then
Begin
  WriteLn('Access granted');
End;
```

Or move the decision into a Boolean function:

```pascal
Function CanEnter : Boolean;
Begin
  CanEnter :=
    IsActive And
    Not IsDeleted And
    (UserLevel >= 20) And
    HasTime;
End;
```

## Guard Conditions

A guard condition exits a procedure or function early when processing should not continue.

```pascal
Procedure DisplayUser(Name String);
Begin
  If Name = '' Then
    Exit;

  WriteLn('User: ' + Name);
End;
```

This can be clearer than wrapping the entire procedure in another conditional block.

## Numeric Comparisons

```pascal
If MessageCount > 0 Then
  WriteLn('You have messages');
```

## String Comparisons

```pascal
If UserName = 'Sysop' Then
  WriteLn('Welcome, Sysop');
```

Do not assume that string comparisons are case-insensitive.

These values may compare differently:

```text
Sysop
SYSOP
sysop
```

Normalize input when case should not matter, using a verified MPL string-conversion function.

## Character Comparisons

```pascal
If MenuChoice = 'Q' Then
  WriteLn('Exiting');
```

To accept uppercase and lowercase input:

```pascal
If (MenuChoice = 'Q') Or (MenuChoice = 'q') Then
  WriteLn('Exiting');
```

## `Case` Statement

A `Case` statement selects one branch based on a single expression.

```pascal
Case MenuChoice Of
  'A':
    WriteLn('Add selected');

  'E':
    WriteLn('Edit selected');

  'Q':
    WriteLn('Quit selected');
Else
  WriteLn('Unknown selection');
End;
```

Use `Case` when several branches compare the same value against exact alternatives.

## `Case` with Blocks

Use `Begin` and `End` when a branch contains multiple statements.

```pascal
Case MenuChoice Of
  'A':
  Begin
    WriteLn('Add selected');
    AddRecord;
    WriteLn('Record added');
  End;

  'E':
  Begin
    WriteLn('Edit selected');
    EditRecord;
    WriteLn('Record updated');
  End;
Else
  WriteLn('Unknown selection');
End;
```

## Multiple Values in One Branch

Several values can share one branch.

```pascal
Case MenuChoice Of
  'Y', 'y':
    WriteLn('Confirmed');

  'N', 'n':
    WriteLn('Cancelled');
Else
  WriteLn('Please enter Y or N');
End;
```

## Numeric `Case`

```pascal
Case MenuNumber Of
  1:
    WriteLn('First option');

  2, 3, 4:
    WriteLn('Middle option');

  5:
    WriteLn('Last option');
Else
  WriteLn('Invalid option');
End;
```

## String `Case`

Modern MPL expands `Case` handling to additional value types, including strings.

```pascal
Case Command Of
  'LIST':
    WriteLn('Listing records');

  'ADD':
    WriteLn('Adding a record');

  'DELETE':
    WriteLn('Deleting a record');
Else
  WriteLn('Unknown command');
End;
```

String `Case` behavior, including case sensitivity, should be verified with the exact MPLC version used by the BBS.

## Choosing `Case` or `If`

Use `Case` when:

- One value is compared against several exact alternatives
- Several values share the same action
- A menu or command selector is being processed

```pascal
Case Choice Of
  'A': AddRecord;
  'D': DeleteRecord;
  'Q': Exit;
End;
```

Use `If` when:

- Conditions use ranges
- Several variables are involved
- Conditions use `And`, `Or`, or `Not`
- Each branch tests a different expression

```pascal
If (Age >= 18) And HasPermission Then
  AllowAccess;
```

## Conditions with Bit Flags

Modern MPL supports bitwise operators.

```pascal
Const
  UserDeleted = $00000004;

If (UserFlags And UserDeleted) <> 0 Then
  WriteLn('User is marked deleted');
```

Mystic also provides functions such as `BitCheck`:

```pascal
If BitCheck(3, UserFlags) Then
  WriteLn('User is marked deleted');
```

Use the form that matches the documented flag numbering for the target Mystic release.

## Conditions with Function Results

Boolean-returning functions can be used directly:

```pascal
If FileExist(FileName) Then
  WriteLn('File exists');
```

A function used in a condition may also have side effects, such as:

- Reading a record
- Consuming keyboard input
- Advancing an index
- Changing Mystic runtime variables
- Modifying `Var` parameters

Confirm those effects before using the function in a complex condition.

## Complete Example

```pascal
Var
  UserLevel : Integer = 20;
  IsActive : Boolean = True;
  IsDeleted : Boolean = False;
  MenuChoice : Char = 'S';
  AccessName : String;

Begin
  If Not IsActive Then
  Begin
    WriteLn('Account is inactive');
    Halt;
  End;

  If IsDeleted Then
  Begin
    WriteLn('Account is deleted');
    Halt;
  End;

  If UserLevel >= 100 Then
    AccessName := 'Sysop'
  Else If UserLevel >= 50 Then
    AccessName := 'Co-Sysop'
  Else If UserLevel >= 20 Then
    AccessName := 'Validated'
  Else
    AccessName := 'New User';

  WriteLn('Access: ' + AccessName);

  Case MenuChoice Of
    'A', 'a':
      WriteLn('Add selected');

    'S', 's':
      WriteLn('Status selected');

    'Q', 'q':
      WriteLn('Quit selected');
  Else
    WriteLn('Unknown menu choice');
  End;
End;
```

This example demonstrates:

- Boolean conditions
- `Not`
- Guard-style termination
- `Else If`
- Ordered range checks
- Character `Case`
- Multiple values in one `Case` branch
- A default `Else` branch

## Common Errors

### Using Assignment in a Condition

Incorrect:

```pascal
If UserLevel := 10 Then
```

Correct:

```pascal
If UserLevel = 10 Then
```

### Using C-Style Comparison Operators

Incorrect:

```pascal
If UserLevel == 10 Then
If UserLevel != 10 Then
```

Correct:

```pascal
If UserLevel = 10 Then
If UserLevel <> 10 Then
```

### Forgetting `Then`

Incorrect:

```pascal
If UserLevel = 10
  WriteLn('Level ten');
```

Correct:

```pascal
If UserLevel = 10 Then
  WriteLn('Level ten');
```

### Using the Obsolete `ElseIf` Keyword

Older form:

```text
ElseIf UserLevel >= 20
```

Modern form:

```pascal
Else If UserLevel >= 20 Then
```

### Omitting a Block for Multiple Statements

Incorrect:

```pascal
If IsActive Then
  WriteLn('Active');
  WriteLn('Welcome');
```

Only the first statement is controlled by the condition.

Correct:

```pascal
If IsActive Then
Begin
  WriteLn('Active');
  WriteLn('Welcome');
End;
```

### Reversing `And` and `Or`

Incorrect for uppercase or lowercase input:

```pascal
If (Choice = 'Y') And (Choice = 'y') Then
```

Correct:

```pascal
If (Choice = 'Y') Or (Choice = 'y') Then
```

### Placing a Broad Condition First

Incorrect:

```pascal
If Score >= 50 Then
  Grade := 'Pass'
Else If Score >= 90 Then
  Grade := 'Excellent';
```

Correct:

```pascal
If Score >= 90 Then
  Grade := 'Excellent'
Else If Score >= 50 Then
  Grade := 'Pass';
```

### Ambiguous `Else` Placement

Nested conditions without blocks can make it unclear which `If` owns the `Else`. Use explicit blocks.

### Omitting the `Case` Default

A missing `Else` branch may be valid, but unexpected values are silently ignored. User-input selectors should normally include a default branch.

## Verification Checklist

Before marking this page verified, test and record:

- `If ... Then`
- `Else`
- `Else If`
- Nested-condition behavior
- `Begin` and `End` requirements
- Numeric comparisons
- String comparisons
- Character comparisons
- Boolean values
- `And`, `Or`, and `Not`
- Logical operator precedence
- Short-circuit behavior
- Numeric `Case`
- Character `Case`
- Multiple values in one branch
- String `Case`
- `Else` inside `Case`
- Bitwise condition behavior

Suggested verification record:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
If/Else confirmed:
Logical operators confirmed:
Case syntax confirmed:
String Case confirmed:
Short-circuit behavior confirmed:
Notes:
```

## Documentation Status

This page currently covers:

- Boolean values and expressions
- Comparison operators
- Logical operators
- Parentheses and evaluation order
- Short-circuit safety
- `If`, `Else`, and `Else If`
- Nested and guard conditions
- Numeric, character, and string comparisons
- Numeric, character, and string `Case`
- Multiple `Case` values
- Bit flags
- Function results in conditions
- Common mistakes

Additional compiler testing is still needed for:

- Exact logical operator precedence
- Short-circuit behavior
- String comparison case sensitivity
- String `Case` case sensitivity
- Range syntax inside `Case`
- Empty `Case` branches
- Duplicate `Case` values
- Interaction with alternate C-like MPL syntax
- Differences between 32-bit and 64-bit Mystic builds

Examples should be tested with the exact Mystic and MPLC version used by the target BBS.

## Related Pages

- [Operators](Operators)
- [Data Types](Data-Types)
- [Variables](Variables)
- [Loops](Loops)
- [Procedures](Procedures)
- [Functions](Functions)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- [Mystic BBS Wiki: Mystic Programming Language](https://wiki.mysticbbs.com/doku.php?id=mpl)
- [Mystic BBS Wiki: Mystic 1.10 Changes](https://wiki.mysticbbs.com/doku.php?id=whats_new_110)
- [Mystic BBS Source and Release Notes](https://github.com/fidosoft/mysticbbs)

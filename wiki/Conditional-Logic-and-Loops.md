# MPL Conditional Logic and Loops

Conditional statements allow an MPL program to choose which code to execute. Loops allow a program to repeat code while a condition remains true, until a condition becomes true, or for a defined range of values.

This page covers:

- Boolean expressions
- Comparison and logical operators
- `If`, `Else If`, and `Else`
- `Case` statements
- `For`, `While`, and `Repeat` loops
- `Break` and `Continue`
- Nested conditions and loops
- Common control-flow mistakes

> [!NOTE]
> Modern MPL follows Pascal-style control-flow syntax. Older source may use obsolete keywords such as `EndIf`, `FEnd`, `WEnd`, or the single keyword `ElseIf`. These forms were replaced when MPL syntax was standardized for Mystic 1.10.

## Related Pages

- [[MPL Syntax]]
- [[MPL Variables]]
- [[MPL Procedures]]
- [[MPL Functions]]

## Boolean Expressions

A Boolean expression evaluates to either `True` or `False`.

```pascal
UserLevel >= 20
Attempts = 0
UserName <> ''
IsActive
Not IsDeleted
```

Conditional statements and loops use Boolean expressions to decide whether their controlled code should run.

```pascal
If UserLevel >= 20 Then
  WriteLn('Access granted');
```

## Comparison Operators

| Operator | Meaning |
|---|---|
| `=` | Equal |
| `<>` | Not equal |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal |
| `>=` | Greater than or equal |

The assignment operator is different:

```pascal
Count := 10;
```

Do not use `=` when assigning a value.

## Logical Operators

| Operator | Meaning |
|---|---|
| `And` | Both expressions must be true |
| `Or` | At least one expression must be true |
| `Not` | Reverses a Boolean result |

```pascal
If IsActive And HasAccess Then
  WriteLn('The user may continue');

If Choice = 'Y' Or Choice = 'y' Then
  WriteLn('Yes selected');

If Not FileExist(FileName) Then
  WriteLn('File not found');
```

Use parentheses to make compound conditions explicit:

```pascal
If (UserLevel >= 20) And (IsActive Or IsSysop) Then
  WriteLn('Access granted');
```

## Code Blocks

A conditional statement or loop can control one statement without a `Begin` and `End` block.

```pascal
If IsReady Then
  WriteLn('Ready');
```

Use `Begin` and `End` when more than one statement is controlled.

```pascal
If IsReady Then
Begin
  WriteLn('Ready');
  Attempts := Attempts + 1;
End;
```

## `If` Statement

An `If` statement executes code only when its condition is true.

```pascal
If Count = 0 Then
  WriteLn('No records found');
```

Block form:

```pascal
If UserLevel >= 50 Then
Begin
  WriteLn('High access level');
  CanConfigure := True;
End;
```

## `If` and `Else`

Use `Else` to provide an alternative when the condition is false.

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

The `Else` applies to the nearest unmatched `If`.

## `Else If`

Modern MPL uses two separate keywords:

```pascal
Else If
```

It does not use the older single keyword `ElseIf`.

```pascal
If Count = 0 Then
  WriteLn('No items')
Else If Count = 1 Then
  WriteLn('One item')
Else
  WriteLn('Several items');
```

Conditions are evaluated from top to bottom. Place more specific conditions before broader conditions.

```pascal
If Score >= 90 Then
  Grade := 'Excellent'
Else If Score >= 50 Then
  Grade := 'Pass'
Else
  Grade := 'Fail';
```

## Nested Conditions

An `If` statement can appear inside another `If` statement.

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

Avoid excessive nesting. Combine related conditions or move them into a Boolean function when that improves readability.

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

## `Case` Statement

A `Case` statement compares one expression against several possible values.

```pascal
Case Choice Of
  'A':
    WriteLn('Add selected');

  'D':
    WriteLn('Delete selected');

  'Q':
    WriteLn('Quit selected');
Else
  WriteLn('Unknown choice');
End;
```

Several values can share one branch:

```pascal
Case Choice Of
  'Y', 'y':
    WriteLn('Yes');

  'N', 'n':
    WriteLn('No');
Else
  WriteLn('Please enter Y or N');
End;
```

Use `Begin` and `End` when a branch contains several statements.

```pascal
Case Choice Of
  'A':
  Begin
    WriteLn('Adding record');
    AddRecord;
    WriteLn('Record added');
  End;

  'D':
  Begin
    WriteLn('Deleting record');
    DeleteRecord;
    WriteLn('Record deleted');
  End;
Else
  WriteLn('Unknown choice');
End;
```

Modern MPL expands Pascal-style `Case` handling to additional types, including strings. String `Case` behavior, including case sensitivity, should be tested with the exact MPLC version used by the BBS.

## Choosing `Case` or `If`

Use `Case` when one value is compared against several exact alternatives.

```pascal
Case Choice Of
  'A': AddRecord;
  'D': DeleteRecord;
  'Q': Exit;
End;
```

Use `If` when conditions involve ranges, several variables, or logical operators.

```pascal
If (Age >= 18) And HasPermission Then
  AllowAccess;
```

## Loops

MPL provides three primary loop forms:

| Loop | Best use |
|---|---|
| `For` | Repeat across a known numeric range |
| `While` | Repeat while a condition is true |
| `Repeat` / `Until` | Run at least once, then stop when a condition is true |

Modern MPL also supports `Break` and `Continue` in `For`, `While`, and `Repeat` loops.

## `For` Loop

A `For` loop repeats code through a defined range.

```pascal
Var Counter : Byte;

Begin
  For Counter := 1 To 5 Do
    WriteLn(Int2Str(Counter));
End;
```

The ending value is included.

Count downward with `DownTo`:

```pascal
For Counter := 5 DownTo 1 Do
  WriteLn(Int2Str(Counter));
```

Use a block for multiple statements:

```pascal
For Counter := 1 To 5 Do
Begin
  Write('Item ');
  WriteLn(Int2Str(Counter));
End;
```

### Processing an Array

```pascal
Var
  Scores : Array[1..5] of Integer;
  Index : Byte;

Begin
  For Index := 1 To 5 Do
    WriteLn('Score: ' + Int2Str(Scores[Index]));
End;
```

The loop range must stay within the array's declared bounds.

Avoid changing a `For` loop's control variable inside the loop body. Let the loop manage it.

## `While` Loop

A `While` loop checks its condition before every iteration.

```pascal
Var Counter : Byte = 1;

Begin
  While Counter <= 5 Do
  Begin
    WriteLn(Int2Str(Counter));
    Counter := Counter + 1;
  End;
End;
```

A `While` loop may execute zero times when its condition is initially false.

The loop must normally change something that affects its condition. Otherwise it may never end.

Incorrect:

```pascal
While Counter <= 5 Do
  WriteLn(Int2Str(Counter));
```

Correct:

```pascal
While Counter <= 5 Do
Begin
  WriteLn(Int2Str(Counter));
  Counter := Counter + 1;
End;
```

## `Repeat` and `Until`

A `Repeat` loop checks its condition after the loop body.

```pascal
Var Counter : Byte = 1;

Begin
  Repeat
    WriteLn(Int2Str(Counter));
    Counter := Counter + 1;
  Until Counter > 5;
End;
```

A `Repeat` loop always executes at least once.

The condition after `Until` describes when the loop should stop.

```pascal
Repeat
  Choice := ReadKey;
Until Choice = 'Q';
```

## `Break`

`Break` exits the nearest active loop immediately.

```pascal
For Counter := 1 To 10 Do
Begin
  If Counter = 5 Then
    Break;

  WriteLn(Int2Str(Counter));
End;
```

Use `Break` when a loop has found its result or cannot continue safely.

## `Continue`

`Continue` skips the remainder of the current iteration and starts the next iteration.

```pascal
For Counter := 1 To 10 Do
Begin
  If Counter % 2 = 0 Then
    Continue;

  WriteLn(Int2Str(Counter));
End;
```

This prints only odd values.

> [!NOTE]
> The exact behavior of `Continue` in a `Repeat` loop should be tested with the target MPLC release.

## Nested Loops

A loop can contain another loop.

```pascal
Var
  Row : Byte;
  Column : Byte;

Begin
  For Row := 1 To 3 Do
  Begin
    For Column := 1 To 5 Do
      Write('*');

    WriteLn('');
  End;
End;
```

In nested loops, `Break` and `Continue` affect only the nearest active loop.

## Choosing the Correct Loop

| Requirement | Recommended loop |
|---|---|
| Count from one value to another | `For` |
| Process every array element | `For` |
| Continue while a condition is true | `While` |
| Processing may run zero times | `While` |
| Prompt at least once | `Repeat` |
| Stop when a condition becomes true | `Repeat` / `Until` |

## Infinite Loops

An infinite loop never reaches a terminating condition.

Intentional loop with an exit:

```pascal
While True Do
Begin
  Choice := ReadKey;

  If Choice = 'Q' Then
    Break;

  ProcessChoice(Choice);
End;
```

When debugging a loop, verify:

1. The initial condition
2. The condition checked each iteration
3. Every variable used by the condition
4. Where those variables change
5. Whether `Continue` skips the update
6. Whether an early branch prevents termination

## Off-by-One Errors

For an array declared as:

```pascal
Var Values : Array[1..10] of Integer;
```

Correct:

```pascal
For Index := 1 To 10 Do
  Values[Index] := 0;
```

Incorrect:

```pascal
For Index := 0 To 10 Do
  Values[Index] := 0;
```

Index `0` is outside the declared range.

## Complete Example

```pascal
Var
  Choice : Char;
  Counter : Byte;
  Total : LongInt = 0;
  IsRunning : Boolean = True;

Procedure DisplayMenu;
Begin
  WriteLn('');
  WriteLn('[1] Count upward');
  WriteLn('[2] Count downward');
  WriteLn('[3] Sum odd values');
  WriteLn('[Q] Quit');
  Write('Choice: ');
End;

Procedure CountUp;
Begin
  For Counter := 1 To 5 Do
    WriteLn(Int2Str(Counter));
End;

Procedure CountDown;
Begin
  For Counter := 5 DownTo 1 Do
    WriteLn(Int2Str(Counter));
End;

Procedure SumOddValues;
Begin
  Total := 0;

  For Counter := 1 To 10 Do
  Begin
    If Counter % 2 = 0 Then
      Continue;

    Total := Total + Counter;
  End;

  WriteLn('Odd-value total: ' + Int2Str(Total));
End;

Begin
  While IsRunning Do
  Begin
    DisplayMenu;
    Choice := ReadKey;
    WriteLn('');

    Case Choice Of
      '1': CountUp;
      '2': CountDown;
      '3': SumOddValues;
      'Q', 'q': IsRunning := False;
    Else
      WriteLn('Invalid choice');
    End;
  End;

  WriteLn('Goodbye');
End;
```

## Common Conditional Mistakes

### Using `:=` in a comparison

Incorrect:

```pascal
If Count := 10 Then
```

Correct:

```pascal
If Count = 10 Then
```

### Forgetting `Then`

```pascal
If Count = 10 Then
  WriteLn('Ten');
```

### Using the obsolete `ElseIf` keyword

Use:

```pascal
Else If Count = 2 Then
```

### Forgetting a block

```pascal
If IsReady Then
Begin
  WriteLn('Ready');
  Count := Count + 1;
End;
```

### Reversing `And` and `Or`

A choice cannot be uppercase and lowercase simultaneously.

Correct:

```pascal
If (Choice = 'Y') Or (Choice = 'y') Then
```

## Common Loop Mistakes

### Forgetting `Do`

```pascal
While Count < 10 Do
  Count := Count + 1;
```

The same applies to `For`.

### Using obsolete loop terminators

Older MPL may use `FEnd` or `WEnd`. Modern MPL uses `End` for blocked loops.

### Reversing an `Until` condition

To stop when `Q` is pressed:

```pascal
Repeat
  Choice := ReadKey;
Until Choice = 'Q';
```

### Changing the `For` control variable

Let the `For` loop update its control variable.

### Breaking only the inner loop

In nested loops, `Break` exits only the nearest loop.

## Testing Recommendations

Create a small compiler test covering:

```text
If with one statement
If with a Begin/End block
If and Else
Else If chain
Nested If
Numeric Case
Character Case
String Case
Case with several matching values
Case Else branch
For with To
For with DownTo
While
Repeat/Until
Break in For
Break in While
Break in Repeat
Continue in For
Continue in While
Continue in Repeat
Nested loops
Break inside nested loops
Boolean operators with parentheses
Array bounds
```

Record the environment:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Compile result:
Runtime result:
```

## Documentation Status

This page currently covers:

- Boolean expressions
- Comparison and logical operators
- Parentheses
- Code blocks
- `If`, `Else`, and `Else If`
- Nested conditions
- Guard conditions
- Numeric, character, and string `Case`
- `For`, `To`, and `DownTo`
- `While`
- `Repeat` and `Until`
- `Break` and `Continue`
- Nested loops
- Infinite loops
- Off-by-one errors
- Common compiler and logic mistakes

Additional compiler testing is still needed for:

- Exact Boolean operator precedence
- Short-circuit behavior of `And` and `Or`
- String `Case` case sensitivity
- Range syntax inside `Case`
- Empty `Case` branches
- `Break` behavior in every loop type
- `Continue` behavior in `Repeat`
- Final value of a `For` control variable
- Changing a `For` control variable
- Loop variable type restrictions
- Interaction between `Break`, `Continue`, and nested blocks
- C-like alternate syntax
- Differences between 32-bit and 64-bit Mystic builds

Examples should be tested with the exact Mystic and MPLC version used by the target BBS.

## References

- [Mystic BBS Wiki: Mystic Programming Language](https://wiki.mysticbbs.com/doku.php?id=mpl)
- [Mystic BBS Wiki: Mystic 1.10 Changes](https://wiki.mysticbbs.com/doku.php?id=whats_new_110)
- [Mystic BBS Source and Release Notes](https://github.com/fidosoft/mysticbbs)

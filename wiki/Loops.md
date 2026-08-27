# Loops

Loops repeat one or more statements until a condition changes or a defined range has been completed.

This page covers:

- `For` loops
- `To` and `DownTo`
- `While` loops
- `Repeat ... Until`
- `Break` and `Continue`
- Nested loops
- Array-processing loops
- Infinite-loop prevention
- Common boundary and control-flow mistakes

> **Documentation status**
>
> This page documents modern Pascal-style MPL loop syntax used by Mystic 1.10 and later. Compiler-sensitive details, including loop-variable types, final counter values, and some `Break` or `Continue` edge cases, should still be tested with the exact Mystic and MPLC release in use.

## Loop Types

MPL provides three primary loop forms:

| Loop | Best used when |
|---|---|
| `For` | The number of iterations is known |
| `While` | The condition should be checked before each iteration |
| `Repeat ... Until` | The loop body must run at least once |

Modern MPL also supports `Break` and `Continue` in `For`, `While`, and `Repeat` loops.

## Code Blocks in Loops

A loop can control one statement without a separate block.

```pascal
For Index := 1 To 5 Do
  WriteLn('Processing');
```

Use `Begin` and `End` when more than one statement belongs to the loop.

```pascal
For Index := 1 To 5 Do
Begin
  Write('Item ');
  WriteLn(Int2Str(Index));
End;
```

Using explicit blocks is recommended when a loop body may grow later.

## `For` Loops

A `For` loop repeats code while changing a control variable through a defined range.

General upward form:

```pascal
For <Variable> := <StartValue> To <EndValue> Do
  <Statement>;
```

General downward form:

```pascal
For <Variable> := <StartValue> DownTo <EndValue> Do
  <Statement>;
```

## Counting Up with `To`

```pascal
Var Index : Integer;

Begin
  For Index := 1 To 5 Do
    WriteLn(Int2Str(Index));
End;
```

Output:

```text
1
2
3
4
5
```

The ending value is included.

## Counting Down with `DownTo`

```pascal
Var Index : Integer;

Begin
  For Index := 5 DownTo 1 Do
    WriteLn(Int2Str(Index));
End;
```

Output:

```text
5
4
3
2
1
```

Older MPL references may describe `DownTo` as version-sensitive. Modern MPL uses Pascal-style descending loops, but the exact target compiler should still be tested.

## Displaying the Counter

Convert numeric loop counters to text when building strings.

```pascal
For Index := 1 To 5 Do
  WriteLn('Loop: ' + Int2Str(Index));
```

Do not assume that implicit number-to-string conversion is supported:

```pascal
WriteLn('Loop: ' + Index);
```

## Processing an Array

```pascal
Var
  Scores : Array[1..5] of Integer;
  Index : Byte;

Begin
  Scores[1] := 90;
  Scores[2] := 85;
  Scores[3] := 100;
  Scores[4] := 75;
  Scores[5] := 95;

  For Index := 1 To 5 Do
    WriteLn('Score: ' + Int2Str(Scores[Index]));
End;
```

The loop range must stay within the array's declared bounds.

## Non-One-Based Ranges

Array indexes do not have to begin at `1`.

```pascal
Var
  Values : Array[5..10] of Integer;
  Index : Byte;

Begin
  For Index := 5 To 10 Do
    Values[Index] := Index * 2;
End;
```

The loop range should match the declared array bounds.

## The `For` Control Variable

The control variable should be a compatible numeric variable.

```pascal
Var Index : Integer;
```

Avoid changing it inside the loop body.

Risky:

```pascal
For Index := 1 To 10 Do
Begin
  Index := Index + 2;
  WriteLn(Int2Str(Index));
End;
```

Let the loop manage its own control variable. Use `Break`, `Continue`, or another variable when different behavior is required.

The final value of the control variable after the loop may be compiler-dependent. Do not rely on it unless tested.

## `While` Loops

A `While` loop checks its condition before each iteration.

```pascal
Var Count : Integer = 1;

Begin
  While Count <= 5 Do
  Begin
    WriteLn(Int2Str(Count));
    Count := Count + 1;
  End;
End;
```

If the condition is false before the first check, the loop body does not run.

```pascal
Var Count : Integer = 10;

Begin
  While Count < 5 Do
    WriteLn('This does not run');
End;
```

## Updating the `While` Condition

A `While` loop must normally change something that affects its condition.

Incorrect:

```pascal
Count := 1;

While Count <= 5 Do
  WriteLn(Int2Str(Count));
```

`Count` never changes, so the loop does not end.

Correct:

```pascal
Count := 1;

While Count <= 5 Do
Begin
  WriteLn(Int2Str(Count));
  Count := Count + 1;
End;
```

## Boolean-Controlled `While` Loop

```pascal
Var KeepRunning : Boolean = True;

Begin
  While KeepRunning Do
  Begin
    ProcessNextItem;

    If ShouldStop Then
      KeepRunning := False;
  End;
End;
```

This pattern is useful when no fixed iteration count exists.

## Sentinel-Controlled `While` Loop

A sentinel is a special value that ends processing.

```pascal
Var Choice : Char;

Begin
  Choice := ReadKey;

  While Choice <> 'Q' Do
  Begin
    WriteLn('You selected: ' + Choice);
    Choice := ReadKey;
  End;
End;
```

The input must be updated inside the loop.

## Function-Controlled `While` Loop

A Boolean-returning function can control a loop.

```pascal
While FileExist(FileName) Do
Begin
  ProcessFile(FileName);
  FileName := GetNextFileName;
End;
```

Some Mystic built-ins both return a Boolean result and change loaded runtime data.

Example pattern:

```pascal
Var BaseNumber : LongInt = 1;

Begin
  While GetMBase(BaseNumber) Do
  Begin
    WriteLn(MBaseName);
    BaseNumber := BaseNumber + 1;
  End;
End;
```

Confirm each function's required `Uses` section and side effects before using it as a loop condition.

## `Repeat ... Until`

A `Repeat` loop checks its condition after the body executes.

```pascal
Var Count : Integer = 1;

Begin
  Repeat
    WriteLn(Int2Str(Count));
    Count := Count + 1;
  Until Count > 5;
End;
```

Because the condition is checked last, the body runs at least once.

## `Until` Means Stop When True

The condition after `Until` describes when the loop should stop.

```pascal
Repeat
  Choice := ReadKey;
Until Choice = 'Q';
```

The loop continues while `Choice <> 'Q'` and stops when `Choice = 'Q'`.

Equivalent forms:

```pascal
While Choice <> 'Q' Do
Begin
  Choice := ReadKey;
End;
```

```pascal
Repeat
  Choice := ReadKey;
Until Choice = 'Q';
```

The `Repeat` form reads at least one key. The `While` form depends on the value of `Choice` before the loop starts.

## Input Validation with `Repeat`

`Repeat` is useful when the program must ask at least once.

```pascal
Var Choice : Char;

Begin
  Repeat
    Write('Select Y or N: ');
    Choice := ReadKey;
    WriteLn('');
  Until (Choice = 'Y') Or
        (Choice = 'y') Or
        (Choice = 'N') Or
        (Choice = 'n');
End;
```

## `While` Compared with `Repeat`

Use `While` when the condition must be true before the body runs:

```pascal
While IsConnected Do
Begin
  ProcessConnection;
End;
```

Use `Repeat ... Until` when the body must run at least once:

```pascal
Repeat
  DisplayMenu;
  Choice := ReadKey;
Until Choice = 'Q';
```

## `Break`

`Break` exits the nearest active loop immediately.

```pascal
Var Index : Byte;

Begin
  For Index := 1 To 10 Do
  Begin
    If Index = 5 Then
      Break;

    WriteLn(Int2Str(Index));
  End;
End;
```

Output:

```text
1
2
3
4
```

Use `Break` when a loop has found its result or cannot continue safely.

```pascal
Var
  Index : Byte;
  Found : Boolean = False;

Begin
  For Index := 1 To 10 Do
  Begin
    If Values[Index] = SearchValue Then
    Begin
      Found := True;
      Break;
    End;
  End;
End;
```

## `Continue`

`Continue` skips the remainder of the current iteration and starts the next iteration.

```pascal
Var Index : Byte;

Begin
  For Index := 1 To 10 Do
  Begin
    If Index % 2 = 0 Then
      Continue;

    WriteLn(Int2Str(Index));
  End;
End;
```

This prints only odd values.

Another example:

```pascal
For Index := 1 To TotalUsers Do
Begin
  If Not GetUser(Index) Then
    Continue;

  If UserDeleted Then
    Continue;

  DisplayUser;
End;
```

The exact behavior of `Continue` in a `Repeat` loop should be tested with the target MPLC release. It should transfer control toward the `Until` condition.

## `Break` and `Continue` Scope

In nested loops, `Break` and `Continue` affect only the nearest active loop.

```pascal
For Row := 1 To 10 Do
Begin
  For Column := 1 To 10 Do
  Begin
    If Grid[Column, Row] = 'X' Then
      Break;
  End;
End;
```

The `Break` exits the `Column` loop. The `Row` loop continues.

To exit several nested loops, consider:

- Moving the search into a function and using `Exit`
- Using a Boolean flag checked by the outer loop
- Reducing the nesting

Function approach:

```pascal
Function FindMarker : Boolean;
Var
  Row : Byte;
  Column : Byte;
Begin
  FindMarker := False;

  For Row := 1 To 10 Do
  Begin
    For Column := 1 To 10 Do
    Begin
      If Grid[Column, Row] = 'X' Then
      Begin
        FindMarker := True;
        Exit;
      End;
    End;
  End;
End;
```

## Nested Loops

A loop can appear inside another loop.

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

Output:

```text
*****
*****
*****
```

Nested loops multiply the amount of work performed. Keep them small when they perform file access, Mystic record access, terminal output, or expensive string processing.

## Conditions Inside Loops

Conditions are commonly used to select which loop items to process.

```pascal
For Index := 1 To 10 Do
Begin
  If Scores[Index] >= 90 Then
    WriteLn('High score: ' + Int2Str(Scores[Index]));
End;
```

Use `Else If` when every item needs one of several actions:

```pascal
For Index := 1 To 10 Do
Begin
  If Scores[Index] >= 90 Then
    WriteLn('Excellent')
  Else If Scores[Index] >= 70 Then
    WriteLn('Passing')
  Else
    WriteLn('Needs improvement');
End;
```

## Loops Inside Conditions

A condition can choose whether a loop should run.

```pascal
If TotalItems > 0 Then
Begin
  For Index := 1 To TotalItems Do
    DisplayItem(Index);
End
Else
Begin
  WriteLn('No items found');
End;
```

## Choosing the Correct Loop

| Requirement | Recommended loop |
|---|---|
| Count from one value to another | `For` |
| Process every array element | `For` |
| Continue while a condition is true | `While` |
| Processing may run zero times | `While` |
| Prompt at least once | `Repeat ... Until` |
| Stop when a condition becomes true | `Repeat ... Until` |
| Search until a match is found | Any loop with `Break` or a Boolean function |

## Infinite Loops

An infinite loop never reaches a terminating condition.

Intentional infinite loop with an exit path:

```pascal
While True Do
Begin
  Choice := ReadKey;

  If Choice = 'Q' Then
    Break;

  ProcessChoice(Choice);
End;
```

Accidental infinite loop:

```pascal
Var Count : Byte = 1;

Begin
  While Count <= 10 Do
    WriteLn(Int2Str(Count));
End;
```

`Count` is never updated.

When debugging a loop, verify:

1. The initial condition
2. The condition checked each iteration
3. Every variable used by the condition
4. Where those variables change
5. Whether `Continue` skips the update
6. Whether an early branch prevents termination

## `Continue` Causing an Infinite Loop

A `Continue` can skip the statement that updates a `While` condition.

Incorrect:

```pascal
Var Count : Byte = 0;

Begin
  While Count < 10 Do
  Begin
    If Count = 5 Then
      Continue;

    Count := Count + 1;
  End;
End;
```

When `Count` reaches 5, `Continue` skips the increment forever.

Correct:

```pascal
While Count < 10 Do
Begin
  Count := Count + 1;

  If Count = 5 Then
    Continue;

  WriteLn(Int2Str(Count));
End;
```

## Off-by-One Errors

An off-by-one error processes one item too many or one item too few.

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

A Pascal-style `For` ending value is included:

```pascal
For Index := 1 To 10 Do
```

This runs ten iterations.

## File-Processing Loops

Loops are commonly used to process records or lines until an end condition is reached.

Conceptual form:

```pascal
While Not EndOfFile Do
Begin
  ReadNextRecord;
  ProcessCurrentRecord;
End;
```

Actual file APIs and end-of-file behavior are Mystic-specific and should be documented on [File Handling](File-Handling).

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

This example demonstrates:

- A Boolean-controlled `While` loop
- `For ... To`
- `For ... DownTo`
- `Continue`
- A loop calling procedures
- A `Case` statement inside a loop
- Controlled loop termination

## Common Errors

### Forgetting `Do`

Incorrect:

```pascal
While Count < 10
  Count := Count + 1;
```

Correct:

```pascal
While Count < 10 Do
  Count := Count + 1;
```

The same applies to `For`.

### Using Obsolete Loop Terminators

Older MPL source may use:

```text
FEnd
WEnd
```

Modern blocked loops use:

```pascal
End;
```

### Forgetting to Update a `While` Condition

```pascal
While Count < 10 Do
Begin
  WriteLn('Running');
End;
```

This may never terminate.

### Reversing an `Until` Condition

`Repeat ... Until` stops when the condition becomes true.

To stop when `Q` is pressed:

```pascal
Repeat
  Choice := ReadKey;
Until Choice = 'Q';
```

### Changing the `For` Control Variable

Avoid:

```pascal
For Index := 1 To 10 Do
  Index := Index + 1;
```

Let the loop update its own control variable.

### Using `Break` Outside a Loop

`Break` is intended for `For`, `While`, and `Repeat` loops. Use `Exit` to leave a procedure or function.

### Using `Continue` Outside a Loop

`Continue` skips the remainder of the current loop iteration. It does not skip arbitrary procedure statements.

### Breaking Only the Inner Loop

In nested loops, `Break` exits only the nearest loop.

### Using the Wrong Loop Type

A `While` loop may run zero times. A `Repeat ... Until` loop runs at least once. Choose the form that matches the required behavior.

## Verification Checklist

Before marking this page verified, test and record:

- `For ... To ... Do`
- Inclusive ending values
- `DownTo`
- Valid loop-variable types
- Behavior when the start exceeds the end
- Whether the ending expression is evaluated once
- `While ... Do`
- `Repeat ... Until`
- Nested-loop behavior
- `Break` in `For`
- `Break` in `While`
- `Break` in `Repeat`
- `Continue` in `For`
- `Continue` in `While`
- `Continue` in `Repeat`
- Final `For` counter value
- Whether modifying a `For` variable is rejected
- Numeric overflow behavior

Suggested verification record:

```text
Mystic version:
MPLC version:
Operating system:
Architecture:
Date tested:
For loop confirmed:
DownTo confirmed:
While loop confirmed:
Repeat Until confirmed:
Break behavior confirmed:
Continue behavior confirmed:
Boundary behavior confirmed:
Notes:
```

## Documentation Status

This page currently covers:

- `For`, `To`, and `DownTo`
- Counter conversion
- Array processing
- `While`
- Boolean and sentinel controls
- Function-controlled loops
- `Repeat ... Until`
- `Break` and `Continue`
- Nested loops
- Conditions inside loops
- Infinite-loop prevention
- Off-by-one errors
- File-processing patterns
- Common mistakes

Additional compiler testing is still needed for:

- Valid `For` loop-variable types
- Start values greater than ending values
- Evaluation timing of the ending expression
- Final value of the `For` counter
- `Break` behavior in every loop type
- `Continue` behavior in `Repeat`
- Modifying a `For` control variable
- Numeric overflow at loop boundaries
- Interaction with alternate C-like MPL syntax
- Differences between 32-bit and 64-bit Mystic builds

Examples should be tested with the exact Mystic and MPLC version used by the target BBS.

## Related Pages

- [Conditional Logic](Conditional-Logic)
- [Operators](Operators)
- [Variables](Variables)
- [Data Types](Data-Types)
- [Procedures](Procedures)
- [Functions](Functions)
- [File Handling](File-Handling)
- [Version Compatibility](Version-Compatibility)
- [Documentation Status](Documentation-Status)

## References

- [Mystic BBS Source and Release Notes](https://github.com/fidosoft/mysticbbs)

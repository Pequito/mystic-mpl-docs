# Loops

Loops repeat one or more statements until a condition changes or a defined range has been completed.

MPL uses Pascal-style loop syntax. The exact support for `For`, `While`, `Repeat`, loop-control statements, and boundary behavior should be verified against the Mystic BBS release being documented.

> **Documentation status**
>
> This page is an initial reference. Loop syntax, range behavior, and exit semantics should be tested with the active MPL compiler before the page is marked verified.

## Loop Types

Common loop forms include:

| Loop | Best used when |
|---|---|
| `For` | The number of iterations is known |
| `While` | The condition should be checked before each iteration |
| `Repeat ... Until` | The loop body must run at least once |

## For Loops

A `For` loop repeats a block while changing a control variable through a defined range.

```pascal
Var
  Index : Integer;

Begin
  For Index := 1 To 5 Do
  Begin
    WriteLn('Iteration');
  End;
End.
```

The loop variable starts at `1` and advances to `5`.

## For Loop with the Counter Value

```pascal
Var
  Index : Integer;

Begin
  For Index := 1 To 5 Do
  Begin
    WriteLn('Loop number reached');
  End;
End.
```

To display the numeric counter, convert it to text with the appropriate MPL conversion function if the compiler does not perform implicit conversion.

Do not assume this is valid without verification:

```pascal
WriteLn('Loop: ' + Index);
```

## Descending For Loops

Pascal-style languages commonly use `DownTo` for descending ranges.

```pascal
For Index := 5 DownTo 1 Do
Begin
  WriteLn('Counting down');
End;
```

MPL support for `DownTo` should be confirmed with the active compiler.

## Single-Statement For Loop

A single statement may follow `Do` without a block:

```pascal
For Index := 1 To 5 Do
  WriteLn('Processing');
```

Using `Begin` and `End` is still recommended because it makes later changes safer.

## While Loops

A `While` loop checks its condition before each iteration.

```pascal
Var
  Count : Integer;

Begin
  Count := 1;

  While Count <= 5 Do
  Begin
    WriteLn('Processing item');
    Count := Count + 1;
  End;
End.
```

If the condition is false before the first check, the loop body does not run.

## Avoiding Infinite While Loops

The loop must eventually change something that affects its condition.

Incorrect:

```pascal
Count := 1;

While Count <= 5 Do
Begin
  WriteLn('Processing item');
End;
```

`Count` never changes, so the loop may continue indefinitely.

Correct:

```pascal
Count := 1;

While Count <= 5 Do
Begin
  WriteLn('Processing item');
  Count := Count + 1;
End;
```

## Repeat Until Loops

A `Repeat ... Until` loop checks its condition after the body executes.

```pascal
Var
  Count : Integer;

Begin
  Count := 1;

  Repeat
    WriteLn('Processing item');
    Count := Count + 1;
  Until Count > 5;
End.
```

Because the condition is checked last, the body runs at least once.

## Repeat with Multiple Statements

`Repeat` already begins a statement group, so a separate `Begin` and `End` block is generally not required.

```pascal
Repeat
  WriteLn('Enter Q to quit');
  { Read input here }
Until Choice = 'Q';
```

The loop terminates when the `Until` condition evaluates to `True`.

## While Versus Repeat

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
  ReadChoice;
Until Choice = 'Q';
```

## Nested Loops

A loop can appear inside another loop.

```pascal
Var
  RowIndex    : Integer;
  ColumnIndex : Integer;

Begin
  For RowIndex := 1 To 3 Do
  Begin
    For ColumnIndex := 1 To 3 Do
    Begin
      WriteLn('Processing cell');
    End;
  End;
End.
```

Nested loops multiply the total number of iterations. Keep them clear and avoid unnecessary nesting.

## Loop Conditions

Loop conditions can use comparison and logical operators.

```pascal
While (Count < Maximum) And IsActive Do
Begin
  Count := Count + 1;
End;
```

Use parentheses when combining several expressions.

See [Operators](Operators) and [Conditional Logic](Conditional-Logic).

## Input Loops

Loops are commonly used to validate user input.

Example structure:

```pascal
Repeat
  WriteLn('Enter Y or N');
  { Read input into Choice }
Until (Choice = 'Y') Or (Choice = 'N');
```

The exact input function should be added after it is verified and documented on the [Functions](Functions) page.

## File-Processing Loops

Loops are also used to process records or lines until an end condition is reached.

Example structure:

```pascal
While Not EndOfFile Do
Begin
  { Read and process one record }
End;
```

Actual file APIs and end-of-file behavior are Mystic-specific and should be documented on [File Handling](File-Handling).

## Exiting a Loop Early

Some MPL versions may provide a loop-exit statement such as `Break`, `Exit`, or another compiler-specific form.

Example concept:

```pascal
While IsActive Do
Begin
  If ShouldStop Then
  Begin
    { Exit the loop }
  End;
End;
```

Do not publish a specific early-exit keyword as supported until it has been verified.

Where no early-exit statement is available, use a Boolean control variable:

```pascal
Var
  KeepRunning : Boolean;

Begin
  KeepRunning := True;

  While KeepRunning Do
  Begin
    If ShouldStop Then
      KeepRunning := False
    Else
      WriteLn('Continuing');
  End;
End.
```

## Skipping an Iteration

Some languages support `Continue`, but MPL support must be verified.

Without a verified `Continue` statement, structure the loop conditionally:

```pascal
For Index := 1 To 10 Do
Begin
  If Index <> 5 Then
  Begin
    WriteLn('Processing item');
  End;
End;
```

## Modifying the For Loop Variable

Do not change the `For` loop control variable inside the loop unless the compiler documentation explicitly allows it.

Avoid:

```pascal
For Index := 1 To 10 Do
Begin
  Index := Index + 1;
End;
```

Changing the control variable can produce undefined or compiler-specific behavior.

## Boundary Behavior

The final value in a Pascal-style `For` loop is usually included.

```pascal
For Index := 1 To 5 Do
```

This normally runs five times: `1`, `2`, `3`, `4`, and `5`.

Verify:

- Whether the ending value is inclusive
- What happens when the starting value is greater than the ending value
- Whether the ending expression is evaluated once or on every iteration
- Whether `Byte`, `Integer`, and `LongInt` can all be loop variables
- Overflow behavior at numeric boundaries

## Common Errors

### Forgetting to update a While-loop condition

```pascal
While Count < 10 Do
Begin
  WriteLn('Running');
End;
```

This may never terminate.

### Reversing an Until condition

`Repeat ... Until` stops when the condition becomes true.

```pascal
Repeat
  Count := Count + 1;
Until Count > 5;
```

Do not treat `Until` as though it means “while.”

### Off-by-one errors

```pascal
For Index := 0 To 10 Do
```

This normally runs eleven times, not ten.

### Using the wrong loop type

A `While` loop may run zero times. A `Repeat ... Until` loop runs at least once.

Choose the form that matches the required behavior.

### Assuming unsupported control statements

Do not assume the compiler supports:

```text
Break
Continue
```

Verify them before use.

## Example Program

```pascal
Var
  Index       : Integer;
  Count       : Integer;
  KeepRunning : Boolean;

Begin
  For Index := 1 To 3 Do
  Begin
    WriteLn('For loop iteration');
  End;

  Count := 1;

  While Count <= 3 Do
  Begin
    WriteLn('While loop iteration');
    Count := Count + 1;
  End;

  Count := 1;

  Repeat
    WriteLn('Repeat loop iteration');
    Count := Count + 1;
  Until Count > 3;

  KeepRunning := True;

  While KeepRunning Do
  Begin
    WriteLn('Controlled loop');
    KeepRunning := False;
  End;

  WriteLn('|PA');
End.
```

This example demonstrates:

- `For ... To ... Do`
- `While ... Do`
- `Repeat ... Until`
- Explicit counter updates
- Boolean-controlled loop termination

## Verification Checklist

Before marking this page verified, test and record:

- `For ... To ... Do` syntax
- Inclusive or exclusive ending values
- `DownTo` support
- Valid loop-variable types
- Behavior when the starting value exceeds the ending value
- Whether the ending expression is evaluated once
- `While ... Do` syntax
- `Repeat ... Until` syntax
- Nested-loop behavior
- `Break` or equivalent support
- `Continue` or equivalent support
- Whether modifying a `For` variable is rejected
- Numeric overflow behavior
- Loop behavior across supported Mystic versions

Suggested verification record:

```text
Mystic version:
Operating system:
Compiler path or build:
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

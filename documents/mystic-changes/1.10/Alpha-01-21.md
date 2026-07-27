# Mystic 1.10 Alpha 1 through Alpha 21

This phase contains the largest concentrated redesign of Mystic Programming Language in the 1.10 cycle. It also introduced new development tools, reworked data and menu behavior, and laid the foundation for the later Internet and message-network features of the release.

> **MPY status:** Embedded Python was not available in Mystic 1.10.

## Alpha 1 — Initial 1.10 Development Foundation

### Mystic software changes

Alpha 1 began the 1.10 development line and introduced broad internal changes that later alphas continued to refine. Data formats, editors, menu behavior, configuration records, and scripting integration were actively changing during this phase.

### MPL changes

MPL began moving away from several historical MPE-specific language forms and toward a more conventional Pascal-style grammar.

Key migration direction included:

- More explicit statement and block structure
- Stronger declaration and type checking
- More predictable procedure and function syntax
- A new compiler and development-tool direction that became MPLC and MIDE

### Compatibility impact

Programs written for older MPE engines should not be assumed to compile unchanged throughout the 1.10 alpha cycle. Source and compiled output should be backed up before testing.

## Alpha 2 — Pascal-Style Control Flow

### MPL changes

Control-flow syntax was standardized around Pascal-style keywords.

Historical forms such as:

```text
PEnd
WEnd
FEnd
EndIf
ElseIf
```

were replaced or superseded by combinations of:

```pascal
Then
Do
Begin
End
Else If
```

Modern examples should use:

```pascal
If IsReady Then
Begin
  WriteLn('Ready');
End;
```

Loops use `Do`:

```pascal
While Count < 10 Do
Begin
  Count := Count + 1;
End;
```

### Compatibility impact

Older source using historical terminators required conversion. This was not merely a formatting change; it altered how the parser identified controlled statements and blocks.

## Alpha 3 — Declarations, Arrays, and Comments

### MPL changes

Variable declarations moved toward Pascal-style notation:

```pascal
Var Count : Integer;
```

Array indexing used square brackets:

```pascal
Scores[Index]
```

Comment handling expanded around forms such as:

```pascal
// Line comment

(*
  Pascal-style block comment
*)

/*
  Alternate block comment
*/
```

The accepted comment forms changed during the development cycle. Current documentation should use `//` and `(* ... *)` unless a target compiler has been tested with another form.

### Compatibility impact

Older comment markers and declaration layouts could fail under the redesigned parser. Included source files required review because a parser failure in an include could prevent the complete program from compiling.

## Alpha 4 — Procedure and Function Structure

### MPL changes

Procedures and functions were aligned more closely with Pascal-style source organization.

Example procedure:

```pascal
Procedure DisplayMessage(Message String);
Begin
  WriteLn(Message);
End;
```

Example function:

```pascal
Function AddValues(A Integer, B Integer) : Integer;
Begin
  AddValues := A + B;
End;
```

The function result continued to be assigned through the function name.

### Developer impact

The change encouraged reusable routines instead of large monolithic program blocks. Parameter order and type matching became more important as compiler validation improved.

## Alpha 5 — MIDE and MPLC Development Workflow

### Mystic software changes

Mystic's MPL development workflow gained dedicated tooling:

- **MPLC** for command-line compilation
- **MIDE** for integrated source editing and compilation

### MPL impact

Compiler errors became more structured and useful for locating source problems. This also made older source incompatibilities more visible because the new parser rejected forms the earlier engine had tolerated.

### Upgrade actions

- Record the exact MPLC build used for every test.
- Recompile source rather than reusing older compiled programs.
- Treat a successful compile as only the first validation step; runtime behavior must also be tested.

## Alpha 6 — File Type and File-I/O Redesign

### MPL changes

Older record-oriented functions such as the historical `fOpen`, `fReadRec`, and `fWriteRec` model were replaced by a more general file interface.

The redesigned model introduced concepts such as:

- A `File` variable type
- Assigning a filename to a file variable
- Opening existing files
- Creating or rewriting files
- Reading and writing values with explicit record sizing
- Checking `IoResult` instead of automatically terminating the program on every file error

Representative structure:

```pascal
Var DataFile : File;

Begin
  fAssign(DataFile, FileName);
  fReset(DataFile);

  If IoResult <> 0 Then
  Begin
    WriteLn('Unable to open file');
    Exit;
  End;

  fClose(DataFile);
End;
```

### Compatibility impact

File-processing programs required careful migration. Record size, current file position, and error handling could differ from the older specialized functions.

## Alpha 7 — Input and Terminal Behavior

### Mystic software changes

Terminal input and display behavior continued to be refined for ANSI, telnet, and local-console use.

### MPL changes

Input-oriented functions gained more explicit behavior. `OneKey`, for example, gained an echo-related Boolean option so the caller could control whether a selected key was displayed.

### Compatibility impact

Interactive programs should test:

- Local console input
- Telnet input
- ANSI and non-ANSI terminals
- Echoed and hidden input
- Escape and extended-key sequences

## Alpha 8 — Signed Numbers and Real Values

### MPL changes

Expression and numeric support expanded to handle negative values more consistently and to provide a `Real` floating-point type.

```pascal
Var
  Balance : LongInt = -25;
  Average : Real = 0.0;
```

### Developer impact

Programs could perform broader calculations without encoding every negative result through manual workarounds. Developers still needed to select types carefully because unsigned types such as `Byte`, `Word`, and `Cardinal` cannot represent negative values.

## Alpha 9 — Mathematical Precedence and Parentheses

### MPL changes

MPL gained standard mathematical order of operations and parentheses.

```pascal
Result := 2 + 3 * 4;
Result := (2 + 3) * 4;
```

The first expression evaluates multiplication before addition. The second evaluates the parenthesized addition first.

The modulus operator moved toward `%`:

```pascal
Remainder := Value % 10;
```

Power calculations used `^`:

```pascal
Squared := Value ^ 2;
```

### Compatibility impact

This is a major behavior change. Older engines were often described as evaluating expressions from left to right. A program could compile successfully but produce different numeric results after upgrading.

### Upgrade actions

Review every nontrivial arithmetic and Boolean expression. Add parentheses wherever the intended grouping must be explicit.

## Alpha 10 — Sized Strings and Character Access

### MPL changes

Strings could declare an explicit maximum length:

```pascal
Var UserName : String[30];
```

Character values could be represented by numeric character codes:

```pascal
SpaceCharacter := #32;
```

Strings could be indexed as character collections:

```pascal
If UserName[1] = 'S' Then
  WriteLn('Name begins with S');
```

### Compatibility impact

String indexing begins at the Pascal-style first character position. Code should not assume C-style zero-based indexing.

Assignment to a sized string should be tested for truncation behavior when the source is longer than the destination.

## Alpha 11 — Local Variables and Declaration Initialization

### MPL changes

Variables could be declared inside procedures and functions with improved local scope.

```pascal
Procedure DisplayName(Name String);
Var
  LabelText : String;
Begin
  LabelText := 'User: ' + Name;
  WriteLn(LabelText);
End;
```

Declarations could also provide an initial value:

```pascal
Var Count : Integer = 0;
```

### Developer impact

Local variables reduced dependence on program-level state and made routines easier to reuse. Local names can hide outer variables with the same name, so accidental shadowing should be avoided.

## Alpha 12 — Nested Routines and Recursion

### MPL changes

Procedures and functions could be declared inside another routine, allowing private helpers.

```pascal
Procedure DisplayBox;
  Procedure DisplayBorder;
  Begin
    WriteLn('+----------------+');
  End;
Begin
  DisplayBorder;
  WriteLn('|   Mystic BBS   |');
  DisplayBorder;
End;
```

Recursive procedures were added, allowing a procedure to call itself.

```pascal
Procedure CountDown(Value Integer);
Begin
  If Value <= 0 Then
    Exit;

  WriteLn(Int2Str(Value));
  CountDown(Value - 1);
End;
```

### Compatibility and safety

Every recursive routine requires a terminating condition. Unbounded recursion can exhaust the available call stack or runtime memory.

## Alpha 13 — `Var` Reference Parameters

### MPL changes

Pascal-style `Var` parameters allowed a routine to modify the caller's variable.

```pascal
Procedure ResetCount(Var Count : Integer);
Begin
  Count := 0;
End;
```

A writable variable must be passed:

```pascal
ResetCount(CurrentCount);
```

A literal or calculated expression is not a normal writable argument:

```pascal
ResetCount(10);          // Invalid concept
ResetCount(Count + 1);   // Invalid concept
```

### Developer impact

Reference parameters made it possible to return several values, populate records, and modify caller-owned state. Their side effects should be documented clearly.

## Alpha 14 — Arrays and Record Expansion

### MPL changes

Array support expanded, including multidimensional arrays in modern documented usage.

```pascal
Var Grid : Array[1..80, 1..25] of Char;
```

Three-dimensional structures also became possible:

```pascal
Var Values : Array[1..10, 1..10, 1..10] of Integer;
```

### Compatibility impact

Large arrays can significantly increase compiled size and runtime memory use. Bounds should match the actual problem, and every index should be validated.

## Alpha 15 — `Case` Statements and Loop Control

### MPL changes

Modern `Case` statements provided selection based on one expression:

```pascal
Case Choice Of
  'A': AddRecord;
  'D': DeleteRecord;
Else
  WriteLn('Unknown choice');
End;
```

MPL also gained loop-control statements:

```pascal
Break;
Continue;
```

`Break` exits the nearest loop. `Continue` skips the remaining statements in the current iteration.

### Compatibility impact

In nested loops, these statements affect only the nearest active loop. A function with an early `Exit` is often clearer when several nested loops must terminate together.

## Alpha 16 — String and Word Functions

### MPL additions

String-processing support expanded with functions for word-oriented parsing and replacement operations.

Important additions included concepts represented by functions such as:

- `WordCount` — count delimited words or fields
- `WordGet` — return a selected word or field
- `WordPos` — locate a word or field
- `Replace` — replace matching text
- `StrWrap` — wrap text to a target width
- `StrComma` — format numeric text with separators
- Path helpers such as `JustPath`, `JustFile`, `JustFileName`, and `JustFileExt`
- Environment access through `ReadEnv`
- Directory checks through `DirExist`

### Developer impact

These additions reduced the amount of custom parsing code needed in menu, file, message, and configuration scripts.

## Alpha 17 — Date, Time, and Timer Functions

### MPL additions

Date and timing support expanded with functions and variables for:

- Day-of-week calculations
- Julian date conversion
- Calculating days elapsed
- Converting date strings and packed date formats
- Validating date strings
- Measuring elapsed time in minutes

Identifiers introduced during the cycle included forms such as:

```text
DayOfWeek
DateJulian
DaysAgo
DateStrJulian
Date2Dos
Date2Julian
DateValid
TimerMin
```

### Compatibility impact

Date format `0` was no longer a universal shortcut for the user's selected format in every context. Scripts should use explicit formats when stable parsing is required.

## Alpha 18 — Program, Configuration, and Runtime Variables

### MPL additions

Programs gained more information about their execution context, including:

```text
ProgName
ProgParams
```

These allow a script to inspect its own invoked name and parameters.

Configuration and runtime variables also expanded, including temporary paths, terminal state, prompt behavior, and MCI-processing controls.

Examples of concepts added or expanded during this phase include:

```text
CfgTempPath
AllowArrow
IgnoreGroups
PausePos
AllowMCI
```

### Developer impact

A single compiled program could support several modes through command-line or menu parameters instead of requiring separate source files.

## Alpha 19 — Output and Screen Functions

### MPL additions

Output functions were separated by how they process pipe colors, MCI codes, and raw terminal data.

Important output families included:

```text
WritePipe
WritePipeLn
WriteRaw
WriteRawLn
```

Screen inspection expanded through functions such as:

```text
GetCharXY
GetAttrXY
```

Bitwise and bit-flag support also became more useful through:

```text
BitCheck
BitToggle
BitSet
```

MPL supported hexadecimal constants beginning with `$` and bitwise operators including `And`, `Or`, `Xor`, `Shl`, and `Shr`.

### Developer impact

Programs could choose whether output should be interpreted as Mystic-formatted text or sent more directly to the terminal. Raw-output routines should be tested across terminals because control sequences can behave differently.

## Alpha 20 — Renamed and Removed Built-ins

### MPL changes

Several identifiers were renamed for clearer naming.

| Older identifier | Replacement |
|---|---|
| `fExist` | `FileExist` |
| `fErase` | `FileErase` |
| `fCopy` | `FileCopy` |
| `CLS` | `ClrScr` |
| `USERLAST` | `UserLastOn` |
| `USERFIRST` | `UserFirstOn` |
| `FB...` file-base variables | `FBASE...` naming |

`IsNoFile` was removed after `DispFile` returned a Boolean result that directly reported whether a display file was found and shown.

Functions could be called without using their result when only their side effect was needed.

### Compatibility impact

Search all source and include files for historical names before recompilation. Ignoring a function result should be intentional because the result may report failure or user input.

## Alpha 21 — Compiled Extension, Limits, and Alternate Syntax

### Compiled extension

The compiled MPL extension changed from `.mpe` to `.mpx`.

```text
program.mps -> program.mpx
```

Menus, events, deployment scripts, startup logic, and documentation referencing `.mpe` required updates.

### MPL limits and identifiers

Compiler capacity increased substantially, including an increase in the maximum variable count from approximately 500 to approximately 2,500. Identifier lengths expanded to support names up to 30 characters in the modern compiler.

### Alternate syntax

An alternate C-like syntax mode was added for developers who preferred braces and C-style presentation. The Pascal-style syntax remained the primary documentation form and should be used consistently within a project.

### Include behavior

Include syntax changed during the 1.10 cycle. Later builds standardized a direct form such as:

```pascal
Include common.mps
```

Source should use the form accepted by the target MPLC build rather than relying on an early-alpha directive form.

### Performance

Parser and runtime work produced substantial speed improvements in some tests, with certain workloads reported as several times faster than earlier engines.

### Upgrade checklist for Alpha 1–21 changes

1. Convert historical block and loop terminators.
2. Add `Then` and `Do` where required.
3. Review comments and include syntax.
4. Migrate old file-I/O functions.
5. Review every mathematical expression for precedence changes.
6. Convert old built-in names.
7. Recompile all source with the target MPLC.
8. Deploy `.mpx` files and update menu commands.
9. Test local variables, nested routines, `Var` parameters, arrays, records, and loop control.
10. Compare runtime output with the older version rather than relying only on successful compilation.

## Documentation Impact

The changes in this phase affect nearly every core language page:

- MPL syntax
- Variables and data types
- Procedures and functions
- Conditional logic and loops
- Arrays and records
- Strings
- Operators
- File handling
- Compiler usage
- Include files
- Version compatibility

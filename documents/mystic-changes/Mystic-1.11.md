# Mystic 1.11 Changes

Status: **Detailed alpha-build history documented**

Mystic 1.11 was a shorter development cycle than 1.10, consisting of six alpha builds before the final release on November 6, 2015. The release concentrated on advanced MPL record handling, ANSI message support, editor behavior, message-network correctness, file-listing performance, date formatting, and stability.

> **MPY status:** Embedded Python was not available in Mystic 1.11. MPY scripting appeared later during the Mystic 1.12 development cycle.

## Release Direction

Mystic 1.10 established the modern MPL foundation. Mystic 1.11 refined that foundation and added capabilities that mattered to larger structured programs:

- Passing records to procedures
- Returning records from functions
- Using multidimensional arrays inside records more reliably
- Millisecond timing
- Four-digit date formats
- Embedded ANSI messages
- Improved full-screen editing and quoting
- Faster file-list generation
- More accurate network-message metadata

## Alpha 1 — Record Parameters, ANSI Messages, and File-List Performance

### Mystic software changes

#### FIDOPOLL host validation

FIDOPOLL was corrected so blank or invalid host fields were skipped safely rather than producing an unusable poll attempt.

SysOps should still validate every configured node because a skipped node can hide an incomplete network configuration.

#### ANSI abort behavior

ANSI display abort handling improved in two ways:

- Mystic waited for an active ANSI escape sequence to complete before treating input as an abort.
- Time-based ANSI abort behavior, introduced near the end of the 1.10 cycle, was incorporated into the 1.11 behavior.

This prevented partial escape sequences from leaving the terminal in an incorrect color, cursor, or mode state.

#### File-list generation performance

File-list output gained improved buffering. Large file bases could be listed more efficiently with fewer small output operations.

Programs that compare output timing should account for buffering. Data may appear in larger bursts rather than one line at a time.

#### Node chat and topic corrections

Node-chat and topic handling received fixes. Interactive chat state became more consistent across entry, topic changes, and exit.

#### Installer and keyboard behavior

Installer keyboard handling was corrected for cases where input was not processed consistently.

#### Node-message abort

The Enter key could abort or dismiss node-message display in supported contexts, improving usability when long or repeated messages were shown.

#### Embedded ANSI messages

Mystic gained support for ANSI-formatted message content. Users could upload ANSI or pipe-formatted content into the full-screen editor and save ANSI messages at a controlled width, commonly 79 columns.

This affected:

- Message entry
- Message quoting
- Message display
- Network export
- Terminal compatibility
- Line wrapping
- Storage and transfer size

A reader must distinguish ANSI message data from plain text and should not blindly strip control sequences unless the caller explicitly requests plain output.

#### FTP protocol response

The FTP server returned the standard `502` response for unsupported commands, improving compatibility with FTP clients that expect protocol-correct failure codes.

### MPL changes

#### Passing records by value

A user-defined record could be supplied to a procedure as a value parameter.

```pascal
Type
  UserSummary = Record
    Name : String[30];
    Calls : LongInt;
  End;

Procedure DisplayUser(User UserSummary);
Begin
  WriteLn(User.Name);
  WriteLn(Int2Str(User.Calls));
End;
```

The procedure receives the record's data but should not modify the caller's original record through a normal value parameter.

#### Passing records by reference

A record could also be passed through a `Var` parameter so the procedure could modify the caller's record.

```pascal
Procedure ResetUser(Var User : UserSummary);
Begin
  User.Name := '';
  User.Calls := 0;
End;
```

### Compatibility impact

Record parameters can be large. Developers should test:

- Copy behavior for value parameters
- Mutation behavior for `Var` parameters
- Nested records
- Arrays inside records
- String fields
- Record fields matching Mystic's internal data formats

Do not assume a record defined by an older release has the same field layout as the target version.

### Upgrade actions

1. Recompile every routine using records.
2. Confirm whether each parameter should be value or `Var`.
3. Test that value parameters do not alter the caller.
4. Test that `Var` parameters deliberately update the caller.
5. Test ANSI messages with multiple terminal emulators.
6. Verify message export does not corrupt ANSI sequences or network-control lines.

## Alpha 2 — Network Time Zones, Quoting, and Editor Corrections

### Mystic software changes

#### `TZUTC` network metadata

Generation or handling of the `TZUTC` kludge was corrected. This control line communicates the sender's UTC offset in compatible network messages.

Accurate time-zone metadata improves:

- Message timestamp interpretation
- Cross-zone sorting
- Display of local and remote times
- Network interoperability

Programs should not derive a message's actual time solely from visible date text when structured time-zone metadata is present.

#### ANSI quote handling

Quoting ANSI messages was corrected so unwanted color or terminal sequences were removed or normalized where plain quoted text was expected.

A quote function must avoid copying arbitrary terminal-control sequences into a reply because they can:

- Distort the editor display
- Change colors unexpectedly
- Move the cursor
- Hide text
- Create unsafe terminal output

#### Editor and kludge fixes

Additional message-editor and network-kludge defects were corrected, improving consistency between saved message text and network metadata.

### MPL impact

MPL message tools should clearly separate:

- Visible message body
- ANSI-formatted body
- Quoted plain text
- Tear and origin lines
- FTN kludge lines
- Routing and time-zone metadata

A custom editor should not treat all stored lines as user-visible content.

### Compatibility impact

Older custom quote routines may produce different output than the corrected built-in editor. Compare quoting results before replacing Mystic's native behavior.

## Alpha 3 — Record Return Types, Millisecond Timing, Dates, and Prompt Changes

### Mystic software changes

#### MUTIL file-description handling

MUTIL improved logging and cleanup when extracting or processing file descriptions such as `FILE_ID.DIZ`. Temporary files were removed more reliably after processing.

#### Time-zone correction

Additional `TZUTC` behavior was corrected after Alpha 2, showing that time-zone handling remained version-sensitive during the cycle.

#### Zmodem inactivity handling

Zmodem activity reset the appropriate inactivity timer so an active transfer was not treated as an idle connection.

#### Auto-signature corrections

Automatic signatures received fixes to prevent incorrect insertion, duplication, or formatting.

#### Prompt-menu revision

Prompt-driven menu behavior was revised, affecting how prompts and command choices were presented and processed.

#### Date formatting

Date formatting gained a `FormatDate`-style capability and expanded formats with four-digit years.

New or revised formats included formats numbered 4, 5, and 6 in the relevant date-format system.

The purpose was to avoid ambiguous two-digit years and provide more suitable international or archival display.

#### File-description and message-editor fixes

Additional file-description and editor issues were corrected, especially around imported text, formatting, and display.

### MPL changes

#### Functions returning records

MPL functions could return a user-defined record.

```pascal
Type
  UserSummary = Record
    Name : String[30];
    Calls : LongInt;
  End;

Function GetDefaultUser : UserSummary;
Var
  User : UserSummary;
Begin
  User.Name := 'Guest';
  User.Calls := 0;
  GetDefaultUser := User;
End;
```

The caller could assign the complete result:

```pascal
CurrentUser := GetDefaultUser;
```

This made it possible to return several related values without modifying several `Var` parameters.

#### `TimerMS`

A millisecond timer became available:

```pascal
StartTime := TimerMS;
```

A program could compare timer values to measure short operations more accurately than whole-second or minute timers.

Timer wraparound, integer type, and platform resolution should be tested before using the value for long-running measurements.

#### New MCI controls

MCI codes represented by forms such as `|-Y` and `|-N` were added or expanded for conditional or display behavior.

Programs generating MCI text should confirm the exact meaning and state changes associated with each code.

### Compatibility impact

Record-return functions require the caller and callee to agree on the exact record type. Two records with similar-looking fields are not automatically interchangeable.

Four-digit date formats can change string lengths and screen alignment. Programs storing dates in fixed-width strings should be reviewed.

## Alpha 4 — Incremental Echomail Export and Message-Network Cleanup

### Mystic software changes

#### Incremental echomail export

MUTIL gained or improved incremental echomail export. Instead of scanning and exporting every possible message every time, it could track progress and process only messages not previously exported.

This improves performance but makes state tracking important. A damaged or reset export pointer can cause skipped or repeated messages.

#### TIC `REPLACES` duplicate handling

File-echo processing corrected duplicate behavior involving the TIC `REPLACES` field. This field indicates that an incoming file supersedes an older file.

The correction reduced incorrect duplicate rejection or incorrect replacement behavior.

#### Tear, origin, and kludge regeneration

The message editor more carefully stripped and regenerated network-specific lines such as:

- Tear lines
- Origin lines
- Kludge lines

This prevented stale network metadata from being carried into a newly edited or forwarded message.

#### Large quote crash

A crash involving quoted text larger than approximately 10 KB was corrected.

### MPL impact

Custom message editors should not preserve an old tear line or origin line when Mystic is expected to generate a new one. Scripts handling large quoted messages should use dynamic or adequately sized storage and should test compiler and runtime string limits.

### Upgrade actions

- Back up MUTIL state and message bases before enabling incremental export.
- Verify the first export after upgrade for duplicate or skipped messages.
- Test TIC replacement with copied file areas.
- Test large quoted messages.

## Alpha 5 — Forwarding, Crossposting, and Arrays Inside Records

### Mystic software changes

Forwarding and crossposting were corrected so network-specific metadata was stripped and recalculated for the destination context.

A forwarded message should not retain routing, origin, destination, or path information that belongs only to the original area.

### MPL fixes

Multidimensional arrays stored inside records received an important correction.

Example structure:

```pascal
Type
  ScreenBuffer = Record
    Cells : Array[1..80, 1..25] of Char;
  End;
```

Earlier behavior could compile or execute incorrectly when indexing such fields. Alpha 5 improved the parser or runtime handling.

### Compatibility impact

Every program using arrays inside records should be recompiled and tested. Validate:

- First and last valid index
- Every dimension
- Assignment between records
- Passing the record by value
- Passing the record by `Var`
- Returning the record from a function
- Clearing the record
- Compiled size and runtime memory use

## Alpha 6 — ANSI Draw Cleanup and Linux Font Fix

### Mystic software changes

#### ANSI drawing cleanup

ANSI drawing mode received cleanup and stability corrections. Screen state, drawing commands, or editor transitions became more consistent.

#### Linux Amiga-font switching

Font-switching behavior involving Amiga-style fonts was corrected on Linux.

A terminal may not support every font-switch sequence. Mystic and MPL programs should provide readable fallback output.

### MPL impact

Programs that draw ANSI interfaces should retest:

- Entry into drawing mode
- Exit from drawing mode
- Cursor visibility
- Cursor position
- Current text attribute
- Screen restoration
- Font changes
- Abort and disconnect handling

## Final Mystic 1.11 Release — November 6, 2015

### Final release state

The final release consolidated the six alpha builds into a stable update emphasizing:

- Structured MPL record handling
- Better ANSI-message support
- Correct message-network metadata
- More reliable editors and quoting
- Faster file-list output
- Better date formatting
- Improved transfer and terminal behavior

### MPL state at release

Important 1.11 additions and fixes included:

- Records passed to procedures by value
- Records passed by `Var`
- Records returned from functions
- Multidimensional arrays inside records corrected
- `TimerMS`
- Date-format expansion
- Improved behavior for structured and larger programs

### MPY state at release

Mystic 1.11 did not include embedded Python or `.mpy` scripts.

## Compatibility Impact

Mystic 1.11 is less disruptive than 1.10, but programs using records or messages require focused testing.

Potentially affected areas include:

- Record copying
- Record mutation through `Var`
- Function record results
- Arrays nested inside records
- ANSI message storage and display
- Quote conversion
- Network-message metadata
- Date-string width and format
- Millisecond timing
- Buffered file-list output

## Upgrade Actions

1. Back up message, file, network, and scripting data.
2. Recompile all record-heavy MPL programs.
3. Test value and `Var` record parameters.
4. Test record-returning functions.
5. Test multidimensional arrays inside records.
6. Verify ANSI message entry, quote, display, and export.
7. Validate `TZUTC`, tear, origin, and kludge handling.
8. Test incremental echomail export and preserve its state.
9. Test TIC `REPLACES` processing.
10. Test file-list generation on large bases.
11. Update fixed-width date displays for four-digit years.
12. Measure `TimerMS` behavior on the target platform.

## Verification Record

```text
Mystic version/build: 1.11
Operating system:
Architecture:
MPLC version:
Record value parameter tested:
Record Var parameter tested:
Record function result tested:
Multidimensional array in record tested:
ANSI message entry tested:
ANSI quoting tested:
Network kludges verified:
Incremental export tested:
TIC REPLACES tested:
TimerMS resolution tested:
Four-digit dates tested:
Notes:
```

## Documentation Impact

Mystic 1.11 affects documentation for:

- Records
- Procedures and `Var` parameters
- Functions and return types
- Arrays
- Timers
- Date and time
- ANSI and screen output
- Message editing
- Message bases
- Echomail and netmail
- File echoes and TIC files
- File descriptions
- Version compatibility

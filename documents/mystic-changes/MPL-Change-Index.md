# MPL and MPY Cross-Version Change Index

This index summarizes major scripting-language and runtime changes across Mystic releases and links to the standalone detailed version histories in this repository.

## Detailed Histories

### Mystic 1.10

- [Version overview](Mystic-1.10.md)
- [Alpha 1–21](1.10/Alpha-01-21.md)
- [Alpha 22–32](1.10/Alpha-22-32.md)
- [Alpha 33–43](1.10/Alpha-33-43.md)
- [Alpha 44–63 and final release](1.10/Alpha-44-63-and-release.md)

### Mystic 1.11

- [Complete Alpha 1–6 history](Mystic-1.11.md)

### Mystic 1.12

- [Version overview](Mystic-1.12.md)
- [Alpha 1–12](1.12/Alpha-01-12.md)
- [Alpha 13–24](1.12/Alpha-13-24.md)
- [Alpha 25–36](1.12/Alpha-25-36.md)
- [Alpha 37–42](1.12/Alpha-37-42.md)
- [Alpha 43–48](1.12/Alpha-43-48.md)

## MPL Language Timeline

### Mystic 1.05

- Early MPE execution and compiler stability
- Script execution and error handling still limited compared with later releases
- No MPY support

### Mystic 1.06

- Procedures, parameters, local variables, constants, and functions established as important MPL building blocks
- Compiler and runtime behavior became more structured
- No MPY support

### Mystic 1.07

- Expression handling and Boolean processing improved
- Runtime performance increased
- Additional record and BBS integration became available
- No MPY support

### Mystic 1.08 and 1.09

- Continued MPE/MPL stabilization
- More built-ins and BBS integration
- Transition toward the large 1.10 redesign
- No MPY support

### Mystic 1.10

Mystic 1.10 is the primary modern MPL migration boundary.

#### Syntax

- Pascal-style `Then`, `Do`, and `Begin`/`End`
- `Else If` replaces historical single-token forms
- Modern `For`, `While`, and `Repeat` structures
- Line and block comments
- Include-file changes
- Optional alternate C-like syntax

#### Expressions and types

- Standard operator precedence
- Parentheses
- Negative numbers
- `Real`
- `%` modulus and `^` power
- Sized strings
- Numeric character values
- String indexing

#### Scope and routines

- Local declarations
- Declaration initialization
- Nested procedures and functions
- Recursive procedures
- `Var` reference parameters
- Longer identifiers
- Higher compiler variable limits

#### Collections and records

- Multidimensional arrays
- Expanded record use
- Broader file and record handling

#### Control flow

- `Case`
- `Break`
- `Continue`

#### Compiler and output

- `.mpe` compiled output becomes `.mpx`
- MIDE and MPLC improvements
- Faster compiler and runtime behavior
- Improved execution-error visibility

#### Built-ins and integration

- Many renamed file, screen, user, and file-base identifiers
- New string, word, path, environment, date, timer, screen, output, and bit functions
- Program name and parameter variables
- Login and new-user integration
- Message and mail statistics
- MPL user-interface classes

### Mystic 1.11

- Records passed to procedures by value
- Records passed through `Var`
- Records returned from functions
- Multidimensional arrays inside records corrected
- `TimerMS`
- Four-digit date-format expansion
- Improved ANSI-message and editor behavior
- No MPY support

### Mystic 1.12

#### MPL file and compiler changes

- New buffered, record-size-aware file API
- MPX programs loaded fully into memory
- Larger compiled programs with a documented limit during part of the alpha cycle
- Improved nested arrays and records
- `Library` source handling
- Bulk and recursive MPLC options
- Error summaries and standard-I/O compiler output
- Better include lookup

#### MPL runtime and integration

- Unix-date conversions
- Expanded user and caller statistics
- User-record migration to Unix and Julian date fields
- IPv6-capable user data
- Password-policy and password-storage functions
- Theme and fallback variables
- Terminal-size variables
- Masked input changes
- Semaphore-path and ACS-result variables
- `fWriteStr`
- `after_login.mpx` and `before_menus.mpx`

## MPY and Embedded Python Timeline

### Before Mystic 1.12

Embedded Python and `.mpy` scripts were not available.

### Mystic 1.12 Alpha 1

- Embedded Python 2.7 engine introduced
- `.mpy` source extension
- Menu execution
- Theme-script lookup

### Early 1.12 alphas

- Prompt execution
- `getuser`
- `onekey`
- `keypressed`
- Script-parameter functions

### Middle 1.12 alphas

Python gained broader Mystic APIs:

- User dictionaries and ID lookup
- Configuration dictionaries
- Message-group and message-base dictionaries
- File-group and file-base dictionaries
- Prompt and prompt-information access
- Screen position, color, raw output, and cursor control
- ACS evaluation
- Node logging
- User-profile updates
- Keyboard stuffing
- File-list APIs

### Mystic 1.12 Alpha 37

Python became capable of implementing message applications through:

- `msg_open`
- `msg_seek`
- `msg_found`
- `msg_next`
- `msg_prev`
- `msg_gethdr`
- `msg_gettxt`
- `msg_close`

A default `msgread.mpy` demonstrated the message-reader lifecycle.

### Mystic 1.12 Alpha 39–45

- User dictionaries changed with the new record format
- Unix timestamps, Julian dates, IPv6, country, and statistics fields expanded
- Theme-fallback configuration fields added
- Terminal-size function added
- File-list and display integration expanded

### Mystic 1.12 Alpha 46

The original Python engine was replaced by one capable of loading Python 2 or Python 3.

Python 3 execution paths included:

- `GZ` menu command
- `-Z` command-line option
- Mystic-DOS `PYTHON3`
- `~` prompt execution

Python libraries must match Mystic's operating system, architecture, bitness, and selected Python major version.

### Mystic 1.12 Alpha 48

- Display and configuration discovery functions
- Expanded user and caller statistics
- More robust Python initialization
- Final documented Alpha 48 integration work

## Breaking and Migration-Sensitive Changes

| Release/build | Migration concern |
|---|---|
| Mystic 1.10 | Modern parser, expression evaluation, renamed built-ins, file I/O, and `.mpx` output |
| Mystic 1.11 | Record parameters, record returns, arrays inside records, ANSI messages |
| 1.12 Alpha 17–18 | File indexes, MPX memory loading, libraries, compiler behavior |
| 1.12 Alpha 35 | Startup MPL login/new-user variables and required recompilation |
| 1.12 Alpha 39–40 | User-record and password-engine migration |
| 1.12 Alpha 44 | Complete theme-system replacement |
| 1.12 Alpha 45 | Terminal-size, masked-input, and display-file changes |
| 1.12 Alpha 46 | Replacement Python engine and Python 3 support |
| 1.12 Alpha 48 | Automatic login hooks and expanded service integration |

## Documentation Rule

Every syntax, built-in, record field, or MPY function documented in the wiki should state the earliest confirmed Mystic version or alpha build when that information is known.

Use a verification block such as:

```text
Mystic version/build:
MPLC version:
Python major version:
Operating system:
Architecture and bitness:
Compiler result:
Runtime result:
Date tested:
Notes:
```

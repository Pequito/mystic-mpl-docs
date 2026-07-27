# Mystic 1.12 Changes

Status: **Expanded into detailed Alpha 1 through Alpha 48 documentation**

Mystic 1.12 had a long alpha cycle spanning forty-eight documented builds. The release line introduced embedded Python, later replaced the original Python engine with a Python 2 and Python 3 capable implementation, expanded MPL file and record handling, changed the user and password databases, replaced the theme system, and delivered major server, network, security, terminal, message, file, and automation changes.

Because the full history is too large for one useful page, the detailed alpha entries are divided into chronological ranges.

## Detailed Alpha History

- [Alpha 1 through Alpha 12](1.12/Alpha-01-12.md) — initial Python 2.7 engine, `.mpy` execution, menu IDs, ANSI messages, user flags, file indexes, code pages, paths, and transfer changes
- [Alpha 13 through Alpha 24](1.12/Alpha-13-24.md) — socket reliability, Python input, MPX memory loading, MPLC improvements, library handling, MPL file I/O, uploads, passive FTP, QWK polling, spell checking, and 64-bit macOS
- [Alpha 25 through Alpha 36](1.12/Alpha-25-36.md) — echomail and QWK correctness, socket-engine migration, MIS2, IPv6, SSL certificates, file hatching, startup MPL, errors, NodeSpy, Python parameters, FTP, NNTP, and security events
- [Alpha 37 through Alpha 42](1.12/Alpha-37-42.md) — major Python dictionaries and message APIs, Unix dates, user-record conversion, password hashing, MPL password functions, SSH, BinkP queue and IPv6 improvements
- [Alpha 43 through Alpha 48](1.12/Alpha-43-48.md) — netmail encryption, new theme architecture, terminal-size APIs, masked input, unlimited display variants, replacement Python 2/3 engine, secure BinkP, login hooks, and final alpha expansion

A directory-level overview is available at [Mystic 1.12 Detailed Change History](1.12/README.md).

## Major Mystic Software Changes

Mystic 1.12 changed nearly every major subsystem during its alpha cycle.

### Servers and networking

- New and revised MIS/MIS2 server implementations
- BinkP, direct SSL BinkP, FTP, anonymous FTP, NNTP, NNTPS, SMTP, POP3, telnet, rlogin, SSH, and IPv6 work
- New socket engine
- More complete logging, auto-ban, country blocking, whitelisting, and IP-block events
- Concurrent outbound MIS polling intended to replace FIDOPOLL
- More accurate transfer queues, timeouts, busy files, and connection cleanup

### Message networks

- QWK and QWKE fixes and performance work
- QWK networking and QWKPOLL changes
- Echomail and netmail routing corrections
- Point-system and 5D address corrections
- Unified `MSGID` generation
- Consistent `TZUTC`
- Better `SEEN-BY`, `PATH`, tear, origin, and `@VIA` generation
- AreaFix and FileFix expansion
- File-echo hatching and EchoNodeTracker automation
- Optional AES-256 encrypted netmail through compatible Cryptlib configuration

### Messages and editors

- ANSI message uploading, storage, quoting, and subject handling
- Spell checking and multiple dictionaries
- Improved large-message and large-message-base performance
- Draft, tagline, quote, prompt, and editor changes
- New template-based index reader
- Large terminal and ANSI editor support

### Files

- New file indexes and duplicate indexes
- Case-aware and case-insensitive network behavior
- `FILE_ID.ANS` support
- File hatching and fileboxes
- Improved archive handling
- More capable file lists and Python file-list APIs
- SAUCE-aware display and description handling

### Users and security

- Never-delete and force-password-change flags
- Larger IPv6 and host fields
- Unix and Julian date storage
- Expanded voting and call statistics
- Separate QWK and REP paths
- New password engine with cleartext and PBKDF2-SHA512 modes
- Locked-account enforcement across more services
- Automatic whitelisting policies

### Themes and menus

- Unique menu IDs
- Larger menus and command counts
- New Theme editor access
- Complete replacement of the old theme database and path model
- Prompt, text, menu, and script inheritance
- Dynamic theme discovery
- Unlimited prompt length
- Wide-terminal layout and menu margins
- New display-file variants and terminal-size lookup

## Major MPL Changes

### File I/O

Mystic 1.12 introduced a newer buffered and record-size-aware MPL file API using a function family such as:

```text
FileOpen
FileEOF
FileRead
FileSeek
FilePos
FileSize
FileWrite
FileReadBlock
FileWriteBlock
FileClose
```

The exact signatures must be recorded against the target MPLC build.

### MPX runtime and compiler

- MPX programs loaded fully into memory for faster execution.
- Compiled-program capacity expanded, with a 128 KB limit documented during part of the cycle.
- Arrays and records inside records became more reliable.
- Include lookup improved.
- `Library` source could be skipped during standalone bulk compilation.
- MPLC bulk and recursive options expanded.
- Error summaries improved.
- MPLC wrote to standard I/O for third-party editor integration.
- Empty source files were skipped.
- Recursive `-ALL` scanning was corrected.

### Date and user integration

MPL gained Unix-date functions:

```text
DateUnix
DateU2D
DateD2U
```

User variables changed with the new record format, including Unix timestamps, Julian dates, IPv6-capable fields, country, password metadata, and expanded statistics.

### Password integration

MPL gained supported password interfaces:

```pascal
Function CheckPW(PW : String) : Boolean;
Procedure SetPW(PW : String);
Function ValidPW(PW : String) : Byte;
```

These replace unsafe direct password-field comparison or assignment.

### Themes and terminal state

New variables included concepts such as:

```text
CfgDefTheme
CfgTextFB
CfgScriptFB
CfgFallback
TermSizeX
TermSizeY
CfgSemaPath
AcsOKFlag
```

### Input and files

- `InputOptions` gained a custom password echo character.
- The Input class gained masked-input mode 4.
- `fWriteStr` wrote text without appending a newline.
- Login scripts could execute automatically through `after_login.mpx` and `before_menus.mpx`.

## Major MPY and Python Changes

### Initial engine

The first 1.12 alphas introduced an embedded Python 2.7 engine and `.mpy` scripts.

Python could run from:

- Menus
- Prompt replacements
- Command-line or shell contexts added during development
- Default example scripts

### Early API

The initial API added capabilities such as:

```text
getuser
onekey
keypressed
param_count
param_str
```

### Record dictionaries

Python gained dictionaries and lookup functions for:

- Users
- Configuration
- Message groups
- Message bases
- File groups
- File bases

Record-number and unique-ID lookup were distinct and should not be confused.

### Terminal and Mystic integration

Python gained functions for:

- Mystic output and raw output
- Cursor position and color
- Keyboard stuffing and key checks
- Prompt retrieval and prompt-information values
- ACS checks
- Node logging
- User profile updates
- Terminal-size detection
- Display-file lookup
- Configuration-file lookup

### Message API

Python gained message-base functions including:

```text
msg_open
msg_seek
msg_found
msg_next
msg_prev
msg_gethdr
msg_gettxt
msg_close
```

Later additions expanded statistics and last-read handling.

### File-list API

Python gained file-list functions built around an opened file-list handle, demonstrated through a distributed `filelist.mpy` example.

### Replacement engine and Python 3

Alpha 46 replaced the original Python engine with one capable of loading Python 2 or Python 3.

Separate Python 3 execution paths included:

- Menu command `GZ`
- Command-line option `-Z`
- Mystic-DOS `PYTHON3`
- Prompt execution using `~`

Python 2 and Python 3 source must be treated as separate compatibility targets unless deliberately written and tested for both.

## Critical Migration Boundaries

### Alpha 1

Embedded Python first appears. Python 2.7 library architecture must match Mystic.

### Alpha 17–18

File duplicate indexes, MPX memory loading, compiler behavior, libraries, and compiled-size constraints change.

### Alpha 28–36

The socket engine and MIS2 infrastructure change. IPv6, server configuration, logging, events, and polling behavior require extensive retesting.

### Alpha 35

Startup MPL gains enough control to replace login and new-user selection. MPL recompilation is required because runtime variables changed.

### Alpha 37

Python becomes a practical application language through message, user, group, base, prompt, screen, access, and logging APIs.

### Alpha 39–40

User records and password storage change. Direct record readers and direct password-field handling become unsafe and incompatible.

### Alpha 44

The theme directory and inheritance model replaces the old theme database and separate global path model.

### Alpha 45

Terminal sizes, masked input, and display-file lookup change. MPL recompilation and theme asset migration are required.

### Alpha 46

The Python engine is replaced and Python 3 support is added. Scripts and native libraries must be matched to the selected major version and architecture.

### Alpha 48

Automatic login hooks, display discovery, expanded statistics, and final server and message-ID behavior become available.

## MPY Deployment Requirements

A production MPY deployment should document:

```text
Mystic alpha build:
Python major version:
Python library version:
Python library path:
Mystic architecture and bitness:
Operating system:
Script location:
Theme and fallback path:
Menu or prompt execution method:
Required user access:
Required external modules:
Failure and logging behavior:
```

## Upgrade Actions

1. Back up the complete BBS and all scripts before crossing alpha boundaries.
2. Read every detailed range file between the source and destination builds.
3. Run supplied upgrade utilities for data and theme changes.
4. Pack or rebuild message and file bases when the documented alpha requires it.
5. Recompile every MPL program after runtime-variable, record, Input, or compiler changes.
6. Match Python libraries to Python major version, operating system, architecture, and bitness.
7. Test every MPY script under the actual execution path used in production.
8. Stop reading raw user, message, file, or configuration records with older layouts.
9. Migrate password handling to official functions.
10. Convert old theme paths and random-display filenames.
11. Test IPv4 and IPv6 separately.
12. Test FTP, BinkP, SSL BinkP, NNTP/NNTPS, SSH, SMTP, POP3, QWK, QWKPOLL, and transfer protocols used locally.
13. Review all event, semaphore, auto-ban, country-block, whitelist, and firewall automation.
14. Test login hooks with a recovery account and a bypass plan.
15. Verify custom prompts and templates against the new defaults.
16. Record the final verified Mystic alpha, MPLC, Python, platform, and architecture.

## Verification Record

```text
Mystic version/build: 1.12 Alpha ___
Operating system:
Architecture and bitness:
MPLC version:
Python 2 library and path:
Python 3 library and path:
MPL recompiled:
MPY menu execution tested:
MPY prompt execution tested:
User dictionaries tested:
Message API tested:
File-list API tested:
Theme fallback tested:
Terminal sizes tested:
Password migration tested:
Servers and IPv6 tested:
Login hooks tested:
Notes:
```

## Documentation Impact

Mystic 1.12 requires dedicated documentation for:

- MPY overview and installation
- Python 2 versus Python 3
- MPY execution contexts
- MPY functions and dictionaries
- MPY message and file-list APIs
- MPL file I/O
- MPL compiler options and limits
- User-record and password changes
- Theme directories and fallbacks
- Terminal-size and display-file lookup
- Login hooks
- MIS2 and server security
- Network automation
- Version and alpha compatibility

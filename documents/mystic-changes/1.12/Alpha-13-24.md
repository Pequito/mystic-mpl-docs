# Mystic 1.12 Alpha 13 through Alpha 24

This phase stabilized new file indexes, improved sockets and transfers, expanded the first Python engine, redesigned MPL file I/O and MPX execution, improved MPLC and MIDE, added spell checking, and introduced additional QWK, FTP, upload, and menu features.

## Alpha 13 — File Index Stabilization

### Mystic software changes

Known problems in the new file-listing index were corrected. These problems could affect:

- File listing
- Tagging
- Searching
- Download selection
- Current-file position

### Scripting impact

MPL or MPY programs that enumerate file bases should use supported runtime APIs instead of reverse-engineering index files. Internal index formats can change between alpha builds.

### Upgrade action

Rebuild indexes using the tools supplied with the target build when upgrading from an affected alpha.

## Alpha 14 — Socket Reliability and Python `keypressed`

### Mystic software changes

- Socket output buffering was corrected to prevent data loss when buffers filled during Zmodem, FTP, or BinkP transfers.
- ANSI detection became more reliable on Windows.
- Echomail export produced additional error logging in selected failure cases.
- Debug behavior that made display-file processing perform unnecessary work was removed.
- Lightbar and grid menu key jumps were corrected for the new unique menu IDs.

### MPY and Python changes

Python gained:

```text
keypressed
```

This allowed a script to test whether keyboard input was waiting without necessarily blocking for a key.

Conceptual use:

```python
if mystic.keypressed():
    # Read or process pending input.
    pass
```

The exact module and call syntax should be verified against the target alpha's Python example.

### Developer impact

Polling `keypressed` in a tight loop can consume CPU. Scripts should yield, delay, or perform useful work between checks.

## Alpha 15 — Configuration Input and Network Corrections

### Mystic software changes

- Pressing Backspace as the first key while editing a configuration field cleared the existing value.
- Pressing Escape during configuration string input restored the original value and exited input.
- An obscure menu-system defect was corrected.
- Point-system packet addressing and related network fields received corrections during this part of the cycle.

### Scripting impact

Custom configuration editors should provide clear cancel and clear behavior rather than treating Escape and Backspace identically.

## Alpha 16 — Echomail Hub and Point-System Fixes

### Mystic software changes

- `auxNet` packet data was populated correctly for point systems.
- A significant defect affecting echomail hubs with multiple downlinks using raw packet mail was corrected.

### MPL and MPY impact

Scripts inspecting packet or network address data must handle point systems correctly. A point address is not equivalent to its boss-node address.

### Upgrade action

Systems acting as echomail hubs should inspect queues and logs after upgrading to ensure that raw packets reach every intended downlink.

## Alpha 17 — Duplicate File Indexes and Python User Fix

### Mystic software changes

- Duplicate-file scanning became case-insensitive even on Unix-like systems because TIC metadata from older DOS systems often used different filename case.
- Linux and macOS systems required a one-time file-base pack or index regeneration after upgrade.
- MUTIL handled invalid duplicate-database sizes more safely.
- A zero or negative duplicate-database size disabled duplicate tracking.
- MUTIL refused to run when the semaphore directory did not exist.

### MPY and Python fixes

A defect in Python user retrieval introduced by the previous alpha was corrected.

### Compatibility impact

Case-insensitive duplicate detection does not mean the filesystem became case-insensitive. The actual on-disk filename still matters for opening and transferring the file.

## Alpha 18 — MPX Memory Loading, MPLC Improvements, and Libraries

### MPL runtime changes

Compiled MPX programs were loaded completely into memory for execution.

Benefits included:

- Reduced repeated file access during execution
- Faster instruction dispatch
- Improved performance for many scripts

A compiled-program limit of approximately 128 KB applied during this implementation phase.

Performance comparisons reported significant improvements, including approximately 20% for some MPX workloads. Relative MPL and Python performance differed by architecture, with 64-bit builds narrowing some gaps.

### MPL structure changes

Support for arrays and records inside records continued to improve.

### MPLC and MIDE changes

Compilation status updated every 10% instead of every 1%, substantially reducing display overhead during bulk compilation.

MPLC tracked multiple compiler errors and printed a summary after processing.

Source beginning with:

```pascal
Library
```

could be treated as an include library rather than compiled as a standalone program during bulk compilation.

Compiler options expanded around forms such as:

```text
-ALL
-C
-P
-R
```

Their exact meaning should be documented from the target MPLC help output. Common purposes included recursive or bulk compilation and compiler-output control.

### Compatibility impact

Large programs could exceed the new in-memory compiled-size limit. Developers should split shared code into libraries and avoid embedding oversized static data where practical.

## Alpha 19 — MCI Truncation and Runtime Library Reversion

### Mystic software changes

A formatting MCI code of the form:

```text
|$Txx
```

could truncate a value to a specified maximum length.

MUTIL logged its version during startup.

Telnet and rlogin commands were corrected.

A runtime-library change from an earlier alpha was reverted to correct multiple small regressions.

### MPL impact

MCI truncation is display formatting, not data modification. Programs should not assume the underlying value was shortened.

### Compatibility impact

The library reversion demonstrates why alpha-specific verification matters: behavior introduced in one build could be rolled back shortly afterward.

## Alpha 20 — Upload and Runtime Maintenance

### Mystic software changes

This build continued upload, network, menu, and runtime corrections following the larger changes in Alphas 17 through 19.

### MPL and MPY impact

No single major language feature defines this build. Regression testing should focus on file upload, display files, menus, Python startup, and bulk MPL compilation.

## Alpha 21 — Include Search and Parser Stabilization

### MPL compiler changes

Include-file lookup was improved so a compiler could locate included source relative to the source file's directory in supported cases.

Parser or library optimizations that caused regressions were rolled back or corrected.

### Developer impact

Relative includes make projects more portable:

```text
project/
├── main.mps
└── include/
    └── common.mps
```

Projects should still avoid relying on the compiler's current working directory when a source-relative path is clearer.

## Alpha 22 — User-Name Trashcan, `menucmd.mps`, and MIDE Parameters

### Mystic software changes

- Upload defects introduced during a rewrite were corrected.
- Names listed in `trashcan.dat` were checked before Mystic offered to create a new account.
- A dedicated prompt informed the caller that the entered name was unacceptable.
- MIS received additional exception trapping in UI mode.

### MPL additions

A default script named:

```text
menucmd.mps
```

was added. It provided an example or central place for menu-command scripting.

MIDE was corrected so MPL scripts executed from the editor received program parameters properly.

### Security impact

Name rejection must happen before account creation. Scripts implementing alternate login or registration must enforce the same policy.

## Alpha 23 — MIS Stability, FTP PASV Hostname, and QWK Polling

### Mystic software changes

- Additional exception handling attempted to prevent MIS from stopping after selected BinkP failures.
- The FTP server gained a **PASV hostname** option. Mystic resolved the configured hostname at server startup and used the resulting address for passive data connections.
- QWK polling and related transfer behavior continued to develop.

### Compatibility impact

The PASV hostname must resolve to an address reachable by clients. A private address returned to Internet clients will prevent passive data connections.

### Scripting impact

Monitoring scripts should separately test FTP control connection success and passive data connection success.

## Alpha 24 — QWKPOLL, Spell Checking, and 64-bit macOS

### Mystic software changes

- A QWKPOLL crash introduced in Alpha 23 was corrected.
- The QWKPOLL FTP client received PORT-mode optimizations.
- QWK-uploaded netmail used the correct origin address for logged-in users.
- On-the-fly spell checking and word suggestions became available across supported platforms.
- Custom message-editor templates required new options to expose spell checking.
- Multiple dictionaries and established dictionary formats could be used.
- macOS builds moved to a newer compiler toolchain and became available in both 32-bit and 64-bit Intel forms.
- 64-bit macOS could improve performance in selected operations.

### MPL and MPY impact

Scripts launching or replacing the message editor should preserve spell-check configuration and editor-template expectations.

Native Python libraries must match the chosen macOS Mystic architecture. Moving from 32-bit to 64-bit Mystic also requires a matching 64-bit Python library.

## MPL File-I/O Redesign During This Phase

Mystic 1.12 introduced a newer buffered, record-size-aware MPL file interface with functions in the following family:

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

The exact parameter and return types are compiler-version-sensitive.

### Developer guidance

- Open files using the appropriate access mode.
- Check the returned status.
- Use record sizes consistently.
- Distinguish record reads from byte-block reads.
- Seek only to valid offsets.
- Close every successfully opened file.
- Do not read beyond end of file.
- Test files larger than available memory.

## Alpha 13–24 Upgrade Checklist

1. Rebuild file indexes after affected alpha upgrades.
2. Test socket transfers under sustained output.
3. Verify Python `keypressed` without busy looping.
4. Test point-system packet addressing.
5. Rebuild duplicate indexes on Linux and macOS after Alpha 17.
6. Confirm the semaphore directory exists before MUTIL runs.
7. Check MPX compiled sizes against the active limit.
8. Use `Library` for include-only MPL sources where supported.
9. Test MPLC bulk and recursive options.
10. Verify include paths from project subdirectories.
11. Enforce `trashcan.dat` in alternate registration scripts.
12. Test passive FTP behind NAT.
13. Test QWKPOLL through both passive and active FTP as used locally.
14. Update message-editor templates for spell checking.
15. Match Python library architecture to Mystic.
16. Test the new MPL file API with text, records, and binary blocks.

## Documentation Impact

This phase affects documentation for:

- Python input APIs
- File-base indexes and duplicate detection
- MPLC and MIDE
- Include libraries
- MPX size and performance
- MPL file handling
- Program parameters
- Login and registration validation
- FTP passive mode
- QWK polling
- Spell checking
- macOS and architecture compatibility

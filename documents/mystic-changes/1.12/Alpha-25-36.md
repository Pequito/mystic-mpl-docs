# Mystic 1.12 Alpha 25 through Alpha 36

This phase concentrated on echomail and file-echo correctness, QWK performance, the MIS2 server transition, IPv6, SSH, BinkP, FTP, message identifiers, file hatching, startup scripting, global error logging, NodeSpy, Python parameters, and improved script logging.

## Alpha 25 — File Index Displays, Circular Paths, and MIS Threads

### Mystic software changes

- New file-index display files were introduced or expanded. Existing themes needed the corresponding `file_index` display assets copied into their text directories.
- MUTIL detected circular-path echomail more accurately and moved affected messages to the configured duplicate base.
- MIS returned to Mystic's custom threading library while server stability was evaluated.

### Scripting impact

Theme-aware scripts should not assume every theme contains newly introduced assets. Use fallback logic or validate required files during installation.

Network tools should report circular-path handling rather than silently discarding affected messages.

## Alpha 26 — Message Headers, `MSGID`, and Point-System Circular Checks

### Mystic software changes

- A MUTIL defect introduced in Alpha 25 that could write an incorrect message header was corrected.
- Re-editing a message preserved the original `MSGID` instead of generating a new one.
- Circular-path checks correctly handled point systems.

### Compatibility impact

A message's identity should remain stable when only its text is edited. Scripts should not generate a replacement `MSGID` unless they are creating a genuinely new message.

## Alpha 27 — QWK Packet Compatibility and Large Message-List Performance

### Mystic software changes

- A QWK packet issue affecting some readers was corrected.
- Returning to a message list after reading began with the last-read message positioned at the top.
- `Home` and `End` navigation in very large message lists became dramatically faster.
- DIZ import in the File Listing Editor was corrected.
- `automessage.mps` was updated for the newer message editor.

### MPL impact

Customized copies of default MPL scripts should be compared with the updated distributed version. Do not overwrite local changes blindly; merge editor-interface changes deliberately.

## Alpha 28 — Duplicate Tracking and Socket-Engine Migration

### Mystic software changes

- A remaining duplicate-system issue affecting point systems was corrected.
- A file-area menu action broken during the file-index transition was fixed.
- Mystic 1.12 adopted the newer Mystic 2.0 socket engine.
- Internet-session menu commands using a nonstandard port changed from semicolon syntax to a `/port=` option.
- MIS2 stability and threading support were corrected.
- Echomail-node password input no longer forced all passwords to uppercase, although only session passwords were intended to be case-sensitive.
- Next and previous file-group navigation was corrected.

### Compatibility impact

Existing menu commands using forms such as:

```text
address;port
```

needed conversion to:

```text
address /port=<number>
```

Connection wrappers and documentation should be updated together.

## Alpha 29 — Password Presentation and MIS2 Stability

### Mystic software changes

This build completed or stabilized several Alpha 28 changes, including socket-engine integration, echomail-node password presentation, threaded features, and MIS2 operation.

### Security impact

Even when a network password is case-insensitive, it should be stored and logged as a secret. Do not normalize a case-sensitive session password.

## Alpha 30 — SSH, Long Log Lines, BinkP, Locks, and MIS2 BinkP

### Mystic software changes

- Linux SSH received extensive fixes.
- Log lines could exceed 255 characters.
- BinkP handled address strings longer than 255 characters.
- The file-find `/ALLGROUP` option was renamed to `/GLOBAL`.
- The tagging command gained `/NOASK`.
- Stale `.BSY` files were considered stale after two hours instead of one day for applicable echomail and polling locks.
- MIS2 gained a BinkP server type.
- LHA Level 1 archive viewing with embedded directories was corrected.

### Scripting impact

Log parsers must not assume a 255-character maximum. Scripts using renamed command options must migrate to the current spelling.

Lock cleanup should verify the owning process before deleting a busy file. A long-running operation may still be valid.

## Alpha 31 — MIS2 Threads, Echomail Address Defaults, and Certificates

### Mystic software changes

- MIS2 switched to the Free Pascal thread library for stability testing.
- The `MX` command defaulted to the current message base's address when posting echomail instead of falling back to `0:0/0`.
- MIS2 stopped recreating `ssl.cert` when the file already existed.
- MIS2 log timestamps were corrected.

### Compatibility and security impact

Scripts posting network text should still pass an explicit address when deterministic routing is required.

Certificate files should be backed up and protected. Automatic recreation can unexpectedly replace a trusted or deployed certificate, so the new preservation behavior was important.

## Alpha 32 — Large QWK Messages and Unified FTN Metadata

### Mystic software changes

- QWK packet corruption involving messages larger than 64 KB was corrected.
- Echomail packet parsing read seconds from message timestamps correctly when supplied.
- Duplicate `MSGID` generation was corrected in situations where multiple posts could receive the same identifier.
- MUTIL `PostTextFile` added `TZUTC` when posting to FTN-style bases.
- The `MX` menu command added both `MSGID` and `TZUTC` when posting to FTN network bases.

### Scripting impact

Custom post-text functions should generate the same required metadata as interactive posting. Missing `MSGID` or `TZUTC` can reduce duplicate detection and timestamp accuracy.

## Alpha 33 — Unified `MSGID` Generation and Platform Identification

### Mystic software changes

- MUTIL TIC processing logged more information when opening and closing TIC files.
- Mystic replaced several separate `MSGID` implementations with one shared generator.
- Tear lines included operating-system bitness.
- Product text changed from `OSX` to `macOS`.

### Compatibility impact

Scripts parsing tear lines should not depend on an exact old product string. `MSGID` should be treated as an opaque identifier rather than parsed for internal assumptions.

## Alpha 34 — JAM Correctness, Xmodem/CRC, File Hatching, and MIS2

### Mystic software changes

- A bad-memory-reference defect affecting several areas was corrected.
- `PostTextFile` correctly accounted for tear and origin lines when splitting large network posts.
- `-VER` output included operating-system information.
- JAM message-write buffering increased from 8 KB to 32 KB.
- JAM preserved trailing spaces in `MSGID` values where needed for compatibility.
- JAM reply and recipient CRC calculations were corrected to lowercase strings as required by the format.
- Message-base packing could regenerate affected CRC data.
- Message Index Reader statistics became substantially faster.
- Node hangup detection improved, reducing ghost users and stuck processes.
- An experimental internal Xmodem/CRC protocol was introduced through a protocol entry such as `@XMODEMCRC`.
- MIS2 shutdown became noninteractive and displayed a completion message.
- File entries gained a Hatch command.
- Mystic created a `filebone.out` semaphore when a file was queued for hatching.
- MUTIL's TIC processor generated TIC files and copied the file into linked-node fileboxes.
- The echomail-node editor gained `/F` to view file-base links in addition to `/E` for message-base links.
- MIS2 received substantial stability work.
- Pass-through packet addressing was changed for compatibility with stricter BBBS security behavior.
- Mystic could generate a node filebox directory automatically from the network domain and FTN address.

### MPL and automation impact

Event scripts can watch `filebone.out` and launch a controlled MUTIL file-toss or hatch process. Prevent overlapping executions by using a lock or checking active process state.

File-hatch automation must validate:

- Source file existence
- Linked nodes
- File-base permissions
- TIC metadata
- CRC values
- Filebox paths
- Free disk space

## Alpha 35 — File-Echo Hubs, Startup MPL, Errors, Themes, and NodeSpy

### Mystic software changes

- File bases gained a **File Echo Hub** flag. A hub could accept a file without automatically forwarding it to downlinks unless the SysOp hatched it manually.
- Hatch CRC and TIC `Origin` generation were corrected.
- Area-index statistics became dramatically faster, trading some accuracy for speed in selected calculations.
- Mystic created a central `errors.log` intended to collect errors from Mystic, MIS, MUTIL, and other components.
- Node logs were cleaned up for later rolling and log-level support.
- Startup and shutdown handling was rewritten, including connection-loss and inactivity-timeout paths.
- Theme changes fell back to the default theme after an error.
- Configuration could be opened when no theme was available.
- MCI `|AA` enabled aborting for the current display; the existing `|AO` disabled it.
- Input functions were optimized.
- NodeSpy gained Insert and F1–F10 key transmission, SSH and rlogin modes, a separate Port field, Escape-to-abort, and clearer window titles.
- NodeSpy address entries using `:<port>` required manual migration to the separate Port field.
- The obsolete `-PATH` option was removed because `-CFG` could be used.
- Cut-and-paste cursor-loss bugs were fixed in the full-screen and ANSI editors.
- TIC files were written with DOS line endings for compatibility with older processors.
- MIS2 BinkP buffers were corrected from an unintended 128 bytes to the intended 32 KB.
- Random display-file selection expanded from numeric variants to numeric and lowercase alphabetic variants, allowing up to 36 alternates.
- Ghost-node termination attempted to notify the real node.
- Irregular Windows shutdowns were logged to `errors.log` and no longer left the same ghost-node state.
- `FILE_ID.ANS` took priority over `FILE_ID.DIZ`, supporting ANSI descriptions up to the documented art width.
- The email-check command gained `/UNREAD`.

### MPL changes

`startup.mps` gained:

```text
UserLoginNew
```

When set to `True`, Mystic could send the caller into the new-user application. Combined with existing login variables, MPL could replace both login and new-user selection logic.

All MPL programs required recompilation for this alpha because runtime or variable tables changed.

New configuration variables included concepts such as:

```text
CfgLoginTries
CfgPWTries
CfgEchoChar
```

### Security impact

A custom startup script becomes part of account creation and authentication. It must enforce rejected names, password policy, call limits, and account-protection rules consistently with Mystic.

## Alpha 36 — Script Parameters, Logging, MIS2 Events, IPv6, and Service Expansion

### Mystic software changes

- MUTIL logged failures to release node busy flags at all log levels and to `errors.log`.
- Door return cleanup removed node drop files from temporary directories.
- Routed pass-through netmail used the destination node's configured packet password.
- Inactive nodes were skipped during route calculations.
- The nodelist compiler recognized compressed update numbers `00` through `99`.
- Mystic logged door returns, every external command line, and normal node shutdown.
- Unix nodes watched door processes and sent `SIGTERM` after connection loss, preventing abandoned DOSEMU processes and high CPU use.
- BinkP unsecure-transfer duplicate checks used the correct unsecure directory.
- BinkP flood protection was adjusted for IREX interoperability.
- Uploaded REP messages received `TZUTC`.
- `-VER` included compile date and time.
- Tear lines temporarily included compile dates.
- BinkP sent an error frame before disconnecting after authentication failure.
- FIDOPOLL moved to the MIS2 BinkP engine.
- MIS2 became the active event engine; the older MIS event engine was disabled.
- Mystic logged every MPL and Python execution.
- Command-line login errors were logged.
- MIS2 gained local IPv4 and IPv6 country blocking using an `iplocation.bin` database and `iplocation.txt` policy.
- Message linking changed database-generation behavior and gained detailed debug logging.
- Node, FIDOPOLL, and MUTIL logs included compile dates.
- `GR` gained `/NOEXEC` to avoid executing first commands after a gosub return.
- A rare BinkP state bug that could lose a frame and cause a timeout was corrected.
- QWK-network hubs used the configured network Packet ID.
- FTP QWK packet visibility through `NLST` was corrected.
- Daily log rotation handled midnight rollover during active logging.
- MIS2 logs became unbuffered.
- MIS2 gained FTP and NNTP services.
- FTP added anonymous access controls, IPv6, periodic domain re-resolution, and per-slot passive ports.
- FIDOPOLL gained IPv6 selection.
- Auto-ban processing and an **IP Blocked** event could invoke a firewall command using the blocked IP.

### MPY and Python additions

Python gained script-parameter functions:

```text
param_count()
param_str()
param_str(0)
param_str(number)
```

Semantics included:

- `param_count()` — number of parameters
- `param_str()` — complete command line
- `param_str(0)` — script path and name
- `param_str(n)` — selected parameter

### MPL compiler changes

MPLC wrote output to standard I/O, improving integration with third-party editors and build tools.

### Security impact

Logging external command lines can expose secrets if passwords are passed as arguments. Avoid credentials on command lines.

An IP-block event that changes a host firewall is privileged automation. Use a narrowly scoped wrapper rather than granting unrestricted firewall control to the BBS account.

## Alpha 25–36 Upgrade Checklist

1. Merge updated default MPL scripts instead of overwriting custom versions.
2. Convert nonstandard port syntax to `/port=`.
3. Update renamed command options such as `/GLOBAL`.
4. Verify SSL certificates are preserved.
5. Pack JAM bases to regenerate corrected CRCs where required.
6. Test Xmodem/CRC only as an experimental protocol.
7. Configure and test file hatching with copied file areas.
8. Recompile all MPL programs for Alpha 35 variable-table changes.
9. Audit custom startup login and new-user logic.
10. Configure central error-log rotation and permissions.
11. Migrate NodeSpy address and port fields.
12. Copy required theme assets and test fallback.
13. Test `FILE_ID.ANS` and ANSI DIZ rendering.
14. Test Python command-line parameter handling.
15. Ensure external-command logging does not reveal secrets.
16. Validate MIS2 event ownership and disable duplicate legacy events.
17. Test FTP passive port ranges through the firewall.
18. Test IPv6 services separately from IPv4.
19. Treat firewall-block events as privileged security automation.
20. Monitor busy-file release errors in `errors.log`.

## Documentation Impact

This phase affects documentation for:

- Startup MPL and authentication
- MPY parameters
- MPLC editor integration
- Errors and logging
- File echoes and hatching
- JAM and message identifiers
- NodeSpy
- Display-file randomization
- ANSI file descriptions
- MIS2 events and services
- BinkP, FTP, NNTP, IPv6, and country blocking
- External doors and process cleanup
- Security and secret handling

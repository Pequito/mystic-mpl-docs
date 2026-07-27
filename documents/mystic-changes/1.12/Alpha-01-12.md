# Mystic 1.12 Alpha 1 through Alpha 12

This phase introduced embedded Python scripting, upgraded menu records, expanded ANSI and color-code handling, changed file indexing, improved network and transfer services, and added important user-security and configuration features.

## Alpha 1 — Embedded Python Arrives

### Mystic software changes

- Temporary upload working data was cleared after each mass-uploaded file so a later archive could not accidentally reuse a description extracted from an earlier archive.
- The full-screen editor's draw mode gained an **Upload ANSI** option.
- ANSI posted into a non-ANSI message base converted to plain text more reliably.
- Quote generation stopped wrapping a first-line kludge into the following visible line.
- MUTIL echomail export gained a `skip_online` option. When enabled, messages from users currently logged in could be held until logout, allowing them more time to edit before export.
- MCI display-code documentation was refreshed.
- File listings gave a clearer message when a base contained files but none were accessible to the current user.

### MPY and Python changes

Mystic introduced embedded Python 2.7 scripting.

The new source extension was:

```text
.mpy
```

Python scripts were located similarly to MPX programs, using the active theme's script directory and configured fallback locations.

A menu command was introduced to execute Python scripts. The early command was commonly identified as `GY`.

Conceptual menu configuration:

```text
Command: GY
Data: scriptname parameters
```

The extension could normally be omitted when Mystic performed the script lookup.

### Python compatibility requirements

The Python library loaded by Mystic had to match:

- Python 2.7 during this initial implementation
- The operating system
- Mystic's CPU architecture and bitness

A 64-bit Mystic executable could not load a 32-bit Python library, and the reverse was also true.

### Developer impact

MPY was new and the API was initially small. Python could not yet be treated as a complete replacement for MPL because many Mystic records and runtime operations were not exposed.

Early scripts should:

- Avoid external dependencies unless deployment is controlled
- Catch Python exceptions
- Log failures without exposing passwords
- Avoid assuming a console exists
- Test execution from an actual Mystic node

## Alpha 2 — Python Prompt Execution and Early Integration

### Mystic software changes

This build continued correcting early 1.12 behavior and integrating Python into more execution paths.

### MPY and Python changes

Prompt processing gained a way to execute a Python script directly. Early prompt-based execution used a Python script marker associated with the Python 2 engine.

This made it possible to use Python for:

- Dynamic prompt text
- Status values
- Small calculated displays
- Site-specific MCI-like output

### Compatibility impact

Prompt-executed scripts run during screen generation. They must return quickly and avoid blocking input. A slow script can delay every prompt display that calls it.

### Security impact

Prompt scripts should be treated as trusted code. They can run frequently and may receive user-specific state.

## Alpha 3 — Menu IDs, Menu Capacity, and Color-Code Compatibility

### Mystic software changes

- Menu items gained unique IDs instead of depending only on record position.
- Existing menus upgraded their IDs when loaded and saved in the menu editor.
- Grid menus became more practical because menu-item identity no longer changed simply because records were reordered.
- Maximum menu items increased to 99 per menu.
- Maximum commands increased to 25 per menu item.
- The message reader and full-screen editor gained support for additional color-code styles, including WWIV, Synchronet, PCBoard, and Wildcat conventions in addition to Mystic pipe codes and full ANSI.
- The Theme editor gained menu-editing access.

### MPL and MPY impact

Scripts that referenced menu positions should move toward stable menu IDs where the API exposes them. Code that processes message text must recognize that a message can contain several formatting dialects.

### Compatibility impact

A custom parser that recognizes only Mystic pipe codes may display foreign color codes literally or strip valid text incorrectly.

## Alpha 4 — File-I/O Performance and Echomail Hub Fixes

### Mystic software changes

- A memory leak caused by invalid echomail links was corrected.
- General file-I/O performance improved.
- A crash in a configuration editor tab was corrected.
- MUTIL improved regeneration and sorting of `SEEN-BY` and `PATH` lines when hubbing echomail to downlinks.
- Netmail routing while hubbing was corrected.
- New groups were corrected to receive unique IDs. Groups created under the affected build could require recreation if duplicate IDs had been assigned.
- Missing From, To, and Subject display data in the message reader was corrected.

### MPL and MPY impact

Scripts must not assume group IDs are valid merely because the group record exists. Migration tools should detect duplicate IDs.

Message-processing scripts should avoid regenerating network-control lines unless they implement the correct hub and downlink rules.

## Alpha 5 — User Protection, Password Changes, and Display Options

### Mystic software changes

- MUTIL removed duplicate `SEEN-BY` information while tossing messages.
- Display-file behavior gained additional options, including handling associated with new or unread content.
- System Configuration received visual cleanup and the beginning of configurable themes.
- The User editor exposed the existing **Never delete** flag so protected accounts could not be removed by user pack or purge operations.
- A **Force password change** flag required a user to choose a new password at the next login.

### Scripting impact

User-maintenance scripts must preserve account-protection and forced-password-change flags. Rewriting an entire user record from an older structure can accidentally clear new flags.

### Security impact

A forced password change must occur only after successful authentication and should not expose the old or new password in logs or error messages.

## Alpha 6 — NNTP Reformatting and Raspberry Pi Stability

### Mystic software changes

- NNTP posting reformatted long free-flowing lines more appropriately.
- Empty uploaded messages were handled more safely.
- Raspberry Pi login crashes were addressed.

### MPL and MPY impact

Scripts that prepare NNTP messages should not pre-wrap content in a way that conflicts with Mystic's own reformatting.

ARM and Raspberry Pi deployments should test native libraries used by MPY because Python library names, bitness, and paths can differ from x86 systems.

## Alpha 7 — Login and Platform Maintenance

### Mystic software changes

Alpha 7 continued correcting login and platform-specific defects. Some problems discovered in the Raspberry Pi or MIS login path were not fully resolved until Alpha 8.

### MPY impact

Python login scripts should be tested separately from interactive menu scripts because login occurs before the normal menu environment is completely established.

## Alpha 8 — Python `getuser` and `onekey`

### Mystic software changes

- A Raspberry Pi crash during MIS login was corrected.
- BinkP authentication logging was rewritten to be clearer.
- Event day-of-week handling and resource use were corrected or reduced.
- The User editor correctly saved the **Never delete** flag.
- A new menu command opened the Theme editor.
- Most older door menu commands were removed in favor of a smaller supported set, including `DD` and `D3`.

### MPY and Python additions

Python gained functions corresponding to:

```text
getuser
onekey
```

`getuser` exposed current user data through the early Python interface. `onekey` allowed controlled single-key input.

The distributed `testpython.mpy` was updated as functions were added, serving as an executable API example.

### Compatibility impact

Old door command configurations required migration to the supported commands. MPY scripts should not assume every user field available in MPL was already present in the Python dictionary.

## Alpha 9 — Mass Upload, File Index Case, Netmail, and Log Rolling

### Mystic software changes

- Online mass upload was rewritten for better performance while retaining lower memory use than MUTIL.
- Linux mass-upload index generation was corrected so it did not use Windows-style case-insensitive indexing that could create duplicate entries.
- A netmail-base selection error introduced in Alpha 8 was corrected.
- Windows disabled the window close button to reduce forced exits that could leave ghost users.
- MUTIL added daily log rotation in addition to size-based rotation.

Example daily log name:

```text
mutil.20160329.log
```

### Scripting impact

File scripts on case-sensitive systems must preserve the actual case of stored filenames while still accounting for network metadata created by case-insensitive systems.

Log-processing scripts should recognize both single rotating files and date-stamped daily logs.

## Alpha 10 — Code-Page Override and Upload Corrections

### Mystic software changes

- Upload and `xfer.log` problems were corrected.
- A command-line option allowed the session's output code page to be overridden:

```text
-CP<mode>
```

Example:

```text
mystic -CPutf8
```

A value containing UTF selected UTF-8 behavior; non-UTF values selected CP437-oriented behavior in the documented implementation.

### MPL and MPY impact

Scripts must not assume all sessions use CP437. Text length in bytes can differ from visible character length under UTF-8.

Avoid slicing encoded text by byte count unless the API explicitly operates on bytes.

## Alpha 11 — ANSI Subjects, Reply Prefixes, Paths, and Scan Efficiency

### Mystic software changes

- Message bases gained an option to add an `[ANSI]` subject prefix to ANSI messages, including an echomail-only mode.
- Replies stripped existing `[ANSI]` and `Re:` prefixes and regenerated them according to the reply content and configuration.
- Message bases gained an **Add Re: Prefix** option.
- Private and netmail bases were prevented from being exposed through NNTP even when misconfigured for NNTP visibility.
- A `-PATHS` command-line option opened System Directory configuration for easier path migration.
- QWK Path appeared in System Directories.
- New-message scans updated last-read pointers more efficiently.

### Scripting impact

A script must not determine whether a message is ANSI solely from the subject prefix. The prefix is configurable and recalculated.

Subject-normalization code should avoid stacking repeated prefixes such as:

```text
RE: RE: [ANSI] [ANSI]
```

Path migration scripts should include the QWK path and should normalize separators for the target operating system.

## Alpha 12 — File Scans, Editor Keys, ANSI Upload, and Zmodem Timing

### Mystic software changes

- File scans no longer failed simply because the first file base's data files were missing.
- Windows SysOp macros moved to `Ctrl+F1` through `Ctrl+F8` because plain function keys were used by editors and other interfaces.
- Uploading several ANSI files into the message editor was cleaned up.
- Downloads initiated during a file scan or search were corrected.
- Zmodem receive startup time increased from 30 to 90 seconds.
- Zmodem stopped unnecessarily retrying `ZFIN` while waiting for the final `OO` sequence from implementations that did not send it.
- Transfer timing allowed more time for overwrite prompts and user responses.

### MPL and MPY impact

Input scripts should not assume function keys are available for SysOp macros on every platform.

Transfer wrappers should allow the built-in protocol to manage its own startup and completion timing rather than imposing shorter script-side timeouts.

## Alpha 1–12 Compatibility Checklist

1. Install a Python 2.7 library matching Mystic's operating system and bitness before testing early MPY.
2. Verify the configured Python library path.
3. Test MPY from a menu, prompt, and login-related context separately.
4. Convert menu-position assumptions to unique menu IDs where possible.
5. Test multiple message color-code formats.
6. Check groups for duplicate IDs after affected early builds.
7. Preserve Never-delete and Force-password-change user flags.
8. Rebuild file indexes on case-sensitive platforms where required.
9. Update log parsers for daily MUTIL logs.
10. Test CP437 and UTF-8 sessions.
11. Normalize ANSI and reply subject prefixes.
12. Verify private and netmail bases remain hidden from NNTP.
13. Test file scans when early bases are empty or missing.
14. Test Zmodem with slow client response and SyncTerm-style completion behavior.

## Documentation Impact

This phase affects documentation for:

- MPY introduction and Python installation
- Menu integration
- Prompt scripting
- User dictionaries
- Keyboard input
- ANSI messages and color codes
- File indexes and case sensitivity
- MUTIL logging
- Code pages and UTF-8
- Message subjects and quoting
- Paths and portability
- Zmodem transfers

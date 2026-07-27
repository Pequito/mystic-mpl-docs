# Mystic 1.12 Alpha 43 through Alpha 48

The final documented 1.12 alpha phase replaced the theme architecture, expanded terminal-size support, introduced a replacement Python engine with Python 2 and Python 3 execution paths, added secure BinkP options, expanded network automation, and created new MPL and MPY hooks for themes, files, users, login events, statistics, and display-file discovery.

## Alpha 43 — Mystic-DOS, Split-Screen Chat, Larger Domains, and Netmail Security

### Mystic software changes

- Door command lines gained `%A` for the user's real name with spaces converted to underscores and `%B` for the user's real name with spaces preserved.
- The configured BBS domain length expanded from 25 to 45 characters.
- Mystic warned when a SysOp attempted to edit a message that had already been sent.
- Private user-to-user chat gained a split-screen interface.
- Themes required new `userchat.ini` and `userchat.ans` assets for the redesigned chat interface.
- Mystic-DOS, an internal command shell, was introduced or substantially expanded.
- File-base editors gained bulk selection and global editing.
- Echomail node records gained an encryption-key option for AES-256-encrypted netmail when Cryptlib was available.
- Netmail could be decrypted and re-encrypted at routing points when each hop had the appropriate configured key.
- AreaFix and FileFix accepted command forms with both leading and surrounding percent signs, such as `%LIST` and `%LIST%`.
- Netmail routing corrected UTC use in `@VIA` and point-system addressing.
- BinkP transmitted local time and time-zone information.
- MUTIL message packing created temporary files beside the message base rather than in Mystic's root temporary directory, allowing bases on separate filesystems to be packed safely.

### Security impact

Encrypted netmail depends on:

- Matching keys at the communicating nodes
- Protected configuration files
- A compatible Cryptlib installation
- Secure key exchange outside Mystic
- Correct routing configuration

Encryption hides content and subject from intermediate systems that do not possess the key, but routing metadata remains necessary for delivery.

### MPL password additions

Password interfaces were clarified and expanded:

```pascal
Function CheckPW(PW : String) : Boolean;
Procedure SetPW(PW : String);
Function ValidPW(PW : String) : Byte;
```

`CheckPW` compares a supplied password with the currently loaded user.

`SetPW` stores a password for the loaded user using the configured password engine.

`ValidPW` evaluates password policy and returns a status code. Documented policy failures included:

| Result | Meaning |
|---|---|
| `1` | Minimum length not met |
| `2` | Minimum uppercase letters not met |
| `3` | Minimum symbols not met |
| `4` | Minimum numbers not met |

### Developer guidance

Do not read or compare raw password fields. Use the supported functions so cleartext, case-sensitive, and PBKDF2 formats are handled correctly.

## Alpha 44 — New Theme System and Theme APIs

### Theme-system replacement

Mystic replaced separate default text, menu, and script paths with one root Themes directory.

Example structure:

```text
themes/
└── default/
    ├── theme.ini
    ├── prompts.txt
    ├── text/
    ├── menus/
    └── scripts/
```

`THEME.DAT` was deprecated and no longer used by the new architecture.

### Theme inheritance

A theme could inherit prompts, text files, scripts, and menus from fallback themes and ultimately the default theme when configured.

A theme no longer needed to duplicate every prompt. `prompts.txt` could contain only prompts customized by that theme.

Prompt text was no longer limited to 255 characters.

Themes became portable directories: copying a complete theme directory into the Themes root made it available without a separate database installation process.

### Input barriers

Themes gained left and right input-barrier characters and attributes. An MCI code could disable the barrier for the next string input.

### MPL configuration variables

New MPL variables included:

```text
CfgDefTheme
CfgTextFB
CfgScriptFB
CfgFallback
```

These exposed the default theme, text fallback, script fallback, and fallback-enabled state.

### MPY configuration variables

Python configuration dictionaries gained equivalents such as:

```text
deftheme
textfb
scriptfb
fallback
```

### Python prompt and file-list APIs

Python gained `setpinfo` for prompt-information values and a file-list API demonstrated by a default `filelist.mpy`.

The file-list family included concepts such as:

```text
fl_open
fl_close
fl_seek
```

with additional functions for iterating and retrieving file records.

### MPLC changes

- Recursive `-ALL` directory scanning was corrected.
- Empty `.mps` files were skipped instead of being compiled.

### Compatibility impact

This is a major filesystem migration. Scripts that directly assembled old text, menu, or script paths required conversion to the theme root and fallback model.

The Upgrade utility could convert existing installations, but every custom theme still required visual and functional testing.

## Alpha 45 — Terminal Sizes, Masked Input, Display Files, and Dynamic Layouts

### Terminal-size changes

Mystic expanded supported terminal sizes up to approximately 160 columns by 60 lines, with smaller configurations also supported.

Terminal size was no longer stored permanently in the user record. Mystic used the detected size for the current connection, falling back to the configured default when detection was unavailable.

### MPL terminal variables

New variables included:

```text
TermSizeX
TermSizeY
```

### MPY terminal function

Python gained:

```text
termsize()
```

returning the current terminal columns and rows.

### MPL masked input

`InputOptions` gained an additional character parameter defining the password echo character.

The MPL Input class gained mode 4 for masked string input.

Programs needed recompilation with the target compiler after the interface changed.

### New display-file randomization

The old `.ana`, `.anb`, and similar limited random-file system was removed.

The replacement allowed unlimited numbered variants:

```text
test.ans
test.1.ans
test.2.ans
test.10.ans
```

Mystic randomly selected an available variant.

### Terminal-size-specific display files

Display-file lookup could select variants matching the current terminal dimensions. This allowed layouts designed for 80-column, 132-column, or wider terminals without one script manually redrawing every layout.

### Display templates

Display lookup also expanded around terminal, theme, and template variants. The same discovery behavior affected MPL `DispFile` and Python `showfile`.

### Additional changes

- Python `setprompt` was corrected.
- BinkP and TIC timestamps stopped being incorrectly localized.
- Lightbar and grid extended-key jumps were corrected.
- Nodelist browser net values were corrected.
- User-setting menu actions could set file-list and message-reader types directly.
- Successful logins could automatically whitelist the user's IP according to user-flag or all-user policy.

### Security impact

Masked input hides display characters but does not make transport secure. Password entry should still occur over SSH or TLS where possible.

Automatic whitelisting reduces future blocking but should be limited to authenticated users and reviewed when accounts are shared or compromised.

## Alpha 46 — Replacement Python Engine and Python 3

### Python engine replacement

Mystic replaced the original embedded Python implementation with a new engine capable of loading Python 2 or Python 3.

The configured Python library had to match Mystic's:

- Operating system
- CPU architecture
- Bitness
- Requested Python major version

### Separate execution paths

Python 3 gained separate execution methods, including:

- A menu command identified as `GZ`
- A command-line option such as `-Z`
- A Mystic-DOS `PYTHON3` command
- A Python 3 prompt marker using `~`

The existing Python 2 paths were renamed or clarified, including Mystic-DOS `PYTHON2`.

### Compatibility impact

Python 2 source is not automatically valid Python 3 source. Common migration differences include:

- `print` statement versus function
- Text versus byte strings
- Exception syntax
- Dictionary iteration behavior
- Integer division
- Library names
- Unicode handling

A script should declare which major version it targets and be tested only through the matching Mystic execution path.

### Mystic software changes

- Linux RLOGIN and SSH stopped passing passwords as visible command-line options and instead used a private hashed mechanism.
- The older message index reader was removed in favor of the newer template-based index reader.
- BinkP gained a direct SSL port.
- Opportunistic BinkP TLS was removed.
- Non-SSL BinkP could be disabled by setting its normal port to zero while an SSL port remained configured.
- FIDOPOLL and echomail nodes gained a Use SSL option.
- Stale busy-file deletion was corrected.
- Cryptlib compatibility was improved.
- MIS gained a replacement outbound polling command supporting BinkP, BinkP SSL, FTP, and directory transfers with concurrent connections.
- MUTIL gained an EchoNodeTracker function for automated node suspension, subscription removal, queue cleanup, crash-to-hold conversion, and statistics reset.

### Security impact

Python libraries are native code loaded into Mystic's process. Use trusted library packages and restrict write access to their installation paths.

Direct BinkP SSL protects the connection but does not replace correct node passwords and address validation.

## Alpha 47 — Prompts, Doors, Network Automation, Layout, and Scripting Expansion

### Mystic software changes

- The write-email command used the configured Feedback To account when the caller selected SYSOP.
- Windows 32-bit could use DOSXTRN for DOS FOSSIL doors through the `DX` command, including SSH sessions.
- Prompts such as the More prompt and Yes/No prompt embedded their accepted hotkeys as the first word. Customized themes required prompt updates.
- Netmail routing and PING handling improved.
- Tagline, message-draft, editor, and save behavior expanded.
- Invalid email handling and user validation improved.
- TLS, server security, and network-service behavior received substantial development.
- User records and caller statistics expanded.
- Large-screen ANSI editing and menu margins improved support for wide terminals.
- Generated menus could draw non-destructively over prepared ANSI layouts.
- Email autovalidation only raised security when the configured target was higher than the current level.
- A new `fWriteStr` MPL function wrote text without adding line-ending characters.
- Group-change commands set the Mystic `OK` ACS flag according to success.
- Time-per-day reset behavior was corrected.
- Caller data could be reset through configuration.
- File-description Boolean searches were corrected.
- Mystic-DOS gained a mass-upload command.
- ACS gained an `OC` condition for the user's first call.

### MPL additions

```text
fWriteStr
AcsOKFlag
CfgSemaPath
```

`AcsOKFlag` allowed MPL to inspect or set the current result used by ACS-sensitive menu logic.

`CfgSemaPath` exposed the configured semaphore directory. MPL recompilation was required for the changed variable table.

### MPY additions

User dictionaries gained additional statistics and date fields, including Unix timestamp forms for first and last calls and expanded call statistics.

### Compatibility impact

Customized prompts must include the new hotkey prefix. Old prompt text may display correctly but fail to accept input as expected.

## Alpha 48 — Login Hooks, Display Discovery, User Statistics, and Final Alpha Expansion

### Login event scripts

Mystic introduced automatic MPL login hooks:

```text
after_login.mpx
before_menus.mpx
```

`after_login.mpx` runs after password validation and login checks.

`before_menus.mpx` runs after login initialization but before normal menu processing.

These hooks allow site-wide login behavior without editing every menu.

### Login precedence

The release clarified how configured start menus, user start menus, and login-event programs interact. Scripts should document whether they change the current menu or merely perform setup.

### MPY display discovery

Python gained functions such as:

```text
find_display
find_config
```

These resolve files through the active theme and fallback paths without forcing the script to reimplement Mystic's lookup rules.

### User and caller statistics

- Caller and last-caller statistics expanded.
- Python user variables expanded.
- New-user and call-count data became available to scripts and MCI.
- Mystic's Python loader became more robust when libraries were missing or initialization failed.

### Service security

FTP, SMTP, NNTP, and POP services enforced locked-user restrictions more consistently.

### Messages and display

- File descriptions became more SAUCE-aware.
- BinkP StartTLS and secure connection behavior continued to improve.
- Events and file indexing received additional fixes.
- Message IDs and `TZUTC` were generated more consistently for local, network, QWK, REP, MUTIL, and NNTP-created messages.
- NNTP gained global article IDs and secure NNTPS support.
- Message-base packing could generate missing message IDs.

### File and transfer changes

- TIC import gained an `ignore_case` option for case-sensitive operating systems and could normalize the TIC `File` field to the on-disk filename case for downlinks.
- Transfer commands gained protocol-selection and prompt-skipping options.
- Menu command data expanded to 200 characters.

### MPY and MPL deployment impact

The automatic MPX login hooks run for every applicable user. They must be fast, defensive, and safe when user, theme, menu, or terminal data is incomplete.

A failure in a login hook should log the error and allow a deliberate recovery path rather than trapping the user before menus.

## Alpha 43–48 Upgrade Checklist

1. Protect netmail encryption keys and install compatible Cryptlib only where needed.
2. Test `CheckPW`, `SetPW`, and `ValidPW` with every configured password mode.
3. Run the theme upgrade and inspect every custom theme.
4. Remove dependencies on `THEME.DAT`.
5. Use theme fallback variables or discovery functions instead of hard-coded old paths.
6. Recompile MPL after variable and Input interface changes.
7. Test 80x25, 132-column, and wide-terminal layouts.
8. Convert old random display filenames.
9. Install matching Python 2 and/or Python 3 libraries.
10. Mark every MPY script as Python 2, Python 3, or dual-compatible.
11. Update custom prompts with embedded hotkey definitions.
12. Copy new index, gallery, user-chat, and editor templates.
13. Test direct SSL BinkP and outbound MIS polling.
14. Review EchoNodeTracker automation before enabling destructive options.
15. Audit automatic IP whitelisting.
16. Recompile MPL for `CfgSemaPath` and other new runtime variables.
17. Test `after_login.mpx` and `before_menus.mpx` with a recovery account.
18. Verify locked accounts cannot authenticate through any service.
19. Pack message bases where needed to generate missing message IDs.
20. Test NNTPS, global article IDs, QWK/REP metadata, and case-insensitive TIC import.

## Documentation Impact

This phase affects documentation for:

- Themes and fallbacks
- MPL and MPY configuration variables
- Python 2 and Python 3 installation
- MPY file-list and display lookup APIs
- Terminal-size APIs
- MPL masked input
- Display-file naming
- Password functions
- Login hooks
- ACS integration
- Semaphores
- Netmail encryption
- BinkP SSL and MIS polling
- Network automation
- User statistics
- NNTP and NNTPS
- File-echo case handling
- Version compatibility

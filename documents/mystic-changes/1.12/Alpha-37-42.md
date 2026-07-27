# Mystic 1.12 Alpha 37 through Alpha 42

This phase transformed MPY from a small scripting experiment into a practical Mystic integration language. Python gained user, group, file-base, message-base, prompt, screen, access, logging, and message-reader APIs. The same period introduced Unix-date MPL functions, major user-record changes, a new password engine, IPv6 and BinkP improvements, and additional server hardening.

## Alpha 37 — Major Python API Expansion

### MPY message-base reading

Python gained a message-base reader interface built around an opened message-base handle.

The initial function family included:

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

A default example named:

```text
msgread.mpy
```

implemented a basic message reader using the user's current message base.

### Message-reader lifecycle

A safe reader follows this structure:

1. Open the message base.
2. Check that a valid handle was returned.
3. Seek to a valid message or starting position.
4. Test whether the requested message exists.
5. Read the header.
6. Read the message text.
7. Move forward or backward as needed.
8. Close the message base even after an error.

A handle must not be reused after `msg_close`.

### Python user and record lookup

Python gained broader dictionary-based access to Mystic records. Functions added or expanded during this period included concepts such as:

```text
getuserid
getuser
getmbase
getmbaseid
getmgroup
getmgroupid
getfgroup
getfgroupid
getfbase
getfbaseid
```

Record-number lookup and unique-ID lookup are different. A record number can change after packing or reordering, while a stable ID is intended to identify the logical record.

### Python prompt and display functions

Python gained:

```text
showfile
getprompt
stuffkey
```

`showfile` displayed Mystic display files with options controlling rate, pause, and related behavior.

`getprompt(number)` returned the current theme's prompt. If the prompt had been replaced by a display file or script, Mystic could execute that replacement.

`stuffkey(text)` inserted text into Mystic's input buffer as though the user typed it.

### Python logging and access

Python gained:

```text
sysoplog
access
```

`sysoplog(level, text)` wrote to the node log using Mystic's configured log level.

`access(acsstring)` evaluated a Mystic ACS expression and returned a Boolean result.

Example concept:

```python
if bbs.access("s20"):
    bbs.writeln("Access granted")
```

### Python output and screen state

Python gained raw and screen-oriented functions including:

```text
rwrite
rwriteln
wherex
wherey
textattr
textcolor
gotoxy
charxy
mci2str
```

Raw output intentionally bypasses normal MCI or color processing. It should be used when the script needs literal output.

### Python user-profile update

A function such as:

```text
upuser(seclevel)
```

updated the current user according to the selected security profile, including associated limits and flags.

### Security impact

`upuser` is a privileged operation. A script must verify the caller's access before increasing a user's security profile.

`stuffkey` can automate input and must never insert untrusted data into a command path without validation.

### Mystic software changes

- Protocol display behavior for batch-only protocols was corrected.
- Additional Python examples and default scripts were provided to demonstrate the expanding API.

## Alpha 38 — MPL Runtime Crash Fix

### MPL changes

A defect that could cause MPL programs to crash was corrected.

### Upgrade action

Recompile and retest programs that previously crashed, especially those using:

- Arrays
- Nested records
- File I/O
- Classes
- Reference parameters
- Large compiled programs

Do not assume every crash was caused by the engine. Invalid indexes and file handles remain program errors.

## Alpha 39 — Unix Dates and User-Record Redesign

### MPL additions

MPL gained Unix-date conversion functions:

```text
DateUnix
DateU2D
DateD2U
```

Their purposes were:

- `DateUnix` — current date and time as a Unix timestamp
- `DateU2D` — convert a Unix timestamp to Mystic's DOS-style packed date
- `DateD2U` — convert a DOS-style packed date to a Unix timestamp

Example concept:

```pascal
UnixNow := DateUnix;
PackedDate := DateU2D(UnixNow);
```

### Mystic user-record changes

The user database changed substantially:

- First-on and last-on values moved to Unix timestamps.
- Expiration, last password change, and last email-validation dates used Julian day numbers.
- IP storage expanded for IPv6.
- Host storage expanded to 80 characters.
- Vote tracking expanded from 20 to 99 entries.
- Space was reserved for variable-iteration PBKDF2-HMAC-SHA512 password data.
- A country field was added.
- Local QWK and REP paths became separate fields.

### MPY impact

Python user dictionaries changed with the underlying record. Scripts must not assume old date formats or fixed field lengths.

### Compatibility impact

Programs reading user records directly from disk require a complete record-layout update. The safer approach is to use Mystic's MPL variables or MPY dictionaries.

## Alpha 40 — Password Engine, Server Timeouts, and User Security

### Mystic password changes

Mystic introduced configurable password storage modes:

- Cleartext, case-insensitive legacy mode
- Cleartext, case-sensitive mode
- PBKDF2 with SHA-512 hashing, case-sensitive

The PBKDF2 mode used a configurable iteration count. Higher values increase resistance to password guessing but also increase login and password-change time, particularly on older Raspberry Pi systems.

### Migration behavior

Existing cleartext passwords could be migrated when the user successfully authenticated or changed a password, depending on the configured upgrade path.

SysOps should test migration with copies of real user records before enabling hashing on production data.

### MPL password interfaces

New MPL interfaces accompanied the password engine.

Documented signatures were presented in forms such as:

```pascal
Procedure SetPW(PW : String);
```

and:

```pascal
Function ValidPW(PW : String) : Byte;
```

Some historical notes label the second item as a procedure while also showing a return type. Treat the compiler-accepted declaration as version-sensitive and verify it directly.

`SetPW` updates the currently loaded user's password using the configured storage method.

`ValidPW` checks a supplied password against the current user's stored password data and returns a status value.

### Security requirements

- Never compare password fields directly after hashing is enabled.
- Never log plaintext passwords.
- Never copy password fields between users.
- Use the official validation function.
- Use the official setter so salts, iterations, and formats are generated correctly.
- Back up `users.dat` before changing storage modes.

### Additional Mystic software changes

- Door command lines gained `%R` for a user name without underscores.
- Duplicate group-ID creation was corrected.
- Python `GotoXY` crash behavior was fixed.
- MUTIL base-creation functions gained a default ANSI-message option.
- FTP logged SysOp file deletion.
- FTP, NNTP, SMTP, and POP3 gained improved idle and shutdown behavior.

## Alpha 41 — SSH, Quote Dates, and Echomail Export

### Mystic software changes

- SSH behavior broken in Alpha 40 was corrected.
- Message quoting used a clearer date form such as `DD MMM YYYY`.
- Echomail export stopped sending a message back to its origin node.

### Scripting impact

Custom quote headers should use an unambiguous four-digit year.

Custom echomail routing must exclude the origin node to avoid loops and duplicate traffic.

## Alpha 42 — Compiler Reversion, BinkP Scale, IPv6, and Message Quote Time

### Mystic software changes

- Unix builds returned to Free Pascal 3.0.2 after later compiler behavior caused problems.
- BinkP's maximum queued files per session increased from 100 to 200.
- BinkP welcome data included build date, time, operating system, and bitness.
- After authentication, BinkP reported queue file count and total bytes before transfer.
- IPv6 server and client behavior became more consistent across Unix systems.
- Prompt 464 gained an MCI replacement for the original message time.
- Several server and message-network behaviors were corrected.

### Version sensitivity

Compiler changes can affect runtime behavior even when source does not change. Record the Free Pascal version used to build Mystic when investigating platform-specific crashes.

### BinkP monitoring impact

Status tools could report:

- Remote product and build
- Operating system and bitness
- Number of queued files
- Total queued bytes
- Address and authentication result

Do not treat remote product text as a security identity. Authentication must still rely on configured credentials and addresses.

## Alpha 37–42 Compatibility Checklist

1. Use the distributed `msgread.mpy` as the baseline for message-handle lifecycle.
2. Close every opened message base.
3. Prefer unique IDs over record numbers for persistent references.
4. Validate ACS before privileged Python actions.
5. Audit all uses of `stuffkey`.
6. Recompile MPL after Alpha 38.
7. Convert user-date handling to Unix and Julian formats as appropriate.
8. Update IPv6 and longer host-field assumptions.
9. Stop reading raw user records with old structures.
10. Back up users before enabling password migration.
11. Use `SetPW` and `ValidPW` rather than direct password-field access.
12. Tune PBKDF2 iterations for the actual server hardware.
13. Test SSH after every affected alpha.
14. Verify echomail never returns to the origin node.
15. Test BinkP queues larger than 100 files.
16. Record the compiler version used for Unix builds.

## Documentation Impact

This phase affects documentation for:

- MPY dictionaries
- MPY user, group, base, prompt, screen, access, and logging APIs
- MPY message reading
- MPL date functions
- User-record fields
- IPv6
- Password storage and migration
- Password-related MPL functions
- SSH
- BinkP monitoring
- Message quoting and routing
- Security practices

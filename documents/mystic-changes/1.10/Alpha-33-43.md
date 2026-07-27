# Mystic 1.10 Alpha 33 through Alpha 43

This phase expanded Mystic's network automation, message editing, theme system, BinkP support, QWK networking, AreaFix, FileFix, and MPL user-interface classes. It also delivered Linux and ARM work that broadened the platforms on which SysOps could deploy Mystic.

> **MPY status:** Embedded Python was not available in Mystic 1.10.

## Alpha 33 — MUTIL, FIDOPOLL, Message Operations, and ARM Linux

### Mystic software changes

- MUTIL and FIDOPOLL received corrections for network processing and polling.
- Message movement and private-reply behavior were improved.
- FTP passive-mode configuration was refined.
- Full-screen editor parameter handling was corrected.
- An Extended Reply access control setting was added or expanded.
- ARM Linux support appeared, extending Mystic beyond conventional x86 deployments.

### MPL impact

Scripts invoking FIDOPOLL, message commands, or editor actions should be tested with the revised command behavior. Platform-dependent code must not assume x86 path, executable, byte-order, or library conventions.

## Alpha 34 — Full-Screen Editor and International Keyboard Corrections

### Mystic software changes

- Quote and color handling in the full-screen editor was improved.
- Editor templates and color assignments were refined.
- International keyboard handling received fixes.

### MPL impact

MPL programs that launch the editor or inspect edited text should test:

- Quoted lines
- Pipe colors
- ANSI colors
- Extended characters
- Non-US keyboard layouts
- Returned cursor and screen state

## Alpha 35 — Theme, Sorting, Matrix, and Scan Improvements

### Mystic software changes

- File sorting became more consistent when names differed only by capitalization.
- Theme characters could be specified using numeric character values.
- QuickScan pause behavior was corrected.
- Matrix handling was fixed when a configured menu was missing.
- Theme choices were stored and restored more consistently.
- Additional theme and menu bugs were corrected.

### MPL impact

Theme-aware scripts should avoid hard-coding drawing characters and colors where a theme variable is available. File-sorting scripts should not depend on older case-sensitive ordering without testing.

## Alpha 36 — MPL Class Interface

### Mystic software changes

Theme memory leaks and related display problems were corrected.

### MPL additions

A class-style interface was introduced for more structured user-interface programming.

Core lifecycle concepts included:

```text
ClassCreate
ClassFree
```

Initial class areas included:

- ANSI box or window drawing
- Input handling
- ANSI image or screen handling

### Developer impact

The class interface allowed programs to create more reusable screen components instead of manually managing every coordinate and attribute.

Every created object must be released through the appropriate free operation. Failing to free class instances can leak memory or leave screen state inconsistent.

### Verification priorities

- Create and destroy each class repeatedly.
- Confirm no object remains allocated after a routine exits.
- Test clipping and coordinates at terminal boundaries.
- Test ANSI and non-ANSI users.
- Test abort and disconnect behavior while an object is active.

## Alpha 37 — User Options, BinkP, Logging, and QWK Networks

### Mystic software changes

- A user-options corruption problem was corrected.
- MIS gained built-in BinkP functionality.
- Server logging became more complete.
- QWK network accounts and configuration settings were expanded.

### MPL impact

Scripts reading or writing user-option records should be revalidated after the corruption fix. Network-status scripts could now account for BinkP and QWK services managed directly by Mystic.

## Alpha 38 — Packet Sorting and Message-Base Maintenance

### Mystic software changes

- Packet processing and sorting were corrected in network workflows.
- Message-base maintenance operations gained improved reset behavior.
- FIDOPOLL could target a single node more reliably.
- Event and semaphore processing continued to develop.

### MPL impact

Automation scripts should distinguish between:

- Polling one node
- Polling all configured nodes
- Processing a semaphore once
- Running a scheduled event repeatedly

A script should verify exit status and resulting semaphore state rather than assuming that launching the command completed all work.

## Alpha 39 — Events, Semaphores, Display, and Message Fixes

### Mystic software changes

- Event scheduling and semaphore processing were refined.
- Display and message-reader issues from earlier builds were corrected.
- Message network automation became more dependable.

### MPL impact

Event-launched MPL programs should log enough context to identify:

- Event name
- Start and finish time
- Node or process ID
- Input parameters
- Success or failure
- Semaphore files consumed or created

## Alpha 40 — Built-In AreaFix and QWK Networking

### Mystic software changes

- AreaFix functionality became available inside Mystic's network-processing tools.
- QWK networking continued to expand.
- Message area subscription and network-account behavior gained additional automation.

### MPL impact

Custom AreaFix-like MPL programs should be reviewed. Native functionality may be safer and more complete, while MPL remains useful for reporting, access policy, or site-specific wrappers.

## Alpha 41 — FileFix and File-Echo Linking

### Mystic software changes

- FileFix functionality was added or expanded.
- File-echo linking became more automated.
- Packet password handling was improved.
- MUTIL rescan statistics became more informative.

### MPL impact

File-network scripts gained more useful native operations to invoke and monitor. Programs should not display packet passwords or store them in logs.

### Security impact

Network passwords and linked-area permissions should be treated as secrets. Restrict file permissions on configuration, logs, and generated command files.

## Alpha 42 — CRAM, Domain, Polling, and QWK Fixes

### Mystic software changes

- ARGUS CRAM behavior was corrected for compatible secure authentication.
- FIDOPOLL semaphore creation and processing improved.
- Domain handling was refined.
- QWK and message-network defects were corrected.

### MPL impact

Domain-aware scripts should treat the domain as part of a network address rather than discarding it. A 5D address may distinguish otherwise identical numeric addresses belonging to separate networks.

## Alpha 43 — Twit Filtering and AKA Visibility

### Mystic software changes

- Twit-filtering support was added or expanded.
- Alternate network addresses could be hidden by domain where appropriate.
- Message-network presentation became more configurable.

### MPL impact

Message-display scripts should respect Mystic's filtering state and should not unintentionally reveal hidden AKAs or content that the configured filter excluded.

### Privacy impact

A custom reader or network report can bypass presentation-layer filtering if it directly accesses raw records. Such programs must deliberately enforce the site's privacy and visibility rules.

## Cross-Build MPL and Integration Changes

### Include syntax

During the wider 1.10 cycle, include syntax moved toward a direct statement form:

```pascal
Include common.mps
```

Projects should standardize one tested form and avoid mixing early-alpha directive syntax with later syntax.

### Message-base statistics

The message-base statistics interface and parameters evolved. Programs should record the exact function signature used with the target compiler.

### Network variables

MPL gained access to more network descriptions and addresses. Code should validate empty, local-only, 4D, and 5D values.

### User-interface classes

The new class interface was powerful but version-sensitive. Class methods, properties, and object cleanup should be documented alongside the exact Mystic build used for testing.

## Upgrade and Test Checklist

1. Test MUTIL imports, exports, scans, rescans, and mass uploads.
2. Test FIDOPOLL against one node and all nodes.
3. Test private replies and message movement.
4. Verify FTP passive-mode behavior through the local firewall and NAT path.
5. Test full-screen editing with quotes and international input.
6. Verify file sorting on case-sensitive and case-insensitive filesystems.
7. Test theme selection and persistence.
8. Stress-test MPL class creation and cleanup.
9. Validate user-option record reads and writes.
10. Test BinkP and QWK network accounts.
11. Verify event and semaphore automation.
12. Test AreaFix and FileFix permissions.
13. Confirm domain-aware address handling.
14. Verify twit filtering and hidden-AKA behavior in custom displays.

## Documentation Impact

This phase affects documentation for:

- Editors and terminal input
- Themes and ANSI display
- MPL classes
- User records
- BinkP
- QWK networking
- Events and semaphores
- MUTIL and FIDOPOLL
- AreaFix and FileFix
- File echoes
- Network passwords
- 5D addresses and domains
- Message filtering and privacy

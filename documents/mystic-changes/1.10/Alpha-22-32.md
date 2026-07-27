# Mystic 1.10 Alpha 22 through Alpha 32

This phase shifted much of the development focus from the initial MPL rewrite toward message handling, menus, terminal tools, QWK/QWKE networking, MUTIL, FIDOPOLL, and echomail processing. MPL continued to gain functions, variables, and integration points needed by these systems.

> **MPY status:** Embedded Python was not available in Mystic 1.10.

## Alpha 22 — Message-Base and Development Tracking Improvements

### Mystic software changes

- Message-base editing and sorting behavior was corrected and refined.
- Editor index handling received fixes to avoid incorrect selections or record movement.
- MIS node tracking was corrected to reduce ghost or stale node entries.
- The project began maintaining clearer `HISTORY` and `WHATSNEW` information.
- Installation tools gained better access to release-history information.

### MPL impact

No major syntax migration was introduced in this build, but script developers benefited from more stable message-base and node state. Scripts that enumerated message bases or displayed node activity should be retested because corrected underlying data can change their output.

## Alpha 23 — Prompt, ANSI, and Numeric Function Additions

### Mystic software changes

- The message-base editor gained a network-related column to make network assignments easier to review.
- Insert and editor operations received corrections.
- NodeSpy gained improved scrollback and searching.
- Prompt simulation and testing improved, helping SysOps preview prompt behavior.
- ANSI viewing behavior was expanded and corrected.

### MPL changes

A numeric absolute-value function was added:

```pascal
Value := Abs(Value);
```

Programs using the newly added function needed recompilation with the matching MPLC build.

### Compatibility impact

Prompt and ANSI-related MPL programs should be tested against both the user terminal and local NodeSpy view because rendering and scrollback handling were changing.

## Alpha 24 — Telnet and Zmodem Stability

### Mystic software changes

- Telnet handling received fixes for connection and terminal behavior.
- Built-in Zmodem transfer handling continued to mature.
- NodeSpy gained a disconnect capability for managing active nodes.
- Transfer state and terminal interaction were corrected in several edge cases.

### MPL impact

Transfer-launching or connection-management scripts should not assume that older timing workarounds remain necessary. Test any MPL wrappers around upload, download, or node-control actions.

## Alpha 25 — Archive Viewing and Transfer Compatibility

### Mystic software changes

- Mystic could inspect archives contained inside another archive in supported viewing contexts.
- ZEDZAP and transfer-related behavior improved.
- Synchronet terminal compatibility received fixes.
- The standard message reader gained a more direct way to jump to a selected message.

### MPL impact

Scripts that invoke archive viewers, transfer commands, or message-reader actions should be checked for changed command behavior and improved built-in capabilities. Custom code may be replaceable with native Mystic functions.

## Alpha 26 — Menus, Smart Input, and File Tagging

### Mystic software changes

- Generated menu output handled zero-column settings more safely.
- A `-G`-related execution option was added or expanded for menu or startup behavior.
- Lightbar and prompt-driven menu handling improved.
- Smart input behavior was introduced or expanded, providing more capable command entry.
- File-tagging logic was substantially rewritten.

### MPL impact

Menu-oriented MPL programs needed testing around:

- Stuffed keyboard input
- Prompt positioning
- Lightbar interaction
- File selection and tagging
- Return behavior after a menu command

Programs that duplicated file-tagging logic could increasingly rely on Mystic's native implementation.

## Alpha 27 — Message Prompts, Offline Access, and Newscan Changes

### Mystic software changes

This build continued message and menu refinement, including areas such as:

- Behavior when no messages are available
- Disconnect handling after downloads
- Access checks while operating in offline or nonstandard contexts
- Newscan controls and the ability to ignore or remove selected areas from scans
- Nodelist and network navigation features

### MPL impact

Scripts that launch message scans or make access decisions should verify whether Mystic already filtered unavailable areas before the MPL program runs. Avoid applying a second, conflicting access filter unless it is intentional.

## Alpha 28 — Nodelist and QWK Statistics Expansion

### Mystic software changes

- Nodelist browsing and lookup behavior expanded.
- QWK and offline-mail statistics became more visible.
- Message scanning and network-area presentation continued to improve.
- Display and navigation bugs from earlier menu work were corrected.

### MPL impact

Network and statistics screens could access more complete built-in information. Any custom nodelist or QWK status display should be reviewed for newer native commands and data fields.

## Alpha 29 — QWK Email and QWKE Support

### Mystic software changes

- QWK email support expanded.
- QWKE extensions were added or improved.
- Offline-mail packet handling gained additional compatibility and metadata support.
- Message options and packet-processing behavior continued to evolve.

### MPL integration changes

Startup MPL gained the ability to influence the login path through variables representing the login name and password. A startup program could set these values to bypass or replace part of the built-in login sequence when deliberately configured.

Conceptual use:

```pascal
UserLoginName := SuppliedName;
UserLoginPW := SuppliedPassword;
```

### Security impact

A startup script that sets login credentials becomes part of the authentication boundary. It must validate input carefully, avoid logging passwords, and fail closed when validation is incomplete.

## Alpha 30 — Message-Base Statistics and Mail State

### Mystic software changes

- Message options and status tracking were refined.
- Received/read-state handling was corrected or expanded.
- Message-base statistics were made more useful to scripts and internal displays.

### MPL changes

The message-base statistics interface changed. The older naming and parameter arrangement around `GetMBaseStats` was revised toward `GetMBStats` or an equivalent shortened form with updated parameters.

Because the signature changed during development, source must be checked against the target MPLC documentation rather than relying on an older example.

### Compatibility impact

A statistics call may still compile if arguments are type-compatible but produce incorrect values when their meaning or order changed. Validate each returned field at runtime.

## Alpha 31 — MUTIL, Netmail, and Quoting Improvements

### Mystic software changes

- MUTIL mass-upload processing gained additional exclusion controls.
- Netmail AKA matching improved so Mystic could select addresses more accurately.
- Message quoting and display behavior received corrections.
- Echomail processing moved toward stronger 5D address support.
- Routing behavior became more configurable.

### MPL impact

Scripts that inspect network addresses should account for domain-aware and 5D forms rather than comparing only zone, net, node, and point numerically.

## Alpha 32 — Echomail Export, Mail Statistics, and Network Variables

### Mystic software changes

- Echomail importing and exporting expanded around 5D addressing.
- Netmail and echomail routing rules were refined.
- MUTIL gained more complete processing and reporting.
- Network configuration became more visible in editors and runtime records.

### MPL additions

Mail-statistics access expanded through a function such as:

```text
GetMailStats
```

Configuration and message-base network variables were added or expanded, including concepts represented by names such as:

```text
CfgNetDesc
MBaseNetAddr
```

### Developer impact

MPL programs could create more informative network dashboards and message-base summaries. Code should not assume that every base has a network address or that every configured network uses the same address format.

## Cross-Build Compatibility Notes

### Message records

Message flags, received state, quoting behavior, and network kludges changed repeatedly during this phase. Programs that read or modify message records should be tested on copies of real data.

### QWK and QWKE

Offline-mail support became more capable across several consecutive builds. Avoid documenting one alpha's packet behavior as universal for all of Mystic 1.10.

### Login scripting

The ability for startup MPL to supply login values is powerful but security-sensitive. Document exactly which startup file is used, where it is stored, and what happens when it returns invalid credentials.

### Statistics calls

Message-base and mail-statistics interfaces changed during development. Record the exact compiler build and parameter meanings in every verified example.

## Upgrade and Test Checklist

1. Test message-base sorting and enumeration.
2. Verify NodeSpy and node-status scripts.
3. Test ANSI output in Mystic, NodeSpy, and common terminals.
4. Test built-in Zmodem and any script wrappers.
5. Validate archive viewing and nested-archive handling.
6. Retest file-tagging workflows.
7. Review newscan and offline access behavior.
8. Test QWK/QWKE packet generation and import.
9. Audit startup MPL authentication logic.
10. Revalidate message-base and mail-statistics calls.
11. Confirm 4D and 5D address formatting.
12. Back up message and network data before testing write operations.

## Documentation Impact

This phase affects documentation for:

- Menu integration
- Prompt handling
- ANSI and screen output
- File transfers
- File tagging
- Message bases
- QWK and QWKE
- Startup programs and authentication
- Message statistics
- Network addresses
- MUTIL and FIDOPOLL integration

# Mystic 1.10 Alpha 44 through Alpha 63 and Final Release

The final phase of Mystic 1.10 concentrated on message-network correctness, BinkP and FTP reliability, filtering, event automation, node chat, display viewers, file-description handling, prompt internals, and release stabilization.

> **MPY status:** Embedded Python was not available in Mystic 1.10.

## Alpha 44 — Message-Network Filtering Corrections

### Mystic software changes

- Twit filtering was corrected so the configured rules were actually applied in the intended message paths.
- Tear line, origin line, `PATH`, and `SEEN-BY` handling received corrections.
- Network-message presentation and export became more consistent.

### MPL impact

A custom message reader or exporter should not reconstruct network lines unless it fully understands the required format. Scripts should distinguish between visible message text and network-control information.

## Alpha 45 — Network Processing Stabilization

### Mystic software changes

This build continued correcting echomail, netmail, and packet-processing issues discovered after the previous network changes.

Focus areas included:

- Packet validation
- Address and route handling
- Message-line generation
- Import/export consistency
- Duplicate or malformed network information

### MPL impact

Scripts that alter message text before export should be retested. A harmless-looking edit can invalidate tear, origin, path, or seen-by data if the script writes the complete message record back incorrectly.

## Alpha 46 — BinkP and Transfer Refinement

### Mystic software changes

BinkP session handling, file transfer, and connection state continued to be refined. Interoperability with external mailers remained a central test area.

### MPL impact

Network-monitoring scripts should report the remote address, session direction, negotiated options, transfer result, and disconnect reason when those values are available.

## Alpha 47 — FTP and BinkP Fixes

### Mystic software changes

- FTP defects were corrected.
- BinkP transfer and session behavior improved.
- Network-service stability was strengthened before the final release.

### MPL impact

Scripts that launch or monitor FTP and BinkP services should avoid relying on exact log wording. Parse stable fields or structured state where possible.

## Alpha 48 — QWK File Visibility and Network Corrections

### Mystic software changes

- QWK-related files were made visible through FTP only where the configured access and account rules allowed them.
- Network-account and packet handling received additional corrections.

### Security impact

Offline-mail packets can contain private messages and user-specific data. FTP visibility must be protected by the same access expectations as direct QWK download inside Mystic.

## Alpha 49 — ICE Colors and Extended Pipe Colors

### Mystic software changes

- ICE color behavior was expanded or corrected.
- Additional pipe-color values, including the higher-intensity range represented by values 24 through 31, became available in supported display contexts.

### MPL impact

ANSI and pipe-color programs could use a broader color range, but output should be tested with terminals that do not implement ICE colors identically.

### Compatibility impact

Blink and high-intensity background behavior can conflict because some terminals interpret the same attribute bit differently.

## Alpha 50 — Automatic Banning and Server Protection

### Mystic software changes

Automatic banning behavior was introduced or expanded for network-service abuse and repeated invalid activity.

### MPL impact

Authentication and connection scripts should avoid generating repeated failed attempts during testing. Administrative reports should distinguish between:

- Temporary automatic bans
- Permanent bans
- Address-based bans
- User-account locks
- Access-control failures

## Alpha 51 — Linux Stability Corrections

### Mystic software changes

Linux-specific defects were corrected across terminal, filesystem, server, or process behavior.

### MPL impact

Programs should use configured paths and path-joining functions rather than embedding Windows separators. Case-sensitive filename behavior must be tested on Linux.

## Alpha 52 — Area Indexing and Message Posting

### Mystic software changes

- Area-index behavior was refined.
- Posting and message-area selection received corrections.
- Message grouping and navigation improved.

### MPL impact

Programs that display a message-area index should validate the current group, current base, access result, and whether the area allows posting before presenting an action.

## Alpha 53 — Paragraph Reformatting and Grouping

### Mystic software changes

- Paragraph reformatting improved in message editing.
- Message and area grouping behavior was refined.
- Additional editor and display inconsistencies were corrected.

### MPL impact

Scripts that post-process editor output should preserve intentional hard line breaks, quoting prefixes, tear lines, and origin lines.

## Alpha 54 — Group Membership Corrections

### Mystic software changes

Group-membership handling was corrected so access decisions and group assignment more accurately reflected the user's configured membership.

### MPL impact

Programs using group access should retest:

- Primary group
- Additional group membership
- Hidden groups
- Message groups
- File groups
- SysOp overrides
- Users with no explicit membership

## Alpha 55 — Locking, Menus, and Group Ordering

### Mystic software changes

- Record or file locking behavior improved.
- Menu handling received corrections.
- Group ordering became more consistent.

### MPL impact

MPL programs that write shared data should check I/O results and should not hold a record or file open longer than necessary. Concurrent nodes can otherwise conflict.

## Alpha 56 — BinkP 1.0 and 1.1, Node Chat Commands

### Mystic software changes

- BinkP protocol handling expanded across version 1.0 and 1.1 capabilities.
- Node-chat commands were added or expanded.
- Interactive inter-node communication became more capable.

### MPL impact

Node-management scripts could integrate more naturally with built-in chat and network state. Avoid sending raw control sequences unless the exact node-chat protocol is documented and tested.

## Alpha 57 — MPL Runtime Crash Correction

### MPL changes

A crash affecting MPL execution was corrected.

### Upgrade action

Recompile and rerun scripts that previously crashed, but do not assume the corrected engine makes invalid source safe. Confirm that arrays, files, records, classes, and reference parameters remain within valid bounds.

## Alpha 58 — Node-Chat Control Codes

### Mystic software changes

Node-chat display and control codes expanded or were corrected, improving interactive chat presentation and command handling.

### MPL impact

A custom chat display should either use Mystic's supported output functions or explicitly filter control codes. Raw chat data may contain terminal instructions that should not be copied into logs unescaped.

## Alpha 59 — Release-Candidate Maintenance

### Mystic software changes

This build continued stabilization across servers, messages, files, terminals, and configuration tools.

### MPL impact

No major language redesign was introduced at this stage. The priority was regression testing of all previously added compiler and runtime features.

## Alpha 60 — Pre-Release Corrections

### Mystic software changes

Additional release-blocking defects were corrected. Network interoperability, terminal output, and data-file safety remained important test areas.

### Upgrade action

Use production-like data copies for testing. Empty test databases do not expose record-conversion, indexing, or network-routing problems that appear only with established systems.

## Alpha 61 — Inline File Descriptions, Viewers, and Hourly Events

### Mystic software changes

- Inline file-description display expanded, including DIZ-related presentation.
- ANSI and text viewers improved.
- Hourly event scheduling became available or more complete.

### MPL impact

Programs displaying file descriptions should account for:

- Plain text descriptions
- ANSI descriptions
- Embedded pipe colors
- SAUCE or metadata trailers where present
- Terminal width and height

Event-launched programs should avoid overlapping runs. Use locks, semaphores, or a recorded running state when an hourly job can take longer than one interval.

## Alpha 62 — Prompt Internals and Baud Emulation

### Mystic software changes

- Prompt values and internal prompt handling were refactored.
- Direct-inquiry or display behavior gained baud-rate emulation options.
- Prompt and terminal timing became more flexible.

### MPL impact

Programs that depend on prompt variables, cursor position, or timing should be retested. Baud emulation intentionally delays output and can expose assumptions about immediate screen completion.

## Alpha 63 — MPL Error Visibility and File Buffering

### Mystic software changes

- MPL execution errors remained visible briefly instead of disappearing immediately behind subsequent screen output.
- File buffering improved.
- ANSI file-description display received additional corrections.

### Developer impact

The error pause made interactive debugging easier, but production scripts should still log enough information to identify the program, parameters, node, and failing operation.

Improved buffering can change timing and when data is physically written. Close files explicitly and check `IoResult` before treating an operation as successful.

## Final Mystic 1.10 Release — February 20, 2015

### Release stabilization

The final release consolidated sixty-three alpha builds and included final corrections such as:

- NNTP behavior fixes
- MUTIL file-creation or date-handling corrections
- Network and message-processing stabilization
- Final terminal, editor, file, and server fixes

### MPL state at release

By the final release, the modern MPL foundation included:

- `.mps` source compiled to `.mpx`
- Pascal-style blocks and control flow
- Standard expression precedence
- Local variables and initialized declarations
- Nested routines and recursive procedures
- `Var` parameters
- Sized strings and string indexing
- Character codes
- Multidimensional arrays
- Expanded records and file I/O
- `Case`, `Break`, and `Continue`
- A class-style user-interface interface
- Many renamed and newly added built-ins
- Program and configuration context variables
- Improved compiler limits and execution speed

### MPY state at release

Mystic 1.10 did not include the embedded Python engine. `.mpy` scripts belong to the later 1.12 development history.

## Final 1.10 Upgrade Checklist

1. Back up configuration, data, menus, themes, scripts, and network queues.
2. Preserve all `.mps`, include, and legacy `.mpe` files.
3. Convert source to the modern parser rules.
4. Search for renamed and removed identifiers.
5. Review mathematical and Boolean expressions.
6. Migrate file I/O and error handling.
7. Recompile every program with the final 1.10 MPLC.
8. Update all execution paths from `.mpe` to `.mpx`.
9. Test ANSI, ICE colors, raw output, and terminal timing.
10. Test local console, telnet, FTP, BinkP, QWK, QWKE, NNTP, and transfer paths used by the system.
11. Test MUTIL, FIDOPOLL, AreaFix, and FileFix with copied network queues.
12. Validate user and group access.
13. Test hourly and semaphore events for overlap.
14. Verify file descriptions and ANSI viewers.
15. Compare message-network output with a known-good mailer.

## Documentation Impact

The final 1.10 history should be reflected in documentation for:

- MPL syntax and compilation
- Operators and expressions
- Variables, procedures, functions, arrays, and records
- Conditional logic and loops
- File handling and I/O errors
- ANSI, colors, and screen output
- Menus, prompts, themes, and classes
- User and group data
- Message and file bases
- QWK/QWKE, echomail, netmail, BinkP, AreaFix, and FileFix
- Events and semaphores
- Version compatibility and migration

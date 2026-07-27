# Mystic BBS Changes and Updates

This folder provides a structured reference to Mystic BBS release-history and `WHATSNEW.TXT` information.

The files in this folder are intended to support the MPL documentation project by recording:

- Mystic version changes
- MPL language additions and removals
- MPL compiler behavior changes
- New built-in functions and procedures
- Renamed or removed MPL symbols
- Data-record and runtime changes that affect MPL programs
- Version-specific compatibility notes
- Documentation pages that may need revision

These files summarize and organize upstream information. They are not intended to replace the original Mystic release notes or `WHATSNEW.TXT` files.

## Change Markers

Mystic release notes traditionally use these markers:

| Marker | Meaning |
|---|---|
| `+` | New or changed feature |
| `!` | Bug fix |
| `-` | Removed feature |

## Version Files

- [Mystic 1.05](Mystic-1.05.md)
- [Mystic 1.06](Mystic-1.06.md)
- [Mystic 1.07](Mystic-1.07.md)
- [Mystic 1.08](Mystic-1.08.md)
- [Mystic 1.09](Mystic-1.09.md)
- [Mystic 1.10](Mystic-1.10.md)
- [Mystic 1.11](Mystic-1.11.md)
- [Mystic 1.12](Mystic-1.12.md)

## MPL Cross-Version Index

See [MPL Change Index](MPL-Change-Index.md) for changes grouped by MPL topic rather than Mystic version.

## Adding New Entries

Use [Entry Template](ENTRY-TEMPLATE.md) when documenting another version, alpha release, or individual change.

Each recorded change should identify:

1. Mystic version or alpha build
2. Change type
3. Upstream wording summarized in original language
4. MPL or documentation impact
5. Verification status
6. Related wiki pages
7. Official source

## Verification Status

Use one of these values:

| Status | Meaning |
|---|---|
| `Source indexed` | Official source has been linked, but details are not fully extracted |
| `Summarized` | Important changes have been summarized |
| `Compiler tested` | Relevant MPL syntax compiled successfully |
| `Runtime tested` | Compiled behavior was tested inside Mystic |
| `Version specific` | Behavior is confirmed only for a recorded version |
| `Needs review` | Information is incomplete or conflicting |

## Primary Sources

- Mystic BBS Wiki history index: https://wiki.mysticbbs.com/doku.php?id=whats_new_intro
- Mystic BBS Wiki: https://wiki.mysticbbs.com/
- Mystic release archives and their included `WHATSNEW.TXT` files
- Mystic source and historical material: https://github.com/fidosoft/mysticbbs

## Copyright and Attribution

Do not copy complete upstream release notes into this repository. Summarize relevant information and link to the official source. Short excerpts should be used only when necessary to preserve an exact identifier, command, or diagnostic message.

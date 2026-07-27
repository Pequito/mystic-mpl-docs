# Mystic 1.10 Detailed Change History

Mystic 1.10 was a long development cycle that modernized the BBS software and substantially redesigned Mystic Programming Language.

This directory expands the version history by alpha-build range so that software-wide, MPL, compatibility, and upgrade changes can be documented without compressing sixty-three alpha builds into one short page.

## Build Range Files

- [Alpha 1 through Alpha 21](Alpha-01-21.md)
- [Alpha 22 through Alpha 32](Alpha-22-32.md)
- [Alpha 33 through Alpha 43](Alpha-33-43.md)
- [Alpha 44 through Alpha 63 and final release](Alpha-44-63-and-release.md)

## Major Themes

### Mystic software

The 1.10 cycle expanded Mystic from a primarily dial-up-era BBS package into a broader Internet-connected platform. Work included:

- Rewritten and expanded server support
- BinkP, FTP, QWK, QWKE, echomail, netmail, AreaFix, and FileFix improvements
- New menu, theme, prompt, ANSI, editor, message, file, and node-management behavior
- MUTIL and FIDOPOLL development
- Better Linux, Windows, ARM, terminal, and transfer support
- User-to-user chat and improved node messaging
- Large configuration-record and data-format changes

### MPL

The MPL language moved toward a modern Pascal-style structure and gained:

- `Then`, `Do`, and `Begin`/`End` control-flow syntax
- Standard mathematical precedence and parentheses
- Local variables and declaration initialization
- Nested procedures and functions
- Recursive procedures
- `Var` reference parameters
- `Case`, `Break`, and `Continue`
- Sized strings, character references, and string indexing
- Improved arrays, records, file I/O, compiler limits, and execution speed
- A C-like alternate syntax mode
- Many new or renamed built-in variables, procedures, and functions
- The `.mpx` compiled-program extension

### MPY and Python

Embedded Python was not available in Mystic 1.10. MPY scripting was introduced later during the Mystic 1.12 development cycle.

## Migration Importance

Mystic 1.10 is one of the most important compatibility boundaries in Mystic scripting history. Existing source may require edits before recompilation because syntax, operator evaluation, built-in identifiers, file handling, record access, and the compiled extension changed.

Review the alpha-range files before moving an older MPE/MPL project to 1.10 or later.

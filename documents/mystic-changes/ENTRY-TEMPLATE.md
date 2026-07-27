# Mystic <Version> Changes

Status: **Documented / Needs testing**

## Release Summary

Explain the purpose and overall direction of this Mystic release in original language. Include release or alpha-build dates when known.

## Mystic Software Changes

### Added

- Describe new BBS, server, editor, menu, message, file, protocol, configuration, or utility features.
- Explain how a SysOp would use each important feature.

### Changed

- Describe altered defaults, workflows, file formats, paths, menu behavior, prompts, record layouts, or platform support.

### Fixed

- Summarize defects whose correction materially changes operation or compatibility.

### Removed

- Identify removed commands, protocols, programs, settings, or compatibility paths.

## MPL Changes

### Language and Compiler

- New syntax
- Changed syntax
- Compiler validation changes
- Expression and type-system changes
- Source or compiled extension changes

### Runtime

- Execution-engine changes
- Performance changes
- Error handling
- Menu and command-line execution changes

### Functions, Procedures, and Variables

| Name | Type | Change | Developer impact |
|---|---|---|---|
| `<identifier>` | Function, procedure, or variable | Added, changed, renamed, or removed | Explain required source change |

### MPL Compatibility Impact

Explain whether existing source must be edited or only recompiled.

## MPY and Python Changes

State one of the following clearly:

- **MPY was not available in this release.**
- **MPY was introduced or changed in this release.**

When MPY exists, document:

- Python engine version
- `.mpy` execution methods
- New or changed Mystic Python functions
- Dictionary or record changes
- Python 2 versus Python 3 behavior
- Required script migration
- Platform-specific behavior

## Upgrade and Migration Actions

1. Back up the Mystic installation and script source.
2. Identify configuration or record changes.
3. Update renamed or removed MPL and MPY identifiers.
4. Recompile affected `.mps` source with the target MPLC.
5. Test `.mpx` and `.mpy` scripts inside Mystic.
6. Validate menu, message, file, and server behavior affected by the release.

## Documentation Impact

List pages in this repository that should be reviewed or updated.

- `wiki/<Page>.md`
- `documents/mystic-changes/MPL-Change-Index.md`

## Verification Record

```text
Mystic version/build:
Operating system:
Architecture:
MPLC version:
Python engine:
Date tested:
Tested by:
MPL compile result:
MPL runtime result:
MPY runtime result:
Software feature result:
Notes:
```

## Known Uncertainties

List behavior that remains untested, platform-specific, or inconsistent among alpha builds.

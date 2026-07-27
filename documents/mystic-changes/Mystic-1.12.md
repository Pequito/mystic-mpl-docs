# Mystic 1.12 Changes

Status: **Documented overview — alpha-build verification required**

## Release Summary

Mystic 1.12 has a long alpha-development history and includes extensive changes to Mystic itself, MPL, and the introduction of embedded Python through `.mpy` scripts.

Because features were added, replaced, and revised across many alpha builds, documentation must always record the exact 1.12 alpha version being tested.

## Mystic Software Changes

Mystic 1.12 continued broad development across:

- Telnet, SSH, and other server services
- Message networking and message-base processing
- File bases, archive handling, and file utilities
- Configuration and maintenance tools
- Menu commands and event execution
- User records and security
- ANSI, terminal, and screen handling
- Linux, Windows, macOS, and architecture support
- Command-line execution and automation

Individual build changes should be added under dated alpha-build headings as they are verified.

## MPL Changes

### Continued Language Expansion

The 1.12 development cycle continued improving modern MPL syntax, type handling, arrays, records, runtime interfaces, compiler diagnostics, and BBS-data access.

Complex features should be tested against the exact alpha build because a construct accepted by one compiler may behave differently in another.

### Command-Line Execution

Mystic supports command-line execution of MPL programs with the `-X` option. During 1.12 development, argument handling and related execution behavior were adjusted.

Automation should test:

- Script path resolution
- Parameter forwarding
- Exit status
- Working directory
- Error output
- Behavior when the script is outside the default script directory

### Records, Arrays, and Data Access

MPL interfaces for Mystic records continued to evolve. Scripts that read or write users, message bases, file bases, groups, configuration, or other BBS data should be considered version-sensitive.

Before deploying a record-writing script:

1. Back up the affected data.
2. Confirm the record was loaded successfully.
3. Modify only intended fields.
4. Save with the correct procedure.
5. Read the record again and verify the result.

### Compiler and Runtime Corrections

The 1.12 alpha cycle includes many fixes where the compiler accepted invalid source, rejected valid source, or the runtime handled a valid construct incorrectly.

For that reason, a wiki example should not be marked verified merely because it compiles with one 1.12 alpha build.

## Embedded Python and MPY

### MPY Introduction

Mystic 1.12 introduced embedded Python scripting. Python source files use the `.mpy` extension and execute inside Mystic with access to Mystic-provided functions and data dictionaries.

MPY scripts can be used for many of the same integration tasks as MPL, including:

- Terminal input and output
- User and session information
- Menu integration
- Message and file data access
- Configuration access
- File-system operations
- Script parameters

### Original Python 2.7 Engine

The first embedded Python implementation used Python 2.7. Scripts written for this engine use Python 2 syntax and behavior.

Typical Python 2 assumptions include:

```python
print 'Hello from Mystic'
```

Python 2 text, byte, integer-division, dictionary, and module behavior differs from Python 3.

### Later Python 2 and Python 3 Engine

Later 1.12 alpha development replaced the original Python engine with a newer implementation capable of loading Python 2 or Python 3, depending on the installed shared library and the Mystic execution method.

This introduced Python 3 execution paths and made the runtime version an essential part of MPY documentation.

Python 3 syntax uses:

```python
print('Hello from Mystic')
```

An MPY script written for the original Python 2.7 engine should not be assumed to run unchanged with Python 3.

### MPY Compatibility Areas

Review these areas during Python 2 to Python 3 migration:

- `print` syntax
- `str` versus `bytes`
- Integer division
- Unicode handling
- Dictionary keys and iteration
- Exception syntax
- Renamed standard-library modules
- Shared-library naming and loading
- Mystic function return types
- Mystic dictionary value types

### Mystic Python Functions and Dictionaries

Mystic exposes its own Python API rather than requiring scripts to communicate with Mystic through external files alone.

The API includes categories such as:

- Display and terminal functions
- Keyboard and input functions
- File and directory functions
- User data functions
- Message-base functions
- File-base functions
- Configuration functions
- Menu and session functions

Mystic also exposes structured BBS information through dictionaries. Dictionary names, keys, value types, and write behavior can change among alpha builds, so each reference page must identify the tested build.

### MPY Execution Context

An MPY script may behave differently depending on where it is launched:

- Interactive menu command
- Login or logoff event
- Command line
- Server or node context
- Background or maintenance process

Test the script using the same execution path intended for production.

## MPL and MPY Comparison

| Area | MPL | MPY |
|---|---|---|
| Source extension | `.mps` | `.mpy` |
| Compiled output | `.mpx` | Normally interpreted by embedded Python |
| Language style | Pascal-influenced | Python 2 or Python 3, depending on engine |
| Compiler required | Yes, MPLC | No MPL compilation step |
| Mystic integration | Built-in functions, procedures, and variables | Built-in Python functions and dictionaries |
| Version sensitivity | Compiler and runtime build | Mystic build and Python runtime version |

Neither language is automatically superior. MPL is tightly integrated and deploys as compiled `.mpx`; MPY offers Python syntax and libraries but requires careful runtime-version management.

## Compatibility Impact

Mystic 1.12 is highly version-sensitive because it spans many alpha releases.

Record all of the following for an MPY test:

```text
Mystic alpha build:
Operating system:
Architecture:
Python major version:
Python library version:
Library path:
MPY execution method:
```

For MPL, record the exact MPLC build and recompile whenever Mystic is upgraded.

## Upgrade Actions

1. Back up the complete Mystic installation and script source.
2. Record the current and target alpha builds.
3. Recompile all MPL source with the target MPLC.
4. Test record-reading and record-writing MPL programs.
5. Identify whether each MPY script targets Python 2 or Python 3.
6. Confirm that Mystic loads the intended Python shared library.
7. Test text, bytes, dictionaries, exceptions, and division behavior.
8. Test scripts from their production menu or event context.
9. Do not remove the previous working build until both MPL and MPY tests pass.

## Documentation Impact

- `wiki/MPL-Syntax.md`
- `wiki/Variables.md`
- `wiki/Procedures.md`
- `wiki/Functions.md`
- `wiki/Compiler-Usage.md`
- Future `wiki/Embedded-Python.md`
- Future `wiki/MPY-Getting-Started.md`
- Future `wiki/MPY-Functions.md`
- Future `wiki/MPY-Dictionaries.md`
- `wiki/Version-Compatibility.md`

## Recommended Alpha-Build Entry Format

```markdown
## Mystic 1.12 Axx

### Mystic Software

- Explain software changes.

### MPL

- Explain compiler, language, or runtime changes.

### MPY

- Explain Python engine, function, dictionary, or execution changes.

### Compatibility

- Explain required migration and testing.
```

## Verification Record

```text
Mystic version/build: 1.12 A__
Operating system:
Architecture:
MPLC version:
Python major version:
Python library version:
MPL compile result:
MPL runtime result:
MPY import result:
MPY runtime result:
Record read/write result:
Menu execution result:
Command-line execution result:
Notes:
```

## Known Uncertainties

The following require build-specific documentation:

- Exact alpha build where embedded Python first appeared
- Exact alpha build where the replacement Python 2/3 engine appeared
- Python shared-library naming by platform
- Which Mystic functions are available in Python 2 versus Python 3 mode
- Dictionary key additions, removals, and type changes
- MPY command-line argument behavior
- MPL record and array differences among alpha builds
- 32-bit versus 64-bit behavior

These details should be added directly to this file as they are tested rather than requiring the reader to consult another wiki.

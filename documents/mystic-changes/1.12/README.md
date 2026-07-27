# Mystic 1.12 Detailed Change History

Mystic 1.12 had a long alpha cycle spanning forty-eight documented alpha builds. It introduced embedded Python scripting, continued major MPL compiler and runtime work, replaced central server and theme infrastructure, changed user and password records, and delivered extensive message, file, network, terminal, security, and automation improvements.

Because of the volume and the long time span, the detailed history is divided into chronological build ranges.

## Build Range Files

- [Alpha 1 through Alpha 12](Alpha-01-12.md)
- [Alpha 13 through Alpha 24](Alpha-13-24.md)
- [Alpha 25 through Alpha 36](Alpha-25-36.md)
- [Alpha 37 through Alpha 42](Alpha-37-42.md)
- [Alpha 43 through Alpha 48](Alpha-43-48.md)

## Major Mystic Software Themes

- Embedded Python scripting
- New and later replaced MIS server implementations
- Socket, SSH, FTP, BinkP, NNTP, SMTP, POP3, and IPv6 work
- QWK, QWK networking, echomail, netmail, TIC, and file-echo corrections
- New menu IDs and expanded menu capacities
- Spell checking
- New file indexes and duplicate indexes
- User database and password-engine changes
- Theme-system replacement
- Message-reader, editor, ANSI-message, draft, quote, and tagline improvements
- Security, automatic banning, TLS, whitelisting, and locked-account handling
- New login-event scripts

## Major MPL Themes

- New buffered and record-size-aware file I/O
- Faster in-memory MPX execution
- Increased compiled-program size
- More complete nested records and arrays inside records
- Expanded compiler command-line options
- Improved include and library handling
- Better compiler error summaries
- Unix-date conversion functions
- User-statistics and password-related interfaces
- Theme configuration variables
- Masked-input and Input-class changes
- New login-event execution points

## Major MPY and Python Themes

- Initial embedded Python 2.7 support
- `.mpy` script lookup and menu execution
- Prompt execution of Python scripts
- Gradual addition of terminal, input, user, configuration, message, file-base, and message-base APIs
- User and message-base dictionaries
- Message-reader APIs and a Python message-reader example
- Theme fallback configuration variables
- Replacement Python engine capable of loading Python 2 or Python 3
- Separate Python 3 menu, command-line, Mystic-DOS, and prompt execution paths
- Display-file discovery functions
- More robust Python loading and user-statistics access

## Version-Sensitive Warning

Mystic 1.12 alpha behavior changed repeatedly. A function or data field documented for one alpha should not automatically be assumed to exist or behave identically in every other alpha.

Every verified example should record:

```text
Mystic 1.12 alpha build:
Operating system:
Architecture and bitness:
MPLC version:
Python major version:
Python library path:
Date tested:
Result:
```

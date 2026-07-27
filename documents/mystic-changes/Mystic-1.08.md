# Mystic 1.08 Changes

Status: **Documented — needs local verification**

## Release Summary

Mystic 1.08 resumed public development after the 1.07 series and focused on modern Internet-facing BBS operation, server security, message-network reliability, configuration improvements, and continued expansion of MPL access to Mystic runtime information.

## Mystic Software Changes

### Telnet Security

Mystic's Telnet server gained IP blocking through an `IPBLOCK.TXT` file in the data directory. Entries could block exact addresses or address masks. This gave SysOps a native way to reject known abusive hosts before login.

### Message Handling

Message-reader and network-message defects were corrected, including filtering of kludge lines and creation of reply identifiers. These fixes improved compatibility for networked message bases.

### Configuration Reliability

MCFG defects involving menu editing and clipboard-style operations were corrected. The release also continued improving platform and server behavior accumulated during the long development interval after 1.07.

## MPL Changes

### User-Time Access

A new `GetUserTime` function returned the number of minutes remaining for the current user. This allowed scripts to make decisions based on the user's active time allowance.

Example use:

```pascal
If GetUserTime < 5 Then
  WriteLn('Less than five minutes remain.');
```

The exact return type and behavior should be verified with the target 1.08 compiler.

### Integration Direction

MPL continued moving toward direct access to Mystic runtime state rather than requiring scripts to reproduce internal calculations. Scripts could increasingly rely on built-in functions for current session information.

## MPY and Python Changes

**MPY was not available in Mystic 1.08.**

The embedded Python engine and `.mpy` format were introduced later during Mystic 1.12 development.

## Compatibility Impact

- Telnet deployments may begin using `IPBLOCK.TXT`; document its path and matching rules locally.
- Scripts that estimate remaining user time should migrate to `GetUserTime` after verifying its units and return type.
- Network-message behavior may differ after reply and kludge-line fixes.

## Upgrade Actions

1. Back up server and message-base configuration.
2. Create and test `IPBLOCK.TXT` only after defining a recovery path for accidental blocks.
3. Recompile MPL source with the 1.08 compiler.
4. Test `GetUserTime` near time-limit boundaries.
5. Validate network-message imports, reply linking, and reader display.

## Documentation Impact

- `wiki/Functions.md`
- `wiki/User-Data.md`
- `wiki/Menu-Integration.md`
- `wiki/Version-Compatibility.md`

## Verification Record

```text
Mystic version/build: 1.08
Operating system:
MPLC version:
GetUserTime result type:
GetUserTime boundary test:
IP block test:
Network message test:
Notes:
```

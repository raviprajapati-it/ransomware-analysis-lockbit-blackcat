# Indicators of Compromise

## Overview

This document records selected indicators associated with the LockBit and BlackCat (ALPHV) samples examined during this project.

Indicators are derived from the original analysis evidence and controlled laboratory observations.

> **Important:** Cryptographic hashes identify the specific samples analysed in this project. They should not be treated as comprehensive detection coverage for the wider LockBit or BlackCat ransomware families.

---

## LockBit Sample

### Cryptographic Identifier

| Type | Value |
| --- | --- |
| SHA-256 | `47C6BA872EEA70CF59233FABBDD6D1978CFC75C602D4710B4F3D123E91F91822` |

**Source:** MalwareBazaar sample record preserved in the investigation evidence.

### Observed Behavioural Indicators

During controlled execution, the LockBit sample produced or exhibited:

- Rapid file-system activity
- High-volume file writes
- Encryption of prepared test files
- Ransom-note creation
- Desktop wallpaper modification
- Background process activity
- Directory scanning
- Interaction with recovery-related system components

### Ransomware Artefact

The observed desktop modification identified the ransomware as:

`LockBit Black`

The wallpaper directed the user toward a generated `.README.txt` ransom-note artefact.

Because ransom-note filenames may vary between samples or executions, this repository does not treat a single filename as a universal LockBit IOC.

---

## BlackCat (ALPHV) Sample

### Cryptographic Identifier

| Type | Value |
| --- | --- |
| SHA-256 | `DF8D000833243ACC0004595B3A8D4B66FCD7B76D685D5C2FF61EE2A40A0E92C` |

**Source:** MalwareBazaar sample record preserved in the investigation evidence.

### Observed Behavioural Indicators

During controlled execution, the BlackCat sample produced or exhibited:

- Background process activity
- Multiple process/thread activity
- Directory scanning
- Drive and system-path enumeration
- Process enumeration
- File enumeration
- System reconnaissance
- Encryption of prepared test files
- Ransom-note generation
- Identifiable system footprints

---

## Static Indicators

Static inspection of the samples identified functionality relevant to several Windows subsystems.

### LockBit

Notable imported functions included:

- `CreateFileW`
- `WriteFile`
- `DeleteFileW`
- `RegCreateKeyExW`
- `RegSetValueExW`
- `CreateProcessW`
- `OpenProcess`
- `CryptProtectMemory`

These imports are useful analytical indicators but should not individually be treated as malicious because legitimate Windows software can use the same APIs.

### BlackCat

Static analysis identified interaction with libraries including:

- `advapi32.dll`
- `kernel32.dll`
- `ws2_32.dll`
- `ntdll.dll`

These libraries provide functionality relevant to registry/service interaction, core system operations, networking, and low-level Windows operations.

Their presence alone is not sufficient to classify a binary as ransomware.

---

## Detection Considerations

Hash-based detection can identify known samples but is insufficient against modified or newly compiled ransomware variants.

Behavioural detection should therefore consider combinations of activity such as:

- Rapid modification of many files
- Abnormal file-encryption patterns
- Unexpected ransom-note creation
- Suspicious process creation
- Recovery-related system interaction
- Unusual directory or drive enumeration
- Reconnaissance followed by high-volume file modification

Individual indicators should be correlated with additional endpoint and security telemetry before determining that ransomware activity is occurring.

---

## Scope

This IOC list is intentionally limited to evidence associated with this project.

It is **not** intended to be:

- A comprehensive LockBit IOC feed
- A comprehensive BlackCat/ALPHV IOC feed
- A list of current ransomware infrastructure
- A replacement for current threat-intelligence sources
- Proof that every statically identified capability executed during the experiment

No live malicious URLs, ransomware payloads, credentials, or operational attacker infrastructure are provided.

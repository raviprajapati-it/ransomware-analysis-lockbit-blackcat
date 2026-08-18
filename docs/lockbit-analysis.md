# LockBit Ransomware Analysis

## Overview

This document presents the LockBit findings from the controlled malware-analysis environment used for this project.

The analysis combines static inspection of the ransomware binary with behavioural observations collected during execution inside an isolated Windows 10 virtual machine.

Findings are separated between static indicators and behaviour directly observed during execution to avoid overstating what the experiment demonstrated.

## Analysis Environment

The LockBit sample was analysed using:

- Windows 10 (64-bit) virtual machine
- Oracle VM VirtualBox
- Network connectivity disabled during execution
- PEStudio
- Process Monitor
- Process Explorer
- Autoruns
- Windows system utilities

The sample used in the original investigation was obtained from MalwareBazaar.

No ransomware binary is included in this repository.

---

## Static Analysis

### Portable Executable Characteristics

Inspection with PEStudio identified the sample as a Windows Portable Executable (PE).

Notable characteristics included:

- High entropy consistent with possible packing or obfuscation
- Multiple code and data sections
- Suspicious Windows API imports
- Strings and system references relevant to ransomware behaviour

High entropy is treated as an indicator rather than proof of a specific packing technique.

### Notable API Imports

Static inspection identified Windows API functions including:

| API | Relevance |
| --- | --- |
| `CreateFileW` | File access and creation |
| `WriteFile` | Writing data to files |
| `DeleteFileW` | File deletion |
| `RegCreateKeyExW` | Registry key creation/modification |
| `RegSetValueExW` | Registry value modification |
| `CreateProcessW` | Process creation |
| `OpenProcess` | Interaction with running processes |
| `CryptProtectMemory` | Cryptographic memory protection |

These imports indicate capabilities relevant to file manipulation, registry interaction, process control, and cryptographic operations.

The presence of an imported function alone does not prove that the associated behaviour occurred during execution.

### Strings and Indicators

String inspection identified material associated with:

- Targeted file extensions
- System-directory references
- Recovery-related commands
- Ransom-note content

These findings provided useful leads for subsequent behavioural analysis.

---

## Dynamic Analysis

### Process Activity

During controlled execution, the LockBit sample created background processes visible through the monitoring environment.

Process activity was examined using Process Explorer and Process Monitor.

### Directory and File Activity

The sample demonstrated rapid interaction with user-accessible directories and files.

Observed behaviour included:

- Directory scanning
- Repeated file access
- File metadata access
- High-volume write operations
- Encryption of prepared test files

The rapid sequence of file operations was consistent with the sample's observed encryption activity.

### Encryption Behaviour

Prepared test files became inaccessible following execution of the sample.

This provides direct experimental evidence of ransomware encryption behaviour within the controlled VM.

### Ransom Note

Following encryption, the sample generated a ransom note in the affected environment.

The note informed the victim that files had been encrypted and provided attacker-controlled recovery/payment instructions.

The repository may include a redacted screenshot of this artefact for defensive analysis purposes, but does not reproduce operational contact information as actionable infrastructure.

### Wallpaper Modification

The execution also resulted in modification of the Windows desktop wallpaper.

The displayed LockBit Black message directed the victim toward the generated ransom note.

This provides an additional visible artefact associated with successful execution of the ransomware sample.

### Recovery-Related Activity

Monitoring indicated interaction with recovery-related system components and behaviour consistent with an attempt to interfere with recovery mechanisms.

Because the isolated environment limited full execution of these behaviours, this project does **not** claim that deletion of shadow copies or complete disabling of Windows recovery mechanisms was experimentally confirmed.

---

## Observed Findings

The following behaviours are supported directly by the controlled experiment:

- Execution of the ransomware sample
- Background process activity
- Directory and file-system interaction
- Rapid file access operations
- Encryption of prepared test files
- High-volume write activity associated with encryption
- Ransom-note creation
- Desktop wallpaper modification
- Interaction with recovery-related components

---

## Contextual LockBit Capabilities

Public threat-intelligence reporting associates LockBit operations with techniques including:

- Credential abuse
- Network-share discovery
- Remote execution
- RDP abuse
- PowerShell
- PsExec
- WMIC
- Lateral movement
- Recovery inhibition

These techniques provide useful context for understanding the ransomware family, but they were **not all reproduced or demonstrated in this isolated single-host experiment**.

They should therefore not be interpreted as laboratory observations from this project.

---

## Defensive Relevance

The observed behaviour highlights several opportunities for endpoint detection and incident response:

- Monitor unusually rapid file modifications
- Detect suspicious process creation
- Alert on abnormal access to large numbers of user files
- Monitor recovery-related system activity
- Detect unexpected ransom-note creation
- Investigate mass file-encryption patterns
- Isolate affected endpoints rapidly when ransomware behaviour is detected

The experiment demonstrates why behavioural telemetry can be valuable even when malware is packed, obfuscated, or otherwise difficult to classify using static characteristics alone.

---

## Evidence

Selected screenshots from the original controlled investigation will be preserved under:

`evidence/screenshots/`

Evidence will be documented separately so that each screenshot clearly states what it demonstrates and what conclusions cannot be drawn from it.

---

## Safety Notice

This repository documents ransomware behaviour for defensive cybersecurity education and portfolio demonstration.

No LockBit executable, malicious payload, malware archive, credentials, or live attacker infrastructure is distributed through this repository.

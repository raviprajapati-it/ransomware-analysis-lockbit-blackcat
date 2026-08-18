# BlackCat (ALPHV) Ransomware Analysis

## Overview

This document presents the BlackCat (ALPHV) findings from the controlled malware-analysis environment used for this project.

The analysis combines static inspection of the ransomware binary with behavioural observations captured during execution inside an isolated Windows 10 virtual machine.

Findings are separated between static indicators and behaviour directly observed during execution.

## Analysis Environment

The BlackCat sample was analysed using:

- Windows 10 (64-bit) virtual machine
- Oracle VM VirtualBox
- Network connectivity disabled during execution
- PEStudio
- Process Monitor
- Process Explorer
- Autoruns
- Windows system utilities

The original sample was obtained from MalwareBazaar.

No ransomware binary is included in this repository.

---

## Static Analysis

### Portable Executable Characteristics

PEStudio inspection identified the sample as a Windows-compatible Portable Executable (PE).

Notable characteristics included:

- High entropy
- Large binary size
- Unusual section naming
- Extensive imported functions
- System-library interaction relevant to file, process, registry, and network operations

The unusual binary structure was consistent with the sample's Rust-based implementation.

### System Libraries

The sample interacted with several Windows libraries, including:

| Library | Relevance |
| --- | --- |
| `advapi32.dll` | Registry and service-related operations |
| `kernel32.dll` | Core Windows system functionality |
| `ws2_32.dll` | Network communication functions |
| `ntdll.dll` | Low-level Windows system calls |

These imports indicate access to functionality relevant to system manipulation, process activity, registry access, and networking.

Their presence does not by itself prove that every associated capability was exercised during the experiment.

### Static Capability Indicators

Static analysis indicated functionality relevant to:

- File enumeration
- File manipulation
- Process and thread management
- Registry access
- Networking
- Cryptographic operations

These indicators helped guide the subsequent dynamic analysis.

---

## Dynamic Analysis

### Process Activity

During controlled execution, the BlackCat sample created background processes and multiple threads.

Process activity was monitored using Process Explorer and Process Monitor.

### Reconnaissance Behaviour

The sample performed observable system and directory enumeration before encryption activity.

Observed behaviour included:

- Directory scanning
- Drive enumeration
- Process enumeration
- System-path inspection
- File enumeration
- Interaction with system components

The activity appeared more deliberate and staged than the LockBit execution observed in the same environment.

### File-System Activity

The sample interacted with the prepared test-file environment and system file structures.

Observed effects included:

- File enumeration
- File access
- Encryption-related activity
- Creation of temporary or modified artefacts
- System footprint generation

### Encryption Behaviour

The BlackCat sample encrypted prepared test files inside the controlled VM.

This provides direct experimental evidence of ransomware encryption behaviour.

### Ransom Note

The execution generated a ransom note following the encryption activity.

This confirms the sample's extortion workflow within the laboratory environment.

### System Interaction

The sample interacted with Windows system libraries and system resources during execution.

Observed activity was consistent with:

- File management
- Memory allocation
- Process execution
- System reconnaissance

No claim is made that every statically identified capability was exercised during the test.

---

## Observed Findings

The following behaviours are supported directly by the controlled experiment:

- Successful execution of the ransomware sample
- Background process creation
- Multiple process/thread activity
- Directory scanning
- Drive and system-path enumeration
- File enumeration
- System reconnaissance
- Encryption of prepared test files
- Ransom-note generation
- Creation of identifiable system footprints

---

## Contextual BlackCat Capabilities

Public threat-intelligence reporting associates BlackCat/ALPHV operations with techniques including:

- Credential theft and reuse
- Manual deployment
- PsExec
- PowerShell
- RDP
- SSH
- Lateral movement
- Data theft
- Double extortion
- Windows targeting
- Linux targeting
- ESXi targeting

These capabilities provide context for the ransomware family but were **not all demonstrated in this isolated Windows VM experiment**.

They should not be interpreted as direct laboratory observations from this project.

---

## Defensive Relevance

The observed behaviour highlights several opportunities for detection:

- Monitor suspicious process and thread creation
- Detect unusual directory or drive enumeration
- Investigate high-volume file access
- Detect abnormal file-encryption activity
- Monitor creation of suspicious temporary artefacts
- Alert on unexpected ransom-note creation
- Correlate reconnaissance activity with later encryption behaviour

The analysis also demonstrates the value of distinguishing reconnaissance activity from destructive activity during incident investigation.

---

## Evidence

Selected screenshots from the original controlled investigation will be stored under:

`evidence/screenshots/`

Each screenshot will be documented separately with a clear explanation of what it demonstrates.

---

## Safety Notice

This repository documents ransomware behaviour for defensive cybersecurity education and portfolio demonstration.

No BlackCat executable, malicious payload, malware archive, credentials, victim data, or live attacker infrastructure is distributed through this repository.

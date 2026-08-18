# Comparative Analysis — LockBit vs BlackCat (ALPHV)

## Overview

This document compares the LockBit and BlackCat (ALPHV) ransomware samples examined during the controlled malware-analysis project.

Both samples demonstrated functional ransomware behaviour inside an isolated Windows environment, but their observed execution patterns and static characteristics differed.

The comparison below distinguishes laboratory observations from broader capabilities documented through threat-intelligence research.

---

## High-Level Comparison

| Characteristic | LockBit | BlackCat (ALPHV) |
| --- | --- | --- |
| Sample format | Windows PE | Windows PE |
| Implementation | C/C++-associated architecture | Rust-based architecture |
| Analysis approach | Static + Dynamic | Static + Dynamic |
| File encryption observed | Yes | Yes |
| Ransom note observed | Yes | Yes |
| Directory scanning observed | Yes | Yes |
| Process activity observed | Yes | Yes |
| Reconnaissance behaviour | More limited in the experiment | More extensive in the experiment |
| Execution pattern | Rapid and aggressive | More deliberate and staged |
| System-library interaction | Observed | Observed |
| Network behaviour tested | No — isolated environment | No — isolated environment |

---

## Static Analysis Comparison

### LockBit

Static inspection identified:

- High entropy consistent with possible packing or obfuscation
- Multiple PE sections
- File-operation APIs
- Registry-related APIs
- Process-management APIs
- Cryptographic functionality
- Strings associated with ransomware behaviour

The binary appeared focused on rapid interaction with files and Windows system resources.

### BlackCat

Static inspection identified:

- High entropy
- Large binary size
- Unusual binary structure
- Extensive imported functionality
- File and process-management capabilities
- Registry interaction
- Networking-related functionality
- System-library interaction

The sample displayed characteristics consistent with a more complex Rust-based implementation.

---

## Dynamic Behaviour Comparison

### LockBit — Rapid Execution

LockBit demonstrated aggressive activity shortly after execution.

Observed behaviour included:

- Background process activity
- Rapid directory scanning
- Repeated file access
- High-volume write operations
- Encryption of prepared test files
- Ransom-note creation
- Desktop wallpaper modification
- Interaction with recovery-related components

The dominant characteristic of the observed execution was speed.

### BlackCat — Reconnaissance Before Encryption

BlackCat displayed a more deliberate execution pattern.

Observed behaviour included:

- Background process creation
- Multiple process/thread activity
- Directory scanning
- Drive enumeration
- Process enumeration
- System-path inspection
- File enumeration
- System reconnaissance
- Encryption of prepared test files
- Ransom-note generation
- Identifiable system footprints

Within this experiment, BlackCat performed more observable reconnaissance before encryption.

---

## Execution Pattern

The most notable behavioural difference observed during the project was the sequence and pace of activity.

### LockBit

The sample moved rapidly into file-system activity and encryption.

This produced a comparatively short window between execution and destructive effects.

### BlackCat

The sample performed more system enumeration and reconnaissance before encryption activity became apparent.

This resulted in a more staged behavioural pattern within the laboratory environment.

These observations apply specifically to the samples and controlled environment examined in this project and should not be interpreted as universal behaviour for every LockBit or BlackCat variant.

---

## Shared Behaviour

Despite their differences, both samples demonstrated several common ransomware characteristics:

- Windows system interaction
- Directory scanning
- File enumeration or access
- Process activity
- Encryption-related behaviour
- Successful encryption of prepared test files
- Ransom-note generation
- Observable forensic artefacts

These similarities provide useful behavioural indicators for ransomware detection even when the underlying ransomware families differ.

---

## Detection Opportunities

### LockBit-Oriented Detection

The observed LockBit behaviour suggests monitoring for:

- Sudden high-volume file access
- Rapid file modifications
- Mass encryption patterns
- Suspicious process creation
- Recovery-related system interaction
- Unexpected ransom-note creation

Because destructive activity occurred quickly, early behavioural detection and endpoint isolation are particularly important.

### BlackCat-Oriented Detection

The observed BlackCat behaviour suggests monitoring for:

- Unusual directory enumeration
- Drive enumeration
- Process enumeration
- Suspicious system discovery
- Abnormal process/thread activity
- File-encryption patterns
- Ransom-note creation

Reconnaissance activity may provide detection opportunities before destructive encryption becomes dominant.

---

## Threat-Intelligence Context

Broader research associates both ransomware operations with enterprise intrusion techniques that extend beyond what was reproduced in this experiment.

Examples include:

| Capability | LockBit | BlackCat |
| --- | --- | --- |
| Credential abuse | Reported | Reported |
| Remote execution | Reported | Reported |
| Lateral movement | Reported | Reported |
| PsExec usage | Reported | Reported |
| RDP usage | Reported | Reported |
| PowerShell usage | Reported | Reported |
| Linux targeting | Limited/family dependent | Reported |
| ESXi targeting | Family/version dependent | Reported |
| Data theft/extortion | Reported | Reported |

These entries provide threat-intelligence context only.

They are **not claims that each technique was reproduced during this laboratory experiment**.

---

## Key Takeaways

1. Both samples demonstrated functional ransomware behaviour in the isolated Windows environment.

2. Both successfully affected prepared test files and produced ransom-note artefacts.

3. LockBit demonstrated a faster and more aggressive file-encryption pattern during the experiment.

4. BlackCat demonstrated more observable reconnaissance and system enumeration before encryption.

5. Static indicators provided useful information about potential capabilities, but dynamic monitoring was necessary to determine which behaviours actually occurred.

6. Process and file-system telemetry provided valuable evidence for distinguishing normal system activity from ransomware behaviour.

7. Behavioural detection is important because ransomware families can differ significantly in implementation while producing similar destructive outcomes.

---

## Scope and Limitations

The analysis was performed inside a controlled single-host Windows virtual machine with network connectivity disabled during malware execution.

As a result, the experiment was not designed to demonstrate:

- Command-and-control communication
- Internet-based infrastructure
- Credential theft across an enterprise
- Network propagation
- Remote execution across multiple hosts
- Real-world lateral movement
- Data exfiltration
- Linux or ESXi execution

References to these capabilities elsewhere in the project are clearly identified as threat-intelligence context rather than experimental findings.

---

## Conclusion

The controlled comparison demonstrated two different ransomware execution patterns leading toward a similar outcome: loss of access to data and generation of extortion artefacts.

LockBit exhibited faster, more aggressive file-system activity, while BlackCat demonstrated a more reconnaissance-oriented sequence before encryption.

The comparison reinforces the value of combining static analysis with behavioural monitoring and of preserving a clear distinction between observed evidence, static capability indicators, and externally documented threat intelligence.

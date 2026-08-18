# Analysis Methodology

## Overview

This project uses a hybrid malware-analysis methodology combining static and dynamic analysis to examine LockBit and BlackCat (ALPHV) ransomware samples.

The objective is to compare observable binary characteristics and runtime behaviour while maintaining a controlled and isolated analysis environment.

## Laboratory Environment

Analysis was conducted inside an isolated Oracle VM VirtualBox environment configured with:

- Windows 10 (64-bit)
- 4 GB RAM
- Dedicated virtual hard disk
- Network connectivity disabled during malware execution
- Virtual machine snapshots for restoration to a clean baseline

Network access was disabled after sample acquisition to prevent the ransomware from communicating with external infrastructure.

## Sample Acquisition

The ransomware samples used during the original investigation were obtained from MalwareBazaar in password-protected archives.

The repository does **not** contain ransomware binaries, malicious payloads, or password-protected malware archives.

## Analysis Tools

The investigation used:

- **PEStudio** — PE structure, metadata, imports, strings, and static indicators
- **Process Monitor** — file-system, registry, and process activity
- **Process Explorer** — process and DLL inspection
- **Autoruns** — inspection of persistence-related locations
- **Windows utilities** — File Explorer, Task Manager, and Event Viewer
- **Oracle VM VirtualBox** — isolated execution environment

## Static Analysis

Static analysis was performed before execution and included:

1. Inspecting PE headers and binary metadata
2. Reviewing imported libraries and functions
3. Examining entropy and indicators of packing or obfuscation
4. Extracting and reviewing embedded strings
5. Identifying suspicious system-interaction indicators

Static findings are treated as indicators of capability and are not automatically interpreted as proof that a behaviour occurred during execution.

## Dynamic Analysis

Dynamic analysis followed a controlled workflow:

1. Restore the virtual machine to a clean snapshot
2. Confirm network isolation
3. Launch monitoring tools
4. Prepare non-sensitive test files
5. Execute the ransomware sample
6. Observe process, file-system, and system activity
7. Record relevant logs and screenshots
8. Inspect encrypted files and ransom-note behaviour
9. Revert the virtual machine to the clean snapshot

## Evidence Interpretation

This project distinguishes between three types of findings:

### Directly Observed

Behaviour captured during controlled execution, such as file encryption, ransom-note creation, process activity, directory enumeration, or other visible system changes.

### Static Indicators

Characteristics identified through binary inspection, including imported functions, libraries, strings, PE metadata, and structural properties.

### Threat-Intelligence Context

Documented capabilities or attack techniques associated with LockBit or BlackCat in external research but not directly demonstrated in the isolated experiment.

Threat-intelligence context is not presented as laboratory observation.

## Safety and Ethics

This repository is intended exclusively for defensive cybersecurity education and malware-analysis documentation.

No live ransomware samples, executable malware, credentials, victim data, or operational malicious infrastructure are included.

# Ransomware Analysis — LockBit & BlackCat (ALPHV)

Comparative **static and dynamic malware analysis** of LockBit and BlackCat (ALPHV) ransomware samples inside an isolated Windows laboratory environment.

This project examines how the two ransomware samples differ in binary characteristics, system interaction, reconnaissance, encryption behaviour, and observable forensic artefacts.

The investigation combines **PEStudio static analysis** with behavioural monitoring using **Process Monitor, Process Explorer, Autoruns, and Windows utilities**.

> **Key distinction:** Laboratory observations, static capability indicators, and external threat-intelligence context are treated separately throughout this repository. Capabilities not reproduced in the controlled environment are not presented as experimental findings.

---

## Project Overview

LockBit and BlackCat represent two prominent ransomware families associated with Ransomware-as-a-Service (RaaS) operations.

The objective of this project was to analyse representative samples in a controlled environment and compare:

* Portable Executable characteristics
* Imported APIs and system libraries
* Embedded/static indicators
* Process activity
* Directory and file-system interaction
* Reconnaissance behaviour
* File-encryption activity
* Ransom-note behaviour
* Observable system modifications
* Forensic artefacts
* Defensive detection opportunities

The project originated from academic malware-analysis work and has been reconstructed as a professional technical case study using the original analysis evidence.

---

## Analysis Environment

The malware was examined inside an isolated virtual laboratory.

| Component     | Configuration                     |
| ------------- | --------------------------------- |
| Hypervisor    | Oracle VM VirtualBox              |
| Guest OS      | Windows 10 (64-bit)               |
| Memory        | 4 GB RAM                          |
| Storage       | Dedicated virtual disk            |
| Networking    | Disabled during malware execution |
| Recovery      | Virtual machine snapshots         |
| Sample source | MalwareBazaar                     |

Network connectivity was disabled during execution to prevent communication with external infrastructure.

Snapshots were used to restore the environment to a known state following testing.

### Core Analysis Tools

* **PEStudio** — PE metadata, imports, strings and static indicators
* **Process Monitor** — file-system, registry and process activity
* **Process Explorer** — process and DLL inspection
* **Autoruns** — persistence-related inspection
* **Windows utilities** — File Explorer, Task Manager and Event Viewer
* **Oracle VM VirtualBox** — isolated execution environment

Full methodology:

**[Analysis Methodology →](docs/analysis-methodology.md)**

---

## Investigation Workflow

```text
Sample Acquisition
        │
        ▼
Sample Identification
        │
        ▼
Static Analysis
PE Headers / Imports / Strings / Metadata
        │
        ▼
Restore Clean VM Snapshot
        │
        ▼
Confirm Network Isolation
        │
        ▼
Launch Monitoring Tools
        │
        ▼
Controlled Sample Execution
        │
        ▼
Process + File-System Observation
        │
        ▼
Encryption / Ransomware Artefact Analysis
        │
        ▼
Evidence Collection
        │
        ▼
Revert VM to Clean Snapshot
```

---

# Key Findings

Both analysed samples demonstrated functional ransomware behaviour inside the controlled Windows environment.

### LockBit

Observed behaviour included:

* Background process activity
* Rapid directory and file interaction
* Repeated file access
* High-volume write operations
* Encryption of prepared test files
* Ransom-note creation
* Desktop wallpaper modification
* Interaction with recovery-related components

The dominant characteristic observed during execution was **speed**.

### BlackCat (ALPHV)

Observed behaviour included:

* Background process creation
* Multiple process/thread activity
* Directory scanning
* Drive enumeration
* Process enumeration
* System-path inspection
* File enumeration
* System reconnaissance
* Encryption of prepared test files
* Ransom-note generation
* Identifiable system footprints

Within the tested environment, BlackCat demonstrated a more **deliberate and reconnaissance-oriented execution pattern** before encryption.

---

# LockBit vs BlackCat

| Characteristic              | LockBit          | BlackCat (ALPHV)       |
| --------------------------- | ---------------- | ---------------------- |
| Sample format               | Windows PE       | Windows PE             |
| Analysis                    | Static + Dynamic | Static + Dynamic       |
| File encryption observed    | Yes              | Yes                    |
| Ransom note observed        | Yes              | Yes                    |
| Directory scanning observed | Yes              | Yes                    |
| Process activity observed   | Yes              | Yes                    |
| Reconnaissance observed     | More limited     | More extensive         |
| Execution pattern           | Rapid/aggressive | More deliberate/staged |
| System interaction          | Observed         | Observed               |
| Network behaviour tested    | No               | No                     |

The comparison applies specifically to the samples and controlled environment examined in this project. It should not be interpreted as universal behaviour for every LockBit or BlackCat variant.

**[Full Comparative Analysis →](docs/comparative-analysis.md)**

---

# Static Analysis

## LockBit

PEStudio inspection identified characteristics including:

* Windows Portable Executable structure
* High entropy consistent with possible packing or obfuscation
* Multiple code/data sections
* File-operation APIs
* Registry-related APIs
* Process-control APIs
* Cryptographic functionality
* Ransomware-related strings and system references

Notable imported functions included:

```text
CreateFileW
WriteFile
DeleteFileW
RegCreateKeyExW
RegSetValueExW
CreateProcessW
OpenProcess
CryptProtectMemory
```

Imported APIs are treated as **capability indicators**, not proof that every associated behaviour occurred during execution.

**[LockBit Technical Analysis →](docs/lockbit-analysis.md)**

---

## BlackCat (ALPHV)

Static inspection identified:

* Windows-compatible PE structure
* High entropy
* Large binary size
* Unusual binary characteristics
* Extensive imported functionality
* File/process-management capability indicators
* Registry interaction
* Networking-related functionality

System libraries observed during analysis included:

```text
advapi32.dll
kernel32.dll
ws2_32.dll
ntdll.dll
```

These libraries provide functionality relevant to Windows system operations but are not malicious indicators when considered individually.

**[BlackCat Technical Analysis →](docs/blackcat-analysis.md)**

---

# Dynamic Analysis

Dynamic analysis was conducted only after restoring the VM to a controlled baseline and confirming network isolation.

Monitoring focused on:

* Process creation
* File access
* Registry activity
* Directory enumeration
* System interaction
* Encryption behaviour
* Ransomware artefacts
* Post-execution changes

## LockBit Execution

LockBit moved rapidly into file-system activity.

The experiment recorded encryption of prepared test files, high-volume file operations, ransom-note creation and a visible desktop modification.

### Selected LockBit Evidence

![LockBit dynamic analysis](evidence/screenshots/03-lockbit-dynamic-analysis.png)

*Process Monitor, Process Explorer, Autoruns and the controlled test-file environment during analysis.*

![LockBit wallpaper modification](evidence/screenshots/04-lockbit-wallpaper-modification.png)

*Post-execution desktop modification produced by the analysed LockBit sample.*

---

## BlackCat Execution

BlackCat demonstrated more observable enumeration and system reconnaissance before encryption activity.

The sample interacted with directories, system paths, processes and file structures before producing ransomware effects in the test environment.

### Selected BlackCat Evidence

![BlackCat PEStudio analysis](evidence/screenshots/06-blackcat-pestudio-binary-analysis.png)

*Static inspection of the BlackCat sample using PEStudio.*

Additional evidence:

**[Complete Evidence Gallery →](evidence/README.md)**

---

# Laboratory Safety

Malware execution was performed inside an isolated VirtualBox environment.

![Disabled VM network adapter](evidence/screenshots/07-isolated-vm-network-disabled.png)

*VirtualBox network adapter disabled for the malware-execution environment.*

Snapshots provided clean restoration points throughout the investigation:

![Analysis VM snapshots](evidence/screenshots/08-analysis-vm-snapshots.png)

The repository does **not** contain:

* LockBit executables
* BlackCat executables
* Malware archives
* Password-protected malware packages
* Credentials
* Victim information
* Operational attacker infrastructure

---

# Verified Sample Identifiers

The following SHA-256 values identify the specific samples documented by the preserved MalwareBazaar evidence.

| Family   | SHA-256                                                            |
| -------- | ------------------------------------------------------------------ |
| LockBit  | `47C6BA872EEA70CF59233FABBDD6D1978CFC75C602D4710B4F3D123E91F91822` |
| BlackCat | `DF8D000833243ACC0004595B3A8D4B66FCD7B76D685D5C2FF61EE2A40A0E92C`  |

These hashes identify the analysed samples only. They are not comprehensive detection coverage for either ransomware family.

**[Indicators of Compromise →](iocs/indicators-of-compromise.md)**

---

# Detection & Defensive Relevance

The experiment highlights several behavioural opportunities for ransomware detection.

## File-System Indicators

Monitor for:

* Rapid modification of many files
* High-volume write operations
* Abnormal file-encryption patterns
* Unexpected ransom-note creation
* Sudden changes across user-accessible directories

## Process Indicators

Investigate:

* Suspicious process creation
* Unexpected process/thread activity
* Processes accessing large numbers of files
* Unusual interaction with recovery-related components

## Reconnaissance Indicators

Potential early-warning activity includes:

* Unexpected drive enumeration
* Directory enumeration
* Process enumeration
* System-path discovery
* Reconnaissance followed by high-volume file modification

Individual events should be correlated with broader endpoint telemetry before classifying activity as ransomware.

---

# Evidence Standard

A central principle of this project is separating three evidence categories.

### Directly Observed

Behaviour captured during controlled execution.

Examples:

* Test-file encryption
* Process activity
* Directory enumeration
* Ransom-note creation
* LockBit wallpaper modification

### Static Indicators

Characteristics discovered through binary inspection.

Examples:

* Imported APIs
* System libraries
* PE metadata
* Strings
* Structural characteristics

Static indicators describe potential capabilities but do not prove execution.

### Threat-Intelligence Context

Public research associates LockBit and BlackCat operations with broader techniques such as:

* Credential abuse
* Remote execution
* Lateral movement
* PsExec
* PowerShell
* RDP
* Data theft
* Double extortion
* Cross-platform targeting

These techniques were **not all reproduced in this laboratory** and are therefore not presented as experimental findings.

---

# Scope & Limitations

The experiment used a controlled **single-host Windows virtual machine with networking disabled during malware execution**.

The project therefore does not claim experimental confirmation of:

* Command-and-control communication
* External network infrastructure
* Multi-host propagation
* Enterprise lateral movement
* Credential theft across a network
* External data exfiltration
* Linux execution
* ESXi execution
* Every capability associated with either ransomware family

This distinction is intentional and preserves the difference between observation and interpretation.

---

# Repository Structure

```text
ransomware-analysis-lockbit-blackcat/
├── README.md
├── LICENSE
│
├── docs/
│   ├── analysis-methodology.md
│   ├── lockbit-analysis.md
│   ├── blackcat-analysis.md
│   └── comparative-analysis.md
│
├── evidence/
│   ├── README.md
│   └── screenshots/
│       ├── 01-lockbit-malwarebazaar-sample.png
│       ├── 02-lockbit-pestudio-binary-analysis.png
│       ├── 03-lockbit-dynamic-analysis.png
│       ├── 04-lockbit-wallpaper-modification.png
│       ├── 05-blackcat-malwarebazaar-sample.png
│       ├── 06-blackcat-pestudio-binary-analysis.png
│       ├── 07-isolated-vm-network-disabled.png
│       └── 08-analysis-vm-snapshots.png
│
└── iocs/
    └── indicators-of-compromise.md
```

---

# Documentation

| Document                                                     | Description                                            |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [Analysis Methodology](docs/analysis-methodology.md)         | Laboratory design, tools and static/dynamic workflow   |
| [LockBit Analysis](docs/lockbit-analysis.md)                 | LockBit static and behavioural findings                |
| [BlackCat Analysis](docs/blackcat-analysis.md)               | BlackCat static and behavioural findings               |
| [Comparative Analysis](docs/comparative-analysis.md)         | Technical comparison of both samples                   |
| [Evidence Gallery](evidence/README.md)                       | Selected original investigation screenshots            |
| [Indicators of Compromise](iocs/indicators-of-compromise.md) | Verified sample identifiers and behavioural indicators |

---

# Skills Demonstrated

This project demonstrates practical experience with:

* Malware Analysis
* Static Analysis
* Dynamic Analysis
* Ransomware Analysis
* Windows Internals
* Portable Executable Analysis
* PEStudio
* Process Monitor
* Process Explorer
* Sysinternals
* Virtualised Malware Labs
* IOC Analysis
* Behavioural Detection
* Incident Response
* Evidence Interpretation
* Technical Documentation

---

# Ethical & Safety Notice

This repository is intended solely for **defensive cybersecurity education, malware-analysis documentation, and professional portfolio demonstration**.

No executable ransomware or malicious payload is distributed.

The project focuses on understanding malicious behaviour so that defenders can improve detection, investigation, containment and response.

---

## Author

**Ravi Prajapati**

Enterprise IT Support | Cybersecurity | Network Security | Security Operations

[LinkedIn](https://www.linkedin.com/in/ravi-prajapati-it) · [GitHub](https://github.com/raviprajapati-it)

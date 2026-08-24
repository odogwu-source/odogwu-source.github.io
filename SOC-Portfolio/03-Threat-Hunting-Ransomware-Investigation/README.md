# Threat Hunting Ransomware Investigation — Credential Dumping, Remote PowerShell & File Encryption

## Executive Summary

This case study documents a hands-on threat-hunting investigation performed in a controlled TryHackMe simulation. The investigation reconstructed a multi-stage ransomware intrusion affecting `WKSTN-02` and `WKSTN-03` on 26 September 2023.

The evidence supported a progression from credential-access activity using `Invoke-SharpKatz`, through PowerShell-based remote execution and payload transfer, to execution of `bomb.exe` and widespread file encryption using the `.777zzz` extension. The investigation also identified the external infrastructure used to deliver the payload and produced containment, credential-remediation, blocking and forensic follow-up recommendations.

> **Training disclosure:** This is a controlled cybersecurity simulation documented to demonstrate threat-hunting methodology, SIEM analysis, ATT&CK mapping and incident-response judgement. It is not represented as a production incident handled for a client or employer.

## Investigation Objective

The objective was to validate or disprove a threat hypothesis involving an initial compromise, credential theft, lateral movement, malicious payload delivery and ransomware-style encryption across multiple Windows workstations.

## Environment & Data Sources

- TryHackMe Threat Hunting Simulator
- Elastic / Kibana Discover
- Windows event telemetry collected through Winlogbeat
- PowerShell operational telemetry
- Process-creation events
- Host and user context from the simulation documentation
- MITRE ATT&CK for tactic and technique mapping

## Affected Assets

| Asset | Role in the Incident | Evidence |
|---|---|---|
| `WKSTN-02` | Initial compromised workstation / PowerShell execution source | SharpKatz activity, PowerShell `Invoke-Command`, encryption telemetry |
| `WKSTN-03` | Remote target / payload execution host | Remote PowerShell activity and `bomb.exe` process creation |
| `anna.jones` | Compromised user context associated with `WKSTN-02` | Simulation documentation and PowerShell telemetry |

## Key Findings

### 1. Credential Dumping
PowerShell telemetry showed execution of `Invoke-SharpKatz` on `WKSTN-02`. The activity is consistent with credential-access behavior and maps to **OS Credential Dumping (T1003)** under **Credential Access (TA0006)**.

### 2. PowerShell Command Execution
At approximately `15:41:29` UTC, PowerShell `Invoke-Command` activity was observed on `WKSTN-02`. This provided the execution mechanism used to continue the attack chain and initiate remote activity.

### 3. Remote Execution and Lateral Movement
The PowerShell command targeted `WKSTN-03`, demonstrating cross-workstation execution. The observed behavior is consistent with remote service / PowerShell remoting activity used for lateral movement.

### 4. Malicious Payload Download
The investigation identified retrieval of the malicious payload from:

`http://www.7zipp.org/a/777bomb.exe`

The payload was saved as `bomb.exe` under the user profile path associated with the remote operation. This activity maps to **Ingress Tool Transfer (T1105)**.

### 5. Payload Execution
A process-creation event on `WKSTN-03` confirmed execution of `bomb.exe` at approximately `15:41:51` UTC.

### 6. Mass File Encryption
PowerShell event telemetry at approximately `15:45:12` UTC showed repeated `Encrypting:` activity affecting documents, training material, network diagrams, backup plans, reports, utilities and executables. Encrypted files were observed with the extension `.777zzz`, supporting **Data Encrypted for Impact (T1486)** under the **Impact** tactic.

## Reconstructed Attack Chain

1. **Credential Access** — `Invoke-SharpKatz` used on `WKSTN-02` to obtain credentials.
2. **Execution** — PowerShell `Invoke-Command` used to continue attacker activity.
3. **Lateral Movement** — Remote PowerShell activity targeted `WKSTN-03`.
4. **Command and Control / Tool Transfer** — `777bomb.exe` retrieved from `7zipp.org` and written as `bomb.exe`.
5. **Execution** — `bomb.exe` launched on `WKSTN-03`.
6. **Impact** — Files encrypted at scale and renamed with `.777zzz`.

## MITRE ATT&CK Mapping

| Tactic | Technique | ID | Evidence |
|---|---|---|---|
| Credential Access | OS Credential Dumping | `T1003` | `Invoke-SharpKatz` activity |
| Execution | Command and Scripting Interpreter: PowerShell | `T1059.001` | PowerShell `Invoke-Command` telemetry |
| Lateral Movement | Remote Services / Windows Remote Management | `T1021` / `T1021.006` | Remote execution from `WKSTN-02` to `WKSTN-03` |
| Command and Control | Ingress Tool Transfer | `T1105` | Download of `777bomb.exe` from external infrastructure |
| Impact | Data Encrypted for Impact | `T1486` | Repeated encryption events and `.777zzz` extension |

## Indicators of Compromise

| Type | Indicator |
|---|---|
| Executable | `bomb.exe` |
| Executable | `777bomb.exe` |
| Malicious domain | `7zipp.org` |
| URL | `http://www.7zipp.org/a/777bomb.exe` |
| File extension | `.777zzz` |
| Credential-dumping tool | `Invoke-SharpKatz` |
| Affected host | `WKSTN-02` |
| Affected host | `WKSTN-03` |
| Compromised user context | `anna.jones` |

## Incident Classification

**Classification:** Confirmed malicious activity / ransomware intrusion in a controlled simulation.

The telemetry supports a coordinated attack sequence involving credential theft, remote execution, payload delivery and destructive encryption behavior across at least two workstations.

## Recommended Response Actions

1. **Containment:** Isolate `WKSTN-02` and `WKSTN-03` from the network to stop further lateral movement and encryption.
2. **Credential response:** Disable or reset `anna.jones` credentials and review authentication activity for secondary compromise.
3. **Network blocking:** Block `7zipp.org` and the observed payload URL at DNS, proxy and perimeter controls.
4. **Host remediation:** Quarantine/remove `bomb.exe` and related artifacts; rebuild systems where integrity cannot be assured.
5. **Forensic scoping:** Search the estate for `Invoke-SharpKatz`, `777bomb.exe`, `bomb.exe`, `.777zzz`, PowerShell remoting and related process chains.
6. **Persistence review:** Inspect scheduled tasks, services, registry run keys, PowerShell profiles and remote-management configuration.
7. **Impact validation:** Determine the full set of encrypted files and confirm the integrity and availability of backups.
8. **Exfiltration review:** Examine proxy, DNS, firewall and endpoint telemetry for evidence of outbound data transfer before encryption.
9. **Recovery:** Restore clean systems and validated backups only after containment and credential remediation are complete.
10. **Detection engineering:** Create detections for SharpKatz/Mimikatz-like behavior, suspicious `Invoke-Command`, remote PowerShell, anomalous tool transfer and mass file modifications.

## Detection Opportunities

Potential monitoring and detection logic should focus on:

- PowerShell script-block content containing `Invoke-SharpKatz`
- PowerShell `Invoke-Command` targeting remote workstations
- Downloads of executable content through PowerShell web requests
- New process creation for unexpected executables in user profile directories
- High-volume file modification/rename activity
- Creation of unusual extensions such as `.777zzz`
- Credential-access tooling followed quickly by remote execution
- Same-user activity across multiple hosts in a short period

## Analyst Assessment

The strongest element of this investigation was the ability to correlate isolated telemetry into a coherent intrusion narrative. Credential-access evidence, remote PowerShell execution, external payload retrieval, remote process creation and mass encryption were not treated as independent events; they were linked by host, user, timestamp and attacker objective.

The investigation demonstrates practical threat-hunting skills in hypothesis validation, SIEM pivoting, event correlation, IOC extraction, MITRE ATT&CK mapping, scoping and incident-response recommendation. Where the simulation interface did not provide complete validation of every possible timeline stage, this portfolio report intentionally distinguishes **observed evidence** from **analyst inference** rather than overstating certainty.

## Evidence

Evidence screenshots from the investigation will be added below in chronological order as they are curated. Screenshots that only show simulator scoring or incomplete-lab status are intentionally excluded because they do not add technical evidentiary value.

### Evidence Register

| Ref | Evidence | Status |
|---|---|---|
| E01 | SharpKatz / credential-dumping telemetry | Pending screenshot upload |
| E02 | PowerShell `Invoke-Command` activity | Pending screenshot upload |
| E03 | Remote execution from `WKSTN-02` to `WKSTN-03` | Pending screenshot upload |
| E04 | `777bomb.exe` download / `bomb.exe` delivery | Pending screenshot upload |
| E05 | `bomb.exe` process creation on `WKSTN-03` | Pending screenshot upload |
| E06 | Mass file-encryption telemetry | Pending screenshot upload |
| E07 | `.777zzz` extension evidence | Pending screenshot upload |
| E08 | Threat report IOC table | Pending screenshot upload |
| E09 | Impact & Findings / remediation section | Pending screenshot upload |

## Skills Demonstrated

`Threat Hunting` · `Elastic/Kibana` · `Winlogbeat` · `PowerShell Analysis` · `Windows Event Analysis` · `Process Creation Analysis` · `Credential Access Investigation` · `Lateral Movement Analysis` · `Ransomware Investigation` · `IOC Extraction` · `MITRE ATT&CK Mapping` · `Incident Scoping` · `Containment & Remediation` · `Security Reporting`

---

[← Back to SOC Portfolio](../)

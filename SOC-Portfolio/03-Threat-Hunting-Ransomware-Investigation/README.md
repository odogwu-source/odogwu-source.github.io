# Threat Hunting Ransomware Investigation — Credential Dumping, Remote PowerShell & File Encryption

## Executive Summary

This case study documents a hands-on threat-hunting investigation performed in a controlled TryHackMe simulation. The investigation reconstructed a multi-stage ransomware intrusion affecting `WKSTN-02` and `WKSTN-03` on 26 September 2023.

The evidence supported a progression from credential-access activity using `Invoke-SharpKatz`, through PowerShell-based remote execution and payload transfer, to execution of `bomb.exe` and widespread file encryption using the `.777zzz` extension. The investigation also identified the external infrastructure used to deliver the payload and produced containment, credential-remediation, blocking and forensic follow-up recommendations.

> **Training disclosure:** This is a controlled cybersecurity simulation documented to demonstrate threat-hunting methodology, SIEM analysis, ATT&CK mapping and incident-response judgement. It is not represented as a production incident handled for a client or employer.

## Investigation Objective

The objective was to validate or disprove a threat hypothesis involving an initial compromise, credential theft, lateral movement, malicious payload delivery and ransomware-style encryption across multiple Windows workstations.

The simulator mission required the analyst to validate a hunting hypothesis, review external IOCs, reconstruct the attack chain, determine the incident scope, and generate a final threat-hunting report.

## Environment & Data Sources

- TryHackMe Threat Hunting Simulator
- Elastic / Kibana Discover
- Windows event telemetry collected through Winlogbeat
- PowerShell operational telemetry, including Event IDs `4103` and `4104`
- Process-creation telemetry
- Host and user context from the simulation documentation
- External IOC intelligence supplied by the simulated SOC team
- MITRE ATT&CK for tactic and technique mapping

## Affected Assets

| Asset | Role in the Incident | Evidence |
|---|---|---|
| `WKSTN-02` | Initial compromised workstation / PowerShell execution source | SharpKatz activity, PowerShell `Invoke-Command`, encryption telemetry |
| `WKSTN-03` | Remote target / payload execution host | Remote PowerShell activity and `bomb.exe` process creation |
| `anna.jones` | Compromised user context associated with `WKSTN-02` | Simulation documentation and PowerShell telemetry |

## Threat Intelligence Reviewed

The simulator supplied the following IOC intelligence as starting pivots. These indicators were treated as **external leads**, not automatically assumed to be observed on the compromised hosts unless corroborated in SIEM telemetry.

| Type | Indicator |
|---|---|
| Domain | `7zipp.org` |
| IP address | `206.189.34.218` |
| File extension | `*.777zzz` |
| File extension | `*.msi` |
| File extension | `*.ps1` |
| Filename | `7zipp.exe` |
| Filename | `7zipp.dll` |
| Filename | `mimikatz.exe` |
| Hash | `4b9213d22989474b467aa53080d9e295` |
| Hash | `29efd64dd3c7fe1e2b022b7ad73a1ba5` |
| Hash | `61c0810a23580cf492a6ba4ff7654566108331e7a4134c968c2d6a05261b2d8a1` |
| Hash | `fd713992f39338986e8573aff1232675323fde827328159b29a31cc3cd515874` |

## Key Findings

### 1. Credential Dumping

PowerShell Script Block telemetry on `WKSTN-02` showed `Invoke-SharpKatz` at approximately `15:36:10` UTC. The activity is consistent with credential-access behavior and maps to **OS Credential Dumping (T1003)** under **Credential Access (TA0006)**.

### 2. PowerShell Command Execution

At approximately `15:41:29` UTC, PowerShell `Invoke-Command` activity was observed on `WKSTN-02`. This provided the execution mechanism used to continue the attack chain and initiate remote activity.

### 3. Remote Execution and Lateral Movement

The PowerShell command targeted `WKSTN-03`, demonstrating cross-workstation execution. The evidence showed PowerShell remoting from `WKSTN-02` to `WKSTN-03` under the compromised user context. For analyst ATT&CK mapping, the behavior is most closely aligned with PowerShell execution plus remote services/WinRM-style lateral movement.

> **Mapping note:** The simulator timeline card displayed `T1563` for the remote PowerShell stage. This portfolio retains the simulator evidence but uses behavior-based analyst mapping in the ATT&CK table below rather than blindly reproducing an interface label.

### 4. Malicious Payload Download

The investigation identified retrieval of the malicious payload from:

`http://www.7zipp.org/a/777bomb.exe`

PowerShell telemetry showed the executable being downloaded and written as `bomb.exe` in the user path as part of the remote operation. This activity maps to **Ingress Tool Transfer (T1105)**.

### 5. Payload Execution

A process-creation event on `WKSTN-03` confirmed execution of `bomb.exe` at approximately `15:41:51.778` UTC. The process telemetry linked the executable to the remote target host and provided direct evidence that the transferred payload ran.

### 6. Mass File Encryption

PowerShell Event ID `4103` telemetry at approximately `15:45:12.177` UTC showed repeated `Encrypting:` activity on `WKSTN-02`. The encrypted data included training documents and other user files. Additional evidence showed filenames receiving the `.777zzz` suffix, supporting **Data Encrypted for Impact (T1486)** under the **Impact** tactic.

## Reconstructed Attack Chain

1. **Credential Access** — `Invoke-SharpKatz` observed on `WKSTN-02`.
2. **Execution** — PowerShell `Invoke-Command` used to continue attacker activity.
3. **Lateral Movement** — Remote PowerShell activity targeted `WKSTN-03`.
4. **Command and Control / Tool Transfer** — `777bomb.exe` retrieved from `7zipp.org` and written as `bomb.exe`.
5. **Execution** — `bomb.exe` launched on `WKSTN-03`.
6. **Impact** — Files encrypted at scale and renamed with `.777zzz`.

The simulator timeline was also used to document the major validated stages. The portfolio intentionally reports the **six evidence-supported stages** above rather than fabricating additional attack steps merely to fill interface capacity.

## MITRE ATT&CK Mapping

| Tactic | Technique | ID | Evidence |
|---|---|---|---|
| Credential Access | OS Credential Dumping | `T1003` | `Invoke-SharpKatz` Script Block activity |
| Execution | Command and Scripting Interpreter: PowerShell | `T1059.001` | PowerShell `Invoke-Command` telemetry |
| Lateral Movement | Remote Services / Windows Remote Management | `T1021` / `T1021.006` | Remote execution from `WKSTN-02` to `WKSTN-03` |
| Command and Control | Ingress Tool Transfer | `T1105` | Download of `777bomb.exe` from external infrastructure |
| Impact | Data Encrypted for Impact | `T1486` | Repeated encryption events and `.777zzz` extension |

## Confirmed Indicators from the Investigation

| Type | Indicator | Context |
|---|---|---|
| Executable | `bomb.exe` | Executed on `WKSTN-03` |
| Executable | `777bomb.exe` | Payload name in download URL |
| Malicious domain | `7zipp.org` | Payload source |
| URL | `http://www.7zipp.org/a/777bomb.exe` | PowerShell retrieval path |
| File extension | `.777zzz` | Encrypted-file suffix |
| Credential-dumping tool | `Invoke-SharpKatz` | PowerShell Script Block evidence |
| Affected host | `WKSTN-02` | Credential access / PowerShell / encryption |
| Affected host | `WKSTN-03` | Remote target / payload execution |
| Compromised user context | `anna.jones` | Associated with the attack sequence |

## Incident Classification

**Classification:** Confirmed malicious activity / ransomware intrusion in a controlled simulation.

The telemetry supports a coordinated attack sequence involving credential theft, remote execution, payload delivery and destructive encryption behavior across at least two workstations.

## Recommended Response Actions

1. **Containment:** Isolate `WKSTN-02` and `WKSTN-03` from the network to stop further lateral movement and encryption.
2. **Credential response:** Disable or reset `anna.jones` credentials and review authentication activity for secondary compromise.
3. **Network blocking:** Block `7zipp.org`, `206.189.34.218`, and the observed payload URL at DNS, proxy, firewall and perimeter controls where appropriate.
4. **Host remediation:** Quarantine/remove `bomb.exe` and related artifacts; rebuild systems where integrity cannot be assured.
5. **Forensic scoping:** Search the estate for `Invoke-SharpKatz`, `mimikatz.exe`, `777bomb.exe`, `bomb.exe`, `.777zzz`, PowerShell remoting and related process chains.
6. **Persistence review:** Inspect scheduled tasks, services, registry run keys, PowerShell profiles and remote-management configuration.
7. **Impact validation:** Determine the full set of encrypted files and confirm the integrity and availability of backups.
8. **Exfiltration review:** Examine proxy, DNS, firewall and endpoint telemetry for evidence of outbound data transfer before encryption.
9. **Recovery:** Restore clean systems and validated backups only after containment and credential remediation are complete.
10. **Detection engineering:** Create detections for SharpKatz/Mimikatz-like behavior, suspicious `Invoke-Command`, remote PowerShell, anomalous tool transfer and mass file modifications.

## Detection Opportunities

Potential monitoring and detection logic should focus on:

- PowerShell Script Block content containing `Invoke-SharpKatz`
- PowerShell `Invoke-Command` targeting remote workstations
- Event IDs `4103` and `4104` associated with suspicious PowerShell content
- Downloads of executable content through PowerShell web requests
- New process creation for unexpected executables in user profile directories
- High-volume file modification/rename activity
- Creation of unusual extensions such as `.777zzz`
- Credential-access tooling followed quickly by remote execution
- Same-user activity across multiple hosts in a short period
- Connections or requests involving `7zipp.org` or `206.189.34.218`

## Analyst Assessment

The strongest element of this investigation was the ability to correlate isolated telemetry into a coherent intrusion narrative. Credential-access evidence, remote PowerShell execution, external payload retrieval, remote process creation and mass encryption were not treated as independent events; they were linked by host, user, timestamp and attacker objective.

The investigation demonstrates practical threat-hunting skills in hypothesis validation, SIEM pivoting, event correlation, IOC extraction, MITRE ATT&CK mapping, scoping and incident-response recommendation. The final assessment intentionally distinguishes three evidence classes:

- **Observed telemetry** — events directly present in Elastic/Winlogbeat data.
- **Provided threat intelligence** — IOC leads supplied by the simulation.
- **Analyst inference** — conclusions derived by correlating host, user, command and timing evidence.

This distinction keeps the report technically defensible and avoids overstating certainty.

## Evidence

The investigation screenshots were curated by evidentiary value. Screenshots that only show simulator scoring or an incomplete-lab status are intentionally excluded because they do not add technical evidence to the case study.

### Evidence Register

| Ref | Evidence | Source Screenshot | Portfolio Use |
|---|---|---|---|
| E01 | Threat-hunting hypothesis and mission objectives | Threat Intel / Mission screens | Establishes investigation scope and methodology |
| E02 | External IOC list: `7zipp.org`, `206.189.34.218`, extensions, filenames and hashes | IOC screenshots | Documents starting intelligence pivots |
| E03 | `Invoke-SharpKatz` PowerShell Script Block evidence on `WKSTN-02` | Elastic Event `4104` screenshot | Supports Credential Access / `T1003` finding |
| E04 | `7zipp.org` / `777bomb.exe` retrieval telemetry | Elastic query screenshots | Supports payload delivery / `T1105` finding |
| E05 | `bomb.exe` process creation on `WKSTN-03` at `15:41:51.778` | Elastic process query screenshot | Confirms payload execution on remote host |
| E06 | Event `4103` `Encrypting:` telemetry on `WKSTN-02` at `15:45:12.177` | Elastic filtered query screenshot | Supports ransomware impact / `T1486` |
| E07 | `bomb.exe` plus `.777zzz` filename evidence | Elastic query screenshot | Corroborates payload/encryption linkage |
| E08 | Timeline stages for credential dumping, remote PowerShell, payload transfer and encryption | Simulator timeline screenshots | Shows structured attack-chain reconstruction |
| E09 | Hypothesis marked **Proven** and attack-chain report view | Threat Report screenshot | Demonstrates final analytical disposition |
| E10 | Threat Case Report executive summary | Final report screenshot | Provides recruiter-facing executive narrative |
| E11 | IOC table and Impact & Findings recommendations | Final report screenshots | Demonstrates reporting and response recommendations |

### Curated Screenshot Filenames Received

The following high-value screenshots have been selected for eventual placement in the case evidence folder:

- `Screenshot 2026-08-24 123406(1).png` — exact Event `4103` encryption hit at `15:45:12.177`
- `Screenshot 2026-08-24 135104(1).png` — `bomb.exe` process creation on `WKSTN-03`
- `Screenshot 2026-08-24 140531(1).png` — Mass File Encryption timeline stage
- `Screenshot 2026-08-24 141139(1).png` — SharpKatz Credential Dumping timeline stage
- `Screenshot 2026-08-24 141428(1).png` — Remote PowerShell Execution timeline stage
- `Screenshot 2026-08-24 142920(1).png` — hypothesis marked Proven / attack-chain view
- `Screenshot 2026-08-24 143002(1).png` — Threat Case Report executive summary

> Binary screenshot upload is handled separately from the Markdown report. The evidence register and captions are already prepared so the images can be inserted without rewriting the case narrative.

## Portfolio Remark

This case demonstrates the analyst's ability to move beyond single-alert triage and conduct a structured threat hunt across multiple telemetry sources. The investigation correlates credential access, PowerShell abuse, lateral movement, payload transfer, process execution and ransomware impact into a defensible incident narrative, then translates those findings into ATT&CK mappings, IOCs, containment priorities and detection opportunities.

The key professional takeaway is not merely identification of `bomb.exe`; it is the demonstrated ability to **pivot from hypothesis to evidence, correlate activity across hosts, validate attacker progression, scope affected entities, and communicate actionable findings**.

## Skills Demonstrated

`Threat Hunting` · `Elastic/Kibana` · `Winlogbeat` · `PowerShell Analysis` · `Windows Event Analysis` · `Process Creation Analysis` · `Credential Access Investigation` · `Lateral Movement Analysis` · `Ransomware Investigation` · `IOC Extraction` · `MITRE ATT&CK Mapping` · `Incident Scoping` · `Containment & Remediation` · `Detection Engineering` · `Security Reporting`

---

[← Back to SOC Portfolio](../)

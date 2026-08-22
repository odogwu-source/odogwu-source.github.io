# SOC Case 02 — Correlated PowerShell, Reconnaissance & Suspected DNS Exfiltration

> **Portfolio case study:** Educational TryHackMe SOC simulation. This case demonstrates SIEM investigation, event correlation, analyst judgement and incident documentation. It is not presented as a production incident from a real organisation.

## Executive Summary
A sequence of six related alerts on Windows endpoint `win-3450` was investigated as a correlated incident rather than as isolated detections. Sysmon and PowerShell telemetry showed PowerShell activity from the user's Downloads directory, creation/execution of `PowerView.ps1`, `net.exe` network-drive activity, creation of an `exfiltration` ZIP archive, and repeated PowerShell-spawned `nslookup.exe` queries to the `haz4rdw4re.io` domain using long encoded-looking subdomains.

The combined evidence supports a **True Positive** classification and escalation. Individual Windows utilities such as PowerShell, `net.exe` and `nslookup.exe` can be legitimate; however, their sequence, parent-child relationships, working directories, archive creation and repeated unusual DNS queries form a materially suspicious attack chain consistent with reconnaissance, staging and suspected DNS-based data exfiltration.

## Environment / Tools
- TryHackMe SOC simulation
- Splunk SIEM
- Sysmon telemetry
- PowerShell logging
- Process and parent-child correlation
- DNS / command-line analysis
- Alert triage and case reporting

## Affected Entity
- **Host:** `win-3450`
- **Observed user-path context:** `C:\Users\michael.ascot\Downloads\`
- **Key parent process:** `powershell.exe` — PID `3728`

## Investigation Timeline
The correlated evidence places the suspicious activity within the 22 August 2026 simulation window, with key events around 12:40–12:47 in the underlying telemetry. The alerts were analysed together because they share the same host, PowerShell ancestry and closely related working-path context.

## Correlated Alert Cases

### Alert 1 — PowerShell / Network Share Activity
**Classification: True Positive**

PowerShell activity was correlated with Windows networking commands and access to a sensitive network-share context. This established the initial suspicious execution chain and provided the parent process (`powershell.exe`, PID `3728`) used to correlate later events.

**Analyst rationale:** PowerShell alone is not malicious, but its subsequent child processes and associated activity justified further investigation.

### Alert 2 — PowerShell Script Created in Downloads
**Classification: True Positive**

Sysmon Event ID 11 recorded PowerShell creating a script in the user's Downloads directory. The investigation subsequently identified `PowerView.ps1` activity. PowerView is commonly used for Active Directory/domain reconnaissance and is highly relevant when seen inside an already suspicious PowerShell chain.

**Observed evidence:**
- PowerShell execution
- File creation in `Downloads`
- `PowerView.ps1`
- PowerShell script-block telemetry

**Analyst rationale:** The script evidence strengthened the hypothesis that the activity represented reconnaissance rather than routine user behaviour.

### Alert 3 — Suspicious Parent-Child Relationship
**Classification: True Positive**

A high-severity alert showed `powershell.exe` (PID `3728`) spawning `nslookup.exe`. The `nslookup.exe` command line queried a long encoded-looking subdomain under `haz4rdw4re.io`, and the working directory referenced `Downloads\exfiltration`.

**Analyst rationale:** `nslookup.exe` is a legitimate DNS utility, but repeated execution from PowerShell using unusual high-entropy subdomains in an exfiltration-themed working directory is inconsistent with ordinary DNS troubleshooting.

### Alert 4 — PowerView / Reconnaissance Activity
**Classification: True Positive**

PowerShell script-block telemetry showed execution of `PowerView.ps1`. The script content contained host/DNS-related enumeration logic, supporting reconnaissance activity within the same incident chain.

**Analyst rationale:** In isolation, an administrative script may be authorised. Here, its temporal and process correlation with archive creation and suspicious DNS queries materially increases the likelihood of malicious use.

### Alert 5 — Network Drive Disconnected
**Classification: True Positive in correlated incident context**

Sysmon process telemetry recorded:

`C:\Windows\system32\net.exe use Z: /delete`

The process was spawned by `powershell.exe` PID `3728` from the user's Downloads context.

**Analyst rationale:** Disconnecting a mapped drive can be legitimate. In this incident, the command is significant because it occurs within the same PowerShell-led sequence associated with network-share activity, staging and suspected exfiltration. It may represent cleanup after accessing a mapped resource; that interpretation remains an analytical hypothesis rather than a separately proven fact.

### Alert 6 — Repeated Suspicious DNS Queries / Suspected DNS Exfiltration
**Classification: True Positive — Escalate**

The SIEM showed multiple `nslookup.exe` command lines querying different long encoded-looking subdomains of `haz4rdw4re.io`. The queries were spawned by PowerShell PID `3728`. Related telemetry also showed creation of an archive at:

`C:\Users\michael.ascot\Downloads\exfiltration\exfiltemp.zip`

**Analyst rationale:** Repeated DNS queries containing changing high-entropy labels, combined with a staged ZIP archive and an `exfiltration` working path, are strongly suspicious and consistent with DNS-tunnelling/data-exfiltration behaviour. The evidence supports escalation while avoiding the unsupported claim that the exact contents of exfiltrated data were proven from the available telemetry.

## 5Ws Analyst Summary
- **Who:** Activity occurred in the user/session context represented by `C:\Users\michael.ascot\` on `win-3450`. Authentication telemetry would be required to attribute the activity conclusively to a person.
- **What:** PowerShell-led activity included PowerView execution, network-drive operations, ZIP archive creation and repeated `nslookup.exe` queries using encoded-looking subdomains.
- **When:** 22 August 2026 during the correlated SOC simulation event window, approximately 12:40–12:47 based on the displayed telemetry.
- **Where:** Endpoint `win-3450`, primarily under the user's Downloads and `Downloads\exfiltration` paths, with DNS queries directed toward `haz4rdw4re.io`.
- **Why:** The sequence is inconsistent with ordinary user activity and is consistent with a reconnaissance → staging → suspected exfiltration chain. Exact attacker intent and transferred content require additional evidence.

## Reason for Escalation
Escalation is warranted because multiple independent detections converge on one endpoint and one PowerShell process lineage. The combination of reconnaissance tooling, sensitive/network-resource activity, archive staging and repeated suspicious DNS queries creates a credible risk of compromise and data loss that exceeds routine L1 triage.

## Recommended Remediation Actions
1. Isolate `win-3450` if activity is ongoing or containment criteria are met.
2. Preserve Sysmon, PowerShell, DNS, authentication, file-access and network telemetry.
3. Terminate or contain confirmed malicious PowerShell/process activity in accordance with incident-response procedures.
4. Block or sinkhole the suspicious domain/indicators after validation and hunt for the same indicators across the environment.
5. Determine which files were accessed, copied or placed into `exfiltemp.zip`.
6. Review mapped-drive and sensitive-share access associated with the user/session.
7. Hunt for `PowerView.ps1`, matching PowerShell command lines and similar `nslookup.exe` parent-child relationships on other endpoints.
8. Reset/revoke affected credentials if compromise is confirmed or strongly suspected.
9. Escalate to incident response for full scoping, containment, eradication and recovery.

## Attack Indicators
- Host: `win-3450`
- Parent process: `powershell.exe`
- Correlated parent PID: `3728`
- Child processes: `nslookup.exe`, `net.exe`
- Script: `PowerView.ps1`
- Suspicious directory: `C:\Users\michael.ascot\Downloads\exfiltration\`
- Staged archive: `exfiltemp.zip`
- Suspicious domain: `haz4rdw4re.io`
- Behaviour: repeated DNS queries with changing encoded/high-entropy-looking subdomains
- Command: `net.exe use Z: /delete`

## MITRE ATT&CK Context
Potentially relevant ATT&CK context includes PowerShell/command scripting, account/domain or network reconnaissance, archive collection/staging, Windows network-share activity and application-layer/DNS-based exfiltration. Technique mapping is presented as analytical context; mappings should not be interpreted as proof of an attacker objective beyond the observed telemetry.

## Skills Demonstrated
SIEM investigation; Splunk searching; Sysmon analysis; PowerShell investigation; script-block analysis; parent-child process correlation; DNS anomaly analysis; IOC identification; alert correlation; True Positive classification; 5Ws reporting; escalation reasoning; containment/remediation planning; evidence-based incident documentation.

## Evidence Plan
The original simulation screenshots should be curated into an `evidence/` directory and mapped to the six alert sections above. Priority screenshots are: alert details, PowerShell/file creation, PowerView script-block telemetry, suspicious parent-child process evidence, `net.exe` drive activity, ZIP creation, repeated `nslookup.exe` queries, and the final True Positive case report.

## Lessons Learned
The strongest signal in this investigation came from **correlation**, not from any single command. Legitimate Windows utilities became high-confidence indicators when process lineage, timing, file staging, reconnaissance tooling and DNS behaviour were considered together. The case also demonstrates the importance of separating confirmed telemetry from hypotheses about attacker intent or exact data loss.

## Disclaimer
This case study is based on an educational TryHackMe SOC simulation. Hostnames, user paths, timestamps, events and findings are training-derived and do not describe a real client or production compromise.
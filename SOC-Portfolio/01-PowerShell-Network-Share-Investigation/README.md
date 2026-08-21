# SOC Case 01 — Suspicious PowerShell & Network Share Activity

> **Portfolio case study:** Educational SOC simulation based on hands-on alert investigation. It demonstrates analyst reasoning and documentation; it is not a production incident from a real organisation.

## Executive Summary
This case study documents the investigation of suspicious Windows activity involving PowerShell, a child `nslookup.exe` process and access to a financial-records network share. The alert was treated as a **True Positive** because the observed process chain and network-share activity were inconsistent with normal user behaviour and indicated potentially malicious discovery/staging or exfiltration-related activity.

## Alert Context
- **Host:** `win-3450`
- **Parent process:** PowerShell (`PID 3728`)
- **Child process:** `nslookup.exe`
- **Suspicious working-path context:** `Downloads\exfiltration`
- **Correlated activity:** `net.exe` mapping/accessing `\\FILESRV-01\SSF-FinancialRecords`
- **Classification:** True Positive

## Investigation Objectives
- Validate whether the alert represented genuine suspicious activity.
- Reconstruct the process and command context.
- Correlate endpoint events with network-share access.
- Determine likely scope and business impact.
- Produce an analyst conclusion without overstating unconfirmed behaviour.

## Environment / Tools
TryHackMe SOC simulation; Windows/Sysmon-style endpoint telemetry; process-event analysis; alert correlation; analyst case documentation.

## Methodology
1. Reviewed the triggering alert and affected endpoint.
2. Examined parent/child process relationships.
3. Identified PowerShell spawning `nslookup.exe`.
4. Reviewed path/context indicators associated with the activity.
5. Correlated the endpoint activity with `net.exe` access to a sensitive financial-records share.
6. Assessed whether the evidence supported malicious or benign behaviour.
7. Classified the alert and documented scope, impact and recommended response.

## Investigation / Analysis
PowerShell is a legitimate administrative utility, and `nslookup.exe` is a legitimate DNS troubleshooting tool. Their presence alone is therefore insufficient to establish malicious activity. The risk increased materially when the process relationship was combined with the suspicious `Downloads\exfiltration` context and access to `\\FILESRV-01\SSF-FinancialRecords`.

The evidence supports suspicious activity involving a sensitive network share and tools that can be abused during discovery or data movement. However, the available evidence does **not** by itself prove that DNS exfiltration actually occurred. The analyst conclusion therefore distinguishes observed facts from hypothesis.

## 5Ws Analyst Summary
- **Who:** Activity associated with the user/session operating on `win-3450`; exact user identity should be confirmed from authentication/session telemetry.
- **What:** PowerShell spawned `nslookup.exe`, with correlated `net.exe` activity accessing a financial-records share.
- **When:** During the alert/event window recorded by the SOC simulation.
- **Where:** Endpoint `win-3450` and network share `\\FILESRV-01\SSF-FinancialRecords`.
- **Why:** Behaviour is suspicious because of the process chain, exfiltration-themed path context and access to sensitive financial records. Further investigation is required to establish intent and whether data left the environment.

## Key Findings
- Suspicious PowerShell execution was confirmed.
- `nslookup.exe` was spawned from the PowerShell process chain.
- Sensitive financial-records share access was correlated with the endpoint activity.
- The total evidence justified a True Positive classification.
- DNS exfiltration remained a hypothesis rather than a confirmed fact.

## Recommended Response
- Isolate or contain the endpoint if the activity is ongoing or otherwise meets the organisation's containment threshold.
- Preserve endpoint, PowerShell, DNS, authentication and file-access telemetry.
- Identify the user/session responsible for the activity.
- Review access to the financial-records share and determine which files were accessed or copied.
- Hunt for similar PowerShell, `nslookup.exe`, share-access and suspicious DNS activity across other hosts.
- Reset/revoke credentials if compromise is confirmed or strongly suspected.
- Escalate for deeper incident response if evidence indicates data staging, lateral movement or exfiltration.

## Evidence
Evidence for this project will be curated from the original SOC simulation screenshots. Screenshots should demonstrate the alert, process relationship, relevant commands/paths, network-share correlation and final analyst classification without exposing unnecessary or misleading artefacts.

## Skills Demonstrated
Alert triage; Sysmon/process analysis; parent-child process correlation; PowerShell investigation; Windows network-share analysis; evidence correlation; True Positive classification; 5Ws documentation; incident scoping; remediation reasoning.

## MITRE ATT&CK Context
Observed behaviour may be analysed against relevant ATT&CK techniques such as command/scripting interpreter use, network-share discovery/access and DNS-related activity. Technique mapping should be treated as analytical context unless the specific behaviour required by a technique is directly evidenced.

## Lessons Learned
Legitimate tools become meaningful indicators when analysed in context. Strong SOC conclusions separate **what the telemetry proves** from **what the analyst suspects**, preventing an investigation from overstating exfiltration or attacker intent.

## Disclaimer
This case study is based on an educational SOC simulation. Hostnames, paths, events and findings are training-derived. It does not describe a real organisation, production compromise or client incident.
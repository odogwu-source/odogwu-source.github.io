# SOC Case 01 — Foundational PowerShell & Network Share Investigation

> **Foundational portfolio case:** Educational SOC simulation documenting an earlier stage of the analyst workflow. A later, evidence-rich investigation in this portfolio expands the same host activity into a multi-alert incident reconstruction.

## Purpose of This Case

This case is retained as a **focused foundational investigation** rather than presented as a separate unrelated incident. It demonstrates the initial analytical reasoning used to assess suspicious PowerShell, `nslookup.exe` and sensitive network-share activity before the broader multi-stage endpoint case was reconstructed from additional alerts and evidence.

For the complete correlated investigation with screenshot evidence, PowerView reconnaissance, data staging, drive cleanup and suspected DNS-based exfiltration, see:

[Open the Multi-Stage Endpoint Compromise investigation](../02-Multi-Stage-Endpoint-Compromise/)

## Executive Summary

The investigation examined suspicious Windows activity involving PowerShell, a child `nslookup.exe` process and access to a financial-records network share. The alert was treated as a **True Positive** because the combined process chain and sensitive-share activity were inconsistent with expected user behaviour and justified deeper investigation.

At this stage of the analysis, the evidence supported suspicious discovery/staging or exfiltration-related activity but did **not**, by itself, prove that DNS exfiltration had occurred. This case therefore demonstrates disciplined separation of observed telemetry from analytical hypothesis.

## Alert Context

- **Host:** `win-3450`
- **Parent process:** PowerShell (`PID 3728`)
- **Child process:** `nslookup.exe`
- **Suspicious working-path context:** `Downloads\exfiltration`
- **Correlated activity:** `net.exe` mapping/accessing `\\FILESRV-01\SSF-FinancialRecords`
- **Classification:** **True Positive**

## Investigation Objectives

1. Validate whether the alert represented genuine suspicious activity.
2. Reconstruct the process and command context.
3. Correlate endpoint events with network-share access.
4. Determine likely scope and business impact.
5. Produce an analyst conclusion without overstating unconfirmed behaviour.

## Environment / Tools

TryHackMe SOC simulation; Windows/Sysmon-style endpoint telemetry; process-event analysis; alert correlation; analyst case documentation.

## Methodology

1. Reviewed the triggering alert and affected endpoint.
2. Examined parent/child process relationships.
3. Identified PowerShell spawning `nslookup.exe`.
4. Reviewed path/context indicators associated with the activity.
5. Correlated endpoint activity with `net.exe` access to a sensitive financial-records share.
6. Assessed whether the evidence supported malicious or benign behaviour.
7. Classified the alert and documented scope, impact and recommended response.

## Investigation / Analysis

PowerShell is a legitimate administrative utility, and `nslookup.exe` is a legitimate DNS troubleshooting tool. Their presence alone is insufficient to establish malicious activity. Risk increased materially when the process relationship was combined with the suspicious `Downloads\exfiltration` context and access to `\\FILESRV-01\SSF-FinancialRecords`.

The evidence supported suspicious activity involving a sensitive network share and dual-use tools that can be abused during discovery or data movement. At this stage, however, the available evidence did **not** independently prove DNS exfiltration. The analyst conclusion therefore distinguishes observed facts from hypothesis.

## 5Ws Analyst Summary

**Who:** Activity associated with the user/session operating on `win-3450`; exact user identity required confirmation from authentication/session telemetry.

**What:** PowerShell spawned `nslookup.exe`, with correlated `net.exe` activity accessing a financial-records share.

**When:** During the alert/event window recorded by the SOC simulation.

**Where:** Endpoint `win-3450` and network share `\\FILESRV-01\SSF-FinancialRecords`.

**Why:** Behaviour was suspicious because of the process chain, exfiltration-themed path context and access to sensitive financial records. Further investigation was required to establish intent and whether data left the environment.

## Key Findings

- Suspicious PowerShell execution was confirmed.
- `nslookup.exe` was spawned from the PowerShell process chain.
- Sensitive financial-records share access was correlated with the endpoint activity.
- The combined evidence justified a True Positive classification.
- At this investigation stage, DNS exfiltration remained a hypothesis requiring additional evidence.

## Recommended Response

- Isolate or contain the endpoint if activity is ongoing or meets the organisation's containment threshold.
- Preserve endpoint, PowerShell, DNS, authentication and file-access telemetry.
- Identify the user/session responsible for the activity.
- Review access to the financial-records share and determine which files were accessed or copied.
- Hunt for similar PowerShell, `nslookup.exe`, share-access and suspicious DNS activity across other hosts.
- Reset or revoke credentials if compromise is confirmed or strongly suspected.
- Escalate for deeper incident response if evidence indicates data staging, lateral movement or exfiltration.

## Relationship to the Featured Investigation

Subsequent investigation produced additional evidence across multiple alerts, including archive/data staging, PowerView reconnaissance, drive disconnection and repeated encoded-looking DNS queries. Those findings are documented in the featured **Multi-Stage Endpoint Compromise** case rather than duplicated here.

This structure shows progression from a focused alert investigation to broader incident correlation—an important SOC analyst skill.

## Skills Demonstrated

Alert triage; Sysmon/process analysis; parent-child process correlation; PowerShell investigation; Windows network-share analysis; evidence correlation; True Positive classification; 5Ws documentation; incident scoping; remediation reasoning; hypothesis management.

## MITRE ATT&CK Context

Observed behaviour may be analysed against relevant ATT&CK techniques such as command/scripting interpreter use, network-share discovery/access and DNS-related activity. Technique mapping should be treated as analytical context unless the specific behaviour required by a technique is directly evidenced.

## Lessons Learned

Legitimate tools become meaningful indicators when analysed in context. Strong SOC conclusions separate **what telemetry proves** from **what the analyst suspects**. The later multi-alert investigation demonstrates how additional evidence can strengthen or refine an initial hypothesis.

> **Lab disclaimer:** This case study is based on an educational SOC simulation. Hostnames, paths, events and findings are training-derived and do not describe a real organisation, production compromise or client incident.

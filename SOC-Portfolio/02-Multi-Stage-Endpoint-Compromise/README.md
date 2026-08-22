# Endpoint & SIEM Investigation

## Multi-Stage Endpoint Compromise — PowerShell, Reconnaissance, Data Staging & Suspected DNS Exfiltration

**Platform:** TryHackMe SOC simulation  
**Date:** 22 August 2026  
**Primary host:** `win-3450`  
**SIEM:** Splunk Enterprise  
**Telemetry:** Sysmon, PowerShell and email events

## Executive Summary

This project documents six SOC alerts investigated during a controlled security simulation. Each alert is preserved as an individual analyst case while related endpoint alerts are correlated into a broader incident chain. The investigation demonstrates phishing triage, SIEM searching, process-tree analysis, PowerShell investigation, network-share analysis, data staging, Active Directory reconnaissance, suspicious DNS activity, incident classification, escalation and remediation planning.

## Case Register

| Case | Alert | Severity | Classification | Escalation |
|---|---|---|---|---|
| SOC 1 | Suspicious External Email / Phishing — Alert 1000 | Low | True Positive | No |
| SOC 2 | Network Drive Mapping & Suspicious Data Staging — Alert 1022 | Medium | True Positive | Yes |
| SOC 3 | Repeated Nslookup / Suspected DNS Exfiltration — Alert 1025 | High | True Positive | Yes |
| SOC 4 | PowerView PowerShell Reconnaissance — Alert 1020 | Low | True Positive | Yes |
| SOC 5 | Network Drive Disconnection Following Data Staging — Alert 1024 | Medium | True Positive | Yes |
| SOC 6 | Suspicious Parent-Child / DNS Exfiltration Activity — Alert 1027 | High | True Positive | Yes |

## SOC 1 — Suspicious External Email / Phishing

An unsolicited inbound email from `eileen@trendymillineryco.me` used a fraudulent inheritance claim to solicit banking details from `support@tryhatme.com`. SIEM searches confirmed the suspicious message. No attachment was present and the available evidence did not show recipient interaction or disclosure of information, supporting True Positive classification without escalation.

**Indicators:** `eileen@trendymillineryco.me`, `trendymillineryco.me`, inheritance-themed social engineering subject.

**Response:** Block sender/domain, quarantine or delete the message, notify the user, reinforce phishing awareness and monitor for related indicators.

## SOC 2 — Network Drive Mapping & Suspicious Data Staging

Sysmon telemetry showed `powershell.exe` PID `3728` spawning `net.exe` to map `Z:` to `\\FILESRV-01\SSF-FinancialRecords`. The same PowerShell lineage was associated with creation of a ZIP archive under `C:\Users\michael.ascot\Downloads\exfiltration\`, and the drive was later removed. Correlation made this suspicious access/staging activity rather than routine administration.

**Response:** Isolate `win-3450`, investigate the complete PowerShell process tree, preserve and analyse the archive, identify files accessed through the share, review authentication/network telemetry and determine whether data left the environment.

## SOC 3 — Repeated Nslookup / Suspected DNS Exfiltration

PowerShell PID `3728` repeatedly spawned `nslookup.exe`. Splunk analysis identified ten distinct command lines containing encoded-looking strings as subdomains of `haz4rdw4re.io`. Correlation with staged archive activity was consistent with suspected DNS-based data exfiltration.

**Indicators:** `powershell.exe` PID `3728`, `nslookup.exe`, `haz4rdw4re.io`, repeated encoded-looking DNS queries, staged archive.

**Response:** Isolate the endpoint, block the suspicious domain, preserve telemetry and artifacts, investigate staged data, hunt across other systems and perform endpoint forensic analysis.

## SOC 4 — PowerView PowerShell Reconnaissance

`PowerView.ps1` was created under `C:\Users\michael.ascot\Downloads\` and PowerShell script-block telemetry showed associated execution activity. PowerView is a dual-use Active Directory reconnaissance framework; within the surrounding suspicious endpoint activity, its execution warranted escalation.

**Response:** Isolate the endpoint, terminate suspicious PowerShell activity, quarantine the script, determine its origin, review PowerShell/Sysmon/authentication telemetry and scope reconnaissance activity.

## SOC 5 — Network Drive Disconnection Following Data Staging

Sysmon recorded `net.exe` PID `8004`, spawned by PowerShell PID `3728`, executing `net.exe use Z: /delete`. In isolation this command may be legitimate, but correlation with the mapped financial-records share and staged archive made it significant as part of the suspicious sequence.

**Response:** Preserve the staged archive, determine what was accessed through `Z:`, review network/share and authentication logs, contain the endpoint and investigate possible exfiltration.

## SOC 6 — Suspicious Parent-Child / DNS Exfiltration Activity

Sysmon identified `powershell.exe` PID `3728` spawning `nslookup.exe` PID `5432` from `C:\Users\michael.ascot\Downloads\exfiltration\`. The child process queried an encoded-looking subdomain of `haz4rdw4re.io`. Together with repeated DNS queries and staging evidence, this supported True Positive classification and escalation for suspected DNS-based exfiltration.

**Response:** Immediately isolate `win-3450`, terminate malicious PowerShell activity, block `haz4rdw4re.io`, preserve logs/artifacts, investigate staged data and hunt for related activity across the environment.

## Correlated Incident Chain

`PowerShell → Network share mapping → Data staging/archive creation → Drive cleanup → PowerView reconnaissance → Repeated nslookup/DNS activity`

The principal analyst lesson is that alert severity must be interpreted in context. Low- and medium-severity detections became materially more significant after process ancestry, file events, working directories, command lines and DNS activity were correlated.

## Skills Demonstrated

Splunk SIEM investigation; Sysmon analysis; PowerShell/script-block analysis; parent-child process correlation; network-share investigation; Active Directory reconnaissance detection; data-staging identification; DNS anomaly analysis; IOC documentation; True Positive classification; 5Ws reporting; escalation reasoning; containment and remediation planning; multi-alert incident reconstruction.

## Evidence

The evidence package contains 37 screenshots separated into SOC 1–SOC 6 and ordered chronologically within each investigation. Once uploaded to this project, each image will be embedded beside the corresponding investigative step with an analyst caption explaining what the evidence establishes.

> **Lab disclaimer:** This project documents hands-on work completed in a controlled TryHackMe SOC simulation. The systems, identities, domains and telemetry shown are training evidence and do not represent a real production compromise.
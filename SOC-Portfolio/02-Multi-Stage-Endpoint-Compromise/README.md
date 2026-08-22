# Endpoint & SIEM Investigation

## Multi-Stage Endpoint Compromise — PowerShell, Reconnaissance, Data Staging & Suspected DNS Exfiltration

**Platform:** TryHackMe SOC simulation  
**Date:** 22 August 2026  
**Primary host:** `win-3450`  
**SIEM:** Splunk Enterprise  
**Telemetry:** Sysmon, PowerShell and email events

## Executive Summary

This project documents a six-alert SOC investigation completed in a controlled security simulation. Each alert is treated as an individual analyst case while related endpoint detections are correlated into a broader incident chain. The work demonstrates phishing triage, SIEM searching, process-tree analysis, PowerShell investigation, network-share analysis, data staging, Active Directory reconnaissance, suspicious DNS activity, incident classification, escalation decisions and remediation planning.

## Case Register

| Case | Alert | Severity | Classification | Escalation |
|---|---|---|---|---|
| SOC 1 | Suspicious External Email / Phishing — Alert 1000 | Low | True Positive | No |
| SOC 2 | Network Drive Mapping & Suspicious Data Staging — Alert 1022 | Medium | True Positive | Yes |
| SOC 3 | Repeated Nslookup / Suspected DNS Exfiltration — Alert 1025 | High | True Positive | Yes |
| SOC 4 | PowerView PowerShell Reconnaissance — Alert 1020 | Low | True Positive | Yes |
| SOC 5 | Network Drive Disconnection Following Data Staging — Alert 1024 | Medium | True Positive | Yes |
| SOC 6 | Suspicious Parent-Child / DNS Exfiltration Activity — Alert 1027 | High | True Positive | Yes |

---

## SOC 1 — Suspicious External Email / Phishing

An unsolicited inbound email from `eileen@trendymillineryco.me` used a fraudulent inheritance claim to solicit banking details from `support@tryhatme.com`. SIEM searches confirmed the suspicious message. No attachment was present and available evidence did not show recipient interaction or disclosure of information, supporting **True Positive** classification without escalation.

**Indicators:** `eileen@trendymillineryco.me`, `trendymillineryco.me`, inheritance-themed social engineering subject.  
**Response:** Block sender/domain, quarantine or delete the message, notify the user, reinforce phishing awareness and monitor for related indicators.

### Evidence — Investigation Sequence

**1. Initial phishing alert** — Alert queue evidence initiating triage.
![SOC 1 initial alert](SOC01-01-initial-alert.png)

**2. Subject-based SIEM search** — Search used to validate matching email telemetry.
![SOC 1 subject search](SOC01-02-subject-search.png)

**3. Sender-based SIEM search** — Pivot on the sender for related activity.
![SOC 1 sender search](SOC01-03-sender-search.png)

**4. Recipient pivot** — Recipient-focused search used to scope additional activity.
![SOC 1 recipient pivot](SOC01-04-recipient-pivot-no-result.png)

**5. Case report preparation** — Analyst documentation of findings.
![SOC 1 case report](SOC01-05-case-report-overview.png)

**6. True Positive classification** — Final classification based on confirmed phishing characteristics.
![SOC 1 true positive](SOC01-06-true-positive-report.png)

**7. Remediation and escalation decision** — Remediation documented without escalation because endpoint compromise was not established.
![SOC 1 remediation](SOC01-07-remediation-no-escalation.png)

---

## SOC 2 — Network Drive Mapping & Suspicious Data Staging

Sysmon telemetry showed `powershell.exe` PID `3728` spawning `net.exe` to map `Z:` to `\\FILESRV-01\SSF-FinancialRecords`. The same PowerShell lineage was associated with creation of a ZIP archive under `C:\Users\michael.ascot\Downloads\exfiltration\`. Correlation made this suspicious access and staging activity rather than routine administration.

**Response:** Isolate `win-3450`, investigate the complete PowerShell process tree, preserve and analyse the archive, identify files accessed through the share, review authentication/network telemetry and determine whether data left the environment.

### Evidence — Investigation Sequence

**1. Alert 1022 / case evidence** — Network-share activity identified for investigation.
![SOC 2 alert 1022](SOC02-01-case-report-alert-1022.png)

**2. Network-share alert details** — Alert telemetry establishes the mapped-share activity and endpoint context.
![SOC 2 network share details](SOC02-02-alert-details-network-share.png)

**3. Splunk process investigation** — SIEM pivot used to correlate `net.exe`, PowerShell and host activity.
![SOC 2 process investigation](SOC02-03-splunk-process-investigation.png)

**4. Parent-process correlation** — PowerShell PID `3728` tied to the suspicious network-share sequence.
![SOC 2 parent process](SOC02-04-parent-process-correlation.png)

**5. Data-staging evidence** — Archive creation under the exfiltration working directory supports staging assessment.
![SOC 2 data staging](SOC02-05-data-staging-archive.png)

**6. Analyst report / True Positive decision** — Findings documented with containment and escalation rationale.
![SOC 2 report](SOC02-06-true-positive-escalation.png)

---

## SOC 3 — Repeated Nslookup / Suspected DNS Exfiltration

PowerShell PID `3728` repeatedly spawned `nslookup.exe`. Splunk analysis identified ten distinct command lines containing encoded-looking strings as subdomains of `haz4rdw4re.io`. Correlation with staged archive activity was consistent with suspected DNS-based data exfiltration.

**Indicators:** `powershell.exe` PID `3728`, `nslookup.exe`, `haz4rdw4re.io`, repeated encoded-looking DNS queries, staged archive.  
**Response:** Isolate the endpoint, block the suspicious domain, preserve telemetry and artifacts, investigate staged data, hunt across other systems and perform endpoint forensic analysis.

### Evidence — Investigation Sequence

**1. Initial high-severity alert 1025** — Suspicious PowerShell-to-`nslookup.exe` parent-child relationship.
![SOC 3 initial alert](SOC03-01-initial-alert-1025.png)

**2. Nslookup SIEM search** — Host-scoped Splunk search identifies repeated `nslookup.exe` activity.
![SOC 3 nslookup search](SOC03-02-nslookup-search.png)

**3. Parent PID filter** — PID correlation ties DNS activity back to PowerShell PID `3728`.
![SOC 3 parent PID](SOC03-03-parent-pid-filter.png)

**4. Ten distinct DNS queries** — Command-line statistics reveal repeated encoded-looking subdomains of `haz4rdw4re.io`.
![SOC 3 DNS queries](SOC03-04-ten-distinct-dns-queries.png)

**5. True Positive report** — Analyst classification based on correlated endpoint and DNS evidence.
![SOC 3 true positive](SOC03-05-true-positive-report.png)

**6. IOC documentation and escalation** — Indicators and response actions documented for escalation.
![SOC 3 escalation](SOC03-06-iocs-escalation.png)

---

## SOC 4 — PowerView PowerShell Reconnaissance

`PowerView.ps1` was created under `C:\Users\michael.ascot\Downloads\` and PowerShell script-block telemetry showed associated execution activity. PowerView is a dual-use Active Directory reconnaissance framework; within the surrounding suspicious endpoint activity, its execution warranted escalation.

**Response:** Isolate the endpoint, terminate suspicious PowerShell activity, quarantine the script, determine its origin, review PowerShell/Sysmon/authentication telemetry and scope reconnaissance activity.

### Evidence — Investigation Sequence

**1. PowerView creation alert** — Detection of `PowerView.ps1` in the Downloads directory.
![SOC 4 initial alert](SOC04-01-initial-alert-1020.png)

**2. PowerView SIEM search** — Splunk search pivots on the script artifact.
![SOC 4 PowerView search](SOC04-02-powerview-search.png)

**3. Host-scoped execution evidence** — Search narrowed to `win-3450` to validate script execution context.
![SOC 4 host search](SOC04-03-host-powerview-search.png)

**4. Endpoint telemetry review** — Surrounding host events used to correlate reconnaissance with the wider compromise.
![SOC 4 endpoint telemetry](SOC04-04-host-telemetry.png)

**5. True Positive case report** — Analyst documents why the dual-use tool is malicious in this incident context.
![SOC 4 report](SOC04-05-true-positive-report.png)

**6. Remediation and escalation** — Containment, script quarantine, credential review and forensic actions documented.
![SOC 4 escalation](SOC04-06-remediation-escalation.png)

---

## SOC 5 — Network Drive Disconnection Following Data Staging

Sysmon recorded `net.exe` PID `8004`, spawned by PowerShell PID `3728`, executing `net.exe use Z: /delete`. In isolation this command may be legitimate, but correlation with the mapped financial-records share and staged archive made it significant as part of the suspicious sequence.

**Response:** Preserve the staged archive, determine what was accessed through `Z:`, review network/share and authentication logs, contain the endpoint and investigate possible exfiltration.

### Evidence — Investigation Sequence

**1. Drive-disconnection alert 1024** — `net.exe use Z: /delete` detected on `win-3450`.
![SOC 5 initial alert](SOC05-01-initial-alert-1024.png)

**2. PID 8004 Splunk pivot** — SIEM evidence validates the exact `net.exe` process and command line.
![SOC 5 PID search](SOC05-02-pid-8004-search.png)

**3. Parent PID 3728 correlation** — PowerShell lineage connects drive cleanup to the broader malicious sequence.
![SOC 5 parent PID](SOC05-03-parent-pid-3728.png)

**4. Archive creation evidence** — `exfilItems.zip` creation supports the data-staging hypothesis.
![SOC 5 archive](SOC05-04-exfilitems-archive.png)

**5. True Positive case report** — Correlated evidence documented with remediation actions.
![SOC 5 report](SOC05-05-true-positive-report.png)

**6. Escalation decision** — Drive cleanup is escalated because it follows suspicious share access and staging.
![SOC 5 escalation](SOC05-06-escalation.png)

---

## SOC 6 — Suspicious Parent-Child / DNS Exfiltration Activity

Sysmon identified `powershell.exe` PID `3728` spawning `nslookup.exe` PID `5432` from `C:\Users\michael.ascot\Downloads\exfiltration\`. The child process queried an encoded-looking subdomain of `haz4rdw4re.io`. Together with repeated DNS queries and staging evidence, this supported **True Positive** classification and escalation for suspected DNS-based exfiltration.

**Response:** Immediately isolate `win-3450`, terminate malicious PowerShell activity, block `haz4rdw4re.io`, preserve logs/artifacts, investigate staged data and hunt for related activity across the environment.

### Evidence — Investigation Sequence

**1. Suspicious parent-child alert 1027** — PowerShell spawning `nslookup.exe` from the exfiltration working directory.
![SOC 6 initial alert](SOC06-01-initial-alert-1027.png)

**2. Process-lineage investigation** — SIEM evidence correlates `nslookup.exe` PID `5432` with PowerShell PID `3728`.
![SOC 6 process lineage](SOC06-02-process-lineage.png)

**3. DNS command-line evidence** — Encoded-looking subdomain query to `haz4rdw4re.io` establishes the suspicious external destination.
![SOC 6 DNS query](SOC06-03-dns-query.png)

**4. Exfiltration-directory correlation** — Working-directory and staged-data context link the DNS behavior to the wider incident.
![SOC 6 exfiltration context](SOC06-04-exfiltration-context.png)

**5. True Positive case report** — Analyst assessment correlates DNS activity, PowerShell lineage and staged data.
![SOC 6 report](SOC06-05-true-positive-report.png)

**6. IOC documentation and escalation** — Final containment, blocking, hunting and escalation actions recorded.
![SOC 6 escalation](SOC06-06-iocs-escalation.png)

---

## Correlated Incident Chain

`PowerShell → Network share mapping → Data staging/archive creation → Drive cleanup → PowerView reconnaissance → Repeated nslookup/DNS activity`

The principal analyst lesson is that alert severity must be interpreted in context. Low- and medium-severity detections became materially more significant after process ancestry, file events, working directories, command lines and DNS activity were correlated.

## Analyst Assessment

The combined telemetry supports a multi-stage endpoint compromise involving access to a sensitive network share, local staging of data, Active Directory reconnaissance and suspected DNS-based exfiltration. The strongest correlation point is PowerShell PID `3728`, which appears repeatedly across process creation, archive staging and DNS-query activity. This demonstrates why SOC analysts should reconstruct behavior across alerts rather than evaluate each detection solely by its assigned severity.

## Skills Demonstrated

- Splunk SIEM investigation
- Sysmon event analysis
- PowerShell and script-block analysis
- Parent-child process correlation
- Network-share investigation
- Active Directory reconnaissance detection
- Data-staging identification
- DNS anomaly and suspected exfiltration analysis
- IOC documentation
- True Positive classification
- 5Ws incident reporting
- Escalation reasoning
- Containment and remediation planning
- Multi-alert incident reconstruction

## Portfolio Note

Evidence is retained in chronological case order so a reviewer can follow the analyst workflow from alert triage through SIEM investigation, correlation, classification, remediation and escalation. Screenshots are training artifacts from the controlled simulation and are presented to demonstrate investigative methodology rather than merely task completion.

> **Lab disclaimer:** This project documents hands-on work completed in a controlled TryHackMe SOC simulation. The systems, identities, domains and telemetry shown are training evidence and do not represent a real production compromise.

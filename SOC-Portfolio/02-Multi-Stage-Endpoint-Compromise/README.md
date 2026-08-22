# Endpoint & SIEM Investigation

## Multi-Stage Endpoint Compromise — PowerShell, Reconnaissance, Data Staging & Suspected DNS Exfiltration

**Platform:** TryHackMe SOC simulation  
**Date:** 22 August 2026  
**Primary host:** `win-3450`  
**SIEM:** Splunk Enterprise  
**Telemetry:** Sysmon, PowerShell and email events

## Executive Summary

This project documents six SOC alerts investigated in a controlled security simulation. Each alert is preserved as an individual analyst case while the related endpoint detections are correlated into a broader incident chain. The investigation demonstrates phishing triage, Splunk SIEM searching, Sysmon analysis, PowerShell investigation, network-share activity, data staging, Active Directory reconnaissance, suspicious DNS activity, incident classification, escalation and remediation planning.

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

### Evidence

**Initial alert**  
![SOC 1 initial alert](SOC01-01-initial-alert.png)

**Subject-based SIEM search**  
![SOC 1 subject search](SOC01-02-subject-search.png)

**Sender-based SIEM search**  
![SOC 1 sender search](SOC01-03-sender-search.png)

**Recipient pivot / scope check**  
![SOC 1 recipient pivot](SOC01-04-recipient-pivot-no-result.png)

**Case report overview**  
![SOC 1 case report](SOC01-05-case-report-overview.png)

**True Positive classification**  
![SOC 1 true positive](SOC01-06-true-positive-report.png)

**Remediation and no-escalation decision**  
![SOC 1 remediation](SOC01-07-remediation-no-escalation.png)

---

## SOC 2 — Network Drive Mapping & Suspicious Data Staging

Sysmon telemetry showed `powershell.exe` PID `3728` spawning `net.exe` to map `Z:` to `\\FILESRV-01\SSF-FinancialRecords`. The same PowerShell lineage was associated with archive creation under `C:\Users\michael.ascot\Downloads\exfiltration\`, and further investigation exposed suspicious PowerShell/Powercat activity. Correlation made this suspicious access and staging behavior rather than routine administration.

**Response:** Isolate `win-3450`, investigate the complete PowerShell process tree, preserve and analyse staged archives, identify files accessed through the share, review authentication/network telemetry and determine whether data left the environment.

### Evidence

**Alert 1022 / case overview**  
![SOC 2 alert](SOC02-01-case-report-alert-1022.png)

**Network-share alert details**  
![SOC 2 alert details](SOC02-02-alert-details-network-share.png)

**Host-scoped SIEM search**  
![SOC 2 host search](SOC02-03-host-search.png)

**`net.exe` correlation**  
![SOC 2 net correlation](SOC02-04-net-exe-correlation.png)

**PowerShell archive creation**  
![SOC 2 archive creation](SOC02-05-powershell-archive-creation.png)

**PowerShell process-create filter**  
![SOC 2 process create](SOC02-06-powershell-process-create-filter.png)

**Powercat command-line evidence**  
![SOC 2 powercat](SOC02-07-powercat-command-line.png)

**PID 3728 correlation**  
![SOC 2 PID correlation](SOC02-08-pid-3728-correlation.png)

**True Positive case report**  
![SOC 2 report](SOC02-09-true-positive-case-report.png)

**Remediation and escalation**  
![SOC 2 escalation](SOC02-10-remediation-escalation.png)

---

## SOC 3 — Repeated Nslookup / Suspected DNS Exfiltration

PowerShell PID `3728` repeatedly spawned `nslookup.exe`. Splunk analysis identified ten distinct command lines containing encoded-looking strings as subdomains of `haz4rdw4re.io`. Correlation with staged archive activity was consistent with suspected DNS-based data exfiltration.

**Indicators:** `powershell.exe` PID `3728`, `nslookup.exe`, `haz4rdw4re.io`, repeated encoded-looking DNS queries, staged archive.  
**Response:** Isolate the endpoint, block the suspicious domain, preserve telemetry and artifacts, investigate staged data, hunt across other systems and perform endpoint forensic analysis.

### Evidence

**Initial high-severity alert 1025**  
![SOC 3 alert](SOC03-01-initial-alert-1025.png)

**Nslookup SIEM search**  
![SOC 3 nslookup search](SOC03-02-nslookup-search.png)

**Parent PID filter**  
![SOC 3 parent PID](SOC03-03-parent-pid-filter.png)

**Ten distinct DNS queries**  
![SOC 3 DNS queries](SOC03-04-ten-distinct-dns-queries.png)

**True Positive report**  
![SOC 3 report](SOC03-05-true-positive-report.png)

**IOC documentation and escalation**  
![SOC 3 escalation](SOC03-06-iocs-escalation.png)

---

## SOC 4 — PowerView PowerShell Reconnaissance

`PowerView.ps1` was created under `C:\Users\michael.ascot\Downloads\` and PowerShell script-block telemetry showed associated execution activity. PowerView is a dual-use Active Directory reconnaissance framework; within the surrounding suspicious endpoint activity, its execution warranted escalation.

**Response:** Isolate the endpoint, terminate suspicious PowerShell activity, quarantine the script, determine its origin, review PowerShell/Sysmon/authentication telemetry and scope reconnaissance activity.

### Evidence

**Initial alert 1020**  
![SOC 4 alert](SOC04-01-initial-alert-1020.png)

**PowerView SIEM search**  
![SOC 4 search](SOC04-02-powerview-search.png)

**Host / PowerView correlation**  
![SOC 4 correlation](SOC04-03-host-powerview-correlation.png)

**Host-wide telemetry review**  
![SOC 4 host search](SOC04-04-host-wide-search.png)

**True Positive and remediation evidence**  
![SOC 4 remediation](SOC04-05-true-positive-remediation.png)

**Escalation decision**  
![SOC 4 escalation](SOC04-06-escalation.png)

---

## SOC 5 — Network Drive Disconnection Following Data Staging

Sysmon recorded `net.exe` PID `8004`, spawned by PowerShell PID `3728`, executing `net.exe use Z: /delete`. In isolation this command may be legitimate, but correlation with the mapped financial-records share and staged archive made it significant as part of the suspicious sequence.

**Response:** Preserve the staged archive, determine what was accessed through `Z:`, review network/share and authentication logs, contain the endpoint and investigate possible exfiltration.

### Evidence

**Initial alert 1024**  
![SOC 5 alert](SOC05-01-initial-alert-1024.png)

**`net.exe` PID 8004 investigation**  
![SOC 5 PID 8004](SOC05-02-net-exe-pid-8004.png)

**Parent PID 3728 / archive correlation**  
![SOC 5 parent correlation](SOC05-03-parent-pid-3728-archive.png)

**True Positive and remediation evidence**  
![SOC 5 remediation](SOC05-04-true-positive-remediation.png)

**Escalation decision**  
![SOC 5 escalation](SOC05-05-escalation.png)

---

## SOC 6 — Suspicious Parent-Child / DNS Exfiltration Activity

Sysmon identified `powershell.exe` PID `3728` spawning `nslookup.exe` PID `5432` from `C:\Users\michael.ascot\Downloads\exfiltration\`. The child process queried an encoded-looking subdomain of `haz4rdw4re.io`. Together with repeated DNS queries and staging evidence, this supported **True Positive** classification and escalation for suspected DNS-based exfiltration.

**Response:** Immediately isolate `win-3450`, terminate malicious PowerShell activity, block `haz4rdw4re.io`, preserve logs/artifacts, investigate staged data and hunt for related activity across the environment.

### Evidence

**Initial alert 1027**  
![SOC 6 alert](SOC06-01-initial-alert-1027.png)

**True Positive, remediation and IOC documentation**  
![SOC 6 remediation](SOC06-02-true-positive-remediation-iocs.png)

**Escalation decision**  
![SOC 6 escalation](SOC06-03-escalation.png)

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

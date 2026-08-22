# Endpoint & SIEM Investigation

## Multi-Stage Endpoint Compromise — PowerShell, Reconnaissance, Data Staging & Suspected DNS Exfiltration

**Platform:** TryHackMe SOC simulation  
**Date:** 22 August 2026  
**Primary host:** `win-3450`  
**SIEM:** Splunk Enterprise  
**Telemetry:** Sysmon and PowerShell events

## Executive Summary

This project documents five correlated endpoint and SIEM alerts investigated in a controlled security simulation. The investigation focuses exclusively on endpoint compromise activity: suspicious PowerShell execution, network-share access, data staging, Active Directory reconnaissance, drive cleanup and suspected DNS-based exfiltration. The separate suspicious-email/phishing alert from the same training session is maintained in the dedicated SOC Phishing Investigation portfolio so the evidence domains remain clearly separated.

## Case Register

| Case | Alert | Severity | Classification | Escalation |
|---|---|---|---|---|
| Endpoint Case 1 | Network Drive Mapping & Suspicious Data Staging — Alert 1022 | Medium | True Positive | Yes |
| Endpoint Case 2 | Repeated Nslookup / Suspected DNS Exfiltration — Alert 1025 | High | True Positive | Yes |
| Endpoint Case 3 | PowerView PowerShell Reconnaissance — Alert 1020 | Low | True Positive | Yes |
| Endpoint Case 4 | Network Drive Disconnection Following Data Staging — Alert 1024 | Medium | True Positive | Yes |
| Endpoint Case 5 | Suspicious Parent-Child / DNS Exfiltration Activity — Alert 1027 | High | True Positive | Yes |

---

## Endpoint Case 1 — Network Drive Mapping & Suspicious Data Staging

Sysmon telemetry showed `powershell.exe` PID `3728` spawning `net.exe` to map `Z:` to `\\FILESRV-01\SSF-FinancialRecords`. The same PowerShell lineage was associated with archive creation under `C:\Users\michael.ascot\Downloads\exfiltration\`, and further investigation exposed suspicious PowerShell/Powercat activity. Correlation made this suspicious access and staging behavior rather than routine administration.

**Response:** Isolate `win-3450`, investigate the complete PowerShell process tree, preserve and analyse staged archives, identify files accessed through the share, review authentication/network telemetry and determine whether data left the environment.

### Evidence

**Alert 1022 / case overview**  
![Endpoint Case 1 alert](SOC02-01-case-report-alert-1022.png)

**Network-share alert details**  
![Endpoint Case 1 alert details](SOC02-02-alert-details-network-share.png)

**Host-scoped SIEM search**  
![Endpoint Case 1 host search](SOC02-03-host-search.png)

**`net.exe` correlation**  
![Endpoint Case 1 net correlation](SOC02-04-net-exe-correlation.png)

**PowerShell archive creation**  
![Endpoint Case 1 archive creation](SOC02-05-powershell-archive-creation.png)

**PowerShell process-create filter**  
![Endpoint Case 1 process create](SOC02-06-powershell-process-create-filter.png)

**Powercat command-line evidence**  
![Endpoint Case 1 powercat](SOC02-07-powercat-command-line.png)

**PID 3728 correlation**  
![Endpoint Case 1 PID correlation](SOC02-08-pid-3728-correlation.png)

**True Positive case report**  
![Endpoint Case 1 report](SOC02-09-true-positive-case-report.png)

**Remediation and escalation**  
![Endpoint Case 1 escalation](SOC02-10-remediation-escalation.png)

---

## Endpoint Case 2 — Repeated Nslookup / Suspected DNS Exfiltration

PowerShell PID `3728` repeatedly spawned `nslookup.exe`. Splunk analysis identified ten distinct command lines containing encoded-looking strings as subdomains of `haz4rdw4re.io`. Correlation with staged archive activity was consistent with suspected DNS-based data exfiltration.

**Indicators:** `powershell.exe` PID `3728`, `nslookup.exe`, `haz4rdw4re.io`, repeated encoded-looking DNS queries, staged archive.  
**Response:** Isolate the endpoint, block the suspicious domain, preserve telemetry and artifacts, investigate staged data, hunt across other systems and perform endpoint forensic analysis.

### Evidence

**Initial high-severity alert 1025**  
![Endpoint Case 2 alert](SOC03-01-initial-alert-1025.png)

**Nslookup SIEM search**  
![Endpoint Case 2 nslookup search](SOC03-02-nslookup-search.png)

**Parent PID filter**  
![Endpoint Case 2 parent PID](SOC03-03-parent-pid-filter.png)

**Ten distinct DNS queries**  
![Endpoint Case 2 DNS queries](SOC03-04-ten-distinct-dns-queries.png)

**True Positive report**  
![Endpoint Case 2 report](SOC03-05-true-positive-report.png)

**IOC documentation and escalation**  
![Endpoint Case 2 escalation](SOC03-06-iocs-escalation.png)

---

## Endpoint Case 3 — PowerView PowerShell Reconnaissance

`PowerView.ps1` was created under `C:\Users\michael.ascot\Downloads\` and PowerShell script-block telemetry showed associated execution activity. PowerView is a dual-use Active Directory reconnaissance framework; within the surrounding suspicious endpoint activity, its execution warranted escalation.

**Response:** Isolate the endpoint, terminate suspicious PowerShell activity, quarantine the script, determine its origin, review PowerShell/Sysmon/authentication telemetry and scope reconnaissance activity.

### Evidence

**Initial alert 1020**  
![Endpoint Case 3 alert](SOC04-01-initial-alert-1020.png)

**PowerView SIEM search**  
![Endpoint Case 3 search](SOC04-02-powerview-search.png)

**Host / PowerView correlation**  
![Endpoint Case 3 correlation](SOC04-03-host-powerview-correlation.png)

**Host-wide telemetry review**  
![Endpoint Case 3 host search](SOC04-04-host-wide-search.png)

**True Positive and remediation evidence**  
![Endpoint Case 3 remediation](SOC04-05-true-positive-remediation.png)

**Escalation decision**  
![Endpoint Case 3 escalation](SOC04-06-escalation.png)

---

## Endpoint Case 4 — Network Drive Disconnection Following Data Staging

Sysmon recorded `net.exe` PID `8004`, spawned by PowerShell PID `3728`, executing `net.exe use Z: /delete`. In isolation this command may be legitimate, but correlation with the mapped financial-records share and staged archive made it significant as part of the suspicious sequence.

**Response:** Preserve the staged archive, determine what was accessed through `Z:`, review network/share and authentication logs, contain the endpoint and investigate possible exfiltration.

### Evidence

**Initial alert 1024**  
![Endpoint Case 4 alert](SOC05-01-initial-alert-1024.png)

**`net.exe` PID 8004 investigation**  
![Endpoint Case 4 PID 8004](SOC05-02-net-exe-pid-8004.png)

**Parent PID 3728 / archive correlation**  
![Endpoint Case 4 parent correlation](SOC05-03-parent-pid-3728-archive.png)

**True Positive and remediation evidence**  
![Endpoint Case 4 remediation](SOC05-04-true-positive-remediation.png)

**Escalation decision**  
![Endpoint Case 4 escalation](SOC05-05-escalation.png)

---

## Endpoint Case 5 — Suspicious Parent-Child / DNS Exfiltration Activity

Sysmon identified `powershell.exe` PID `3728` spawning `nslookup.exe` PID `5432` from `C:\Users\michael.ascot\Downloads\exfiltration\`. The child process queried an encoded-looking subdomain of `haz4rdw4re.io`. Together with repeated DNS queries and staging evidence, this supported **True Positive** classification and escalation for suspected DNS-based exfiltration.

**Response:** Immediately isolate `win-3450`, terminate malicious PowerShell activity, block `haz4rdw4re.io`, preserve logs/artifacts, investigate staged data and hunt for related activity across the environment.

### Evidence

**Initial alert 1027**  
![Endpoint Case 5 alert](SOC06-01-initial-alert-1027.png)

**True Positive, remediation and IOC documentation**  
![Endpoint Case 5 remediation](SOC06-02-true-positive-remediation-iocs.png)

**Escalation decision**  
![Endpoint Case 5 escalation](SOC06-03-escalation.png)

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

Evidence is retained in chronological case order so a reviewer can follow the analyst workflow from alert triage through SIEM investigation, correlation, classification, remediation and escalation. The phishing investigation completed during the same training session is documented separately in the dedicated SOC Phishing Investigation repository.

> **Lab disclaimer:** This project documents hands-on work completed in a controlled TryHackMe SOC simulation. The systems, identities, domains and telemetry shown are training evidence and do not represent a real production compromise.

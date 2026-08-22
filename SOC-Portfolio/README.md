# Security Operations (SOC) Portfolio

Hands-on defensive-security casework demonstrating alert triage, Splunk SIEM investigation, Sysmon/process analysis, evidence correlation, incident classification, analyst documentation, escalation and remediation reasoning.

> **Portfolio note:** SOC labs and simulations are identified as training work. The purpose is to demonstrate investigation methodology and analyst judgement, not to present simulated incidents as production SOC employment.

## Featured Investigation

### Multi-Stage Endpoint Compromise — PowerShell, Reconnaissance, Data Staging & Suspected DNS Exfiltration
A six-alert TryHackMe SOC investigation on `win-3450` reconstructed through Splunk, Sysmon and PowerShell telemetry. The case covers phishing triage, network-share activity, PowerShell process correlation, archive staging, PowerView reconnaissance, repeated `nslookup.exe` activity and suspected DNS-based exfiltration.

**Evidence:** 37 screenshots organised across SOC 1–SOC 6.  
**Outcome:** All six alerts classified as True Positive; five required escalation.

[Open the Endpoint & SIEM investigation](02-Multi-Stage-Endpoint-Compromise/)

## SOC Case 01 — Suspicious PowerShell & Network Share Activity
A focused process-chain investigation covering PowerShell, `nslookup.exe`, `net.exe`, sensitive network-share context and evidence-based True Positive classification.

[Open SOC Case 01](01-PowerShell-Network-Share-Investigation/)

## Phishing Investigation & Incident Triage
A separate evidence-backed phishing repository covering email triage, SIEM correlation, IOC analysis, True/False Positive decisions, 5Ws documentation, escalation and remediation.

[Open the SOC Phishing Investigation repository](https://github.com/odogwu-source/SOC-PHISHING-INVESTIGATION)

## Analyst Workflow Demonstrated
**Alert → Validate evidence → Correlate indicators → Determine scope → Classify → Document 5Ws → Escalate/contain → Recommend remediation → Record lessons learned**

## Core Skills Demonstrated
- Splunk SIEM and Sysmon investigation
- PowerShell and script-block analysis
- Parent-child process correlation
- Network-share activity analysis
- Active Directory reconnaissance detection
- Data-staging identification
- DNS anomaly and suspected exfiltration analysis
- Phishing and suspicious-email triage
- IOC analysis and documentation
- True Positive / False Positive classification
- 5Ws incident documentation
- Escalation and remediation reasoning
- Evidence capture and case documentation
- Security-event communication

## Evidence & Ethics Statement
Screenshots, logs and case records in this portfolio are retained as educational evidence. Sensitive or identifying information should be redacted where necessary. Training scenarios are not represented as incidents handled for real clients or employers. Analytical conclusions distinguish observed telemetry from hypotheses about attacker intent or confirmed data loss.

[Return to main portfolio](../index.html)
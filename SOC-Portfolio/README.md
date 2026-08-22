# Security Operations (SOC) Portfolio

Hands-on defensive-security casework demonstrating alert triage, evidence analysis, SIEM correlation, incident classification, analyst documentation, escalation and remediation reasoning.

> **Portfolio note:** SOC labs and simulations are identified as training work. The purpose is to demonstrate investigation methodology and analyst judgement, not to present simulated incidents as production SOC employment.

## Featured Investigation — Correlated Six-Alert Incident
### PowerShell, Reconnaissance & Suspected DNS Exfiltration
A TryHackMe SOC simulation reconstructed as one correlated incident investigation. Six related alerts on `win-3450` were analysed through Splunk, Sysmon and PowerShell telemetry, revealing a PowerShell-led chain involving PowerView reconnaissance, network-drive operations, ZIP archive staging and repeated `nslookup.exe` queries to encoded-looking subdomains.

**Classification:** True Positive — Escalation warranted.

[Open the correlated six-alert investigation](02-Correlated-PowerShell-DNS-Exfiltration-Investigation/)

## SOC Case 01 — Suspicious PowerShell & Network Share Activity
A focused process-chain investigation covering PowerShell, `nslookup.exe`, `net.exe`, sensitive network-share context and evidence-based True Positive classification.

[Open SOC Case 01](01-PowerShell-Network-Share-Investigation/)

## Phishing Investigation & Incident Triage
A documented investigation workflow covering phishing alert triage, SIEM correlation, email/IOC analysis, URL and IP review, True/False Positive classification, 5Ws analyst documentation, escalation and remediation.

[Open the SOC Phishing Investigation repository](https://github.com/odogwu-source/SOC-PHISHING-INVESTIGATION)

## Analyst Workflow Demonstrated
**Alert → Validate evidence → Correlate indicators → Determine scope → Classify → Document 5Ws → Escalate/contain → Recommend remediation → Record lessons learned**

## Core Skills Demonstrated
- Splunk SIEM and Sysmon investigation
- PowerShell and script-block analysis
- Parent-child process correlation
- DNS anomaly and suspected exfiltration analysis
- Phishing and suspicious-email triage
- IOC and URL/IP analysis
- True Positive / False Positive classification
- 5Ws incident documentation
- Escalation and remediation reasoning
- Evidence capture and case documentation
- Security-event communication

## Evidence & Ethics Statement
Screenshots, logs and case records in this portfolio are retained as educational evidence. Sensitive or identifying information should be redacted where necessary. Training scenarios are not represented as incidents handled for real clients or employers. Conclusions distinguish observed telemetry from analytical hypotheses.

[Return to main portfolio](../index.html)
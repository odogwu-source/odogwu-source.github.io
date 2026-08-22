# Michael Nweke — Cybersecurity Portfolio

Practical, evidence-led cybersecurity portfolio organised around **Security Operations (SOC)**, **Governance, Risk & Compliance (GRC)**, and **Cybersecurity Awareness & Human Risk**, with ethical-hacking methodology supporting the technical work.

My portfolio demonstrates how I investigate security events, correlate telemetry, assess cyber risk, analyse controls, document evidence and translate technical findings into practical security decisions.

> **Evidence & ethics:** Lab and simulation projects are explicitly identified as educational/practice work. They demonstrate methodology, analysis, evidence handling and documentation; they are not presented as production audits, client engagements, compliance attestations or penetration tests against real organisations.

## Featured Work

### 1. Multi-Stage Endpoint Compromise — Featured SOC Investigation
Five correlated TryHackMe endpoint alerts reconstructed through **Splunk, Sysmon and PowerShell telemetry** on `win-3450`.

**Investigation chain:** PowerShell → network-share access → data staging → drive cleanup → PowerView reconnaissance → repeated `nslookup.exe` activity → suspected DNS-based exfiltration.

**Evidence:** 30 screenshots organised across five endpoint cases.  
**Outcome:** Five **True Positive** classifications with escalation and remediation reasoning.

[Open the featured Endpoint & SIEM investigation](./SOC-Portfolio/02-Multi-Stage-Endpoint-Compromise/)

### 2. SOC Phishing Investigation & Incident Triage
Dedicated phishing repository containing evidence-backed suspicious-email investigations, including the **22 August 2026 Suspicious External Email / Inheritance Phishing (Alert 1000)** case.

**Demonstrates:** SIEM pivots · sender/domain analysis · phishing indicators · 5Ws reporting · True Positive/False Positive judgement · escalation decisions · remediation planning.

[Open the SOC Phishing Investigation repository](https://github.com/odogwu-source/SOC-PHISHING-INVESTIGATION)

### 3. Governance, Risk & Compliance
Eight structured case studies covering cloud and enterprise risk, third-party risk, compliance readiness, incident-response governance, OT/SCADA resilience and BCDR, Responsible AI governance, and Zero Trust/IAM risk.

**Demonstrates:** risk registers · likelihood/impact analysis · control assessment · evidence quality · remediation planning · governance documentation · framework mapping.

[Open the GRC Portfolio](./GRC-Portfolio/)

## Portfolio Tracks

### Security Operations (SOC)
Hands-on defensive-security work covering SIEM/log analysis, endpoint telemetry, alert classification, IOC analysis, process correlation, incident documentation, escalation and remediation.

[Open SOC Portfolio](./SOC-Portfolio/)

### Governance, Risk & Compliance (GRC)
Structured governance and cyber-risk casework covering enterprise/cloud risk, third-party risk, compliance readiness, incident-response governance, resilience, Responsible AI and Zero Trust/IAM.

[Open GRC Portfolio](./GRC-Portfolio/)

### Cybersecurity Awareness & Human Risk
Security-awareness programme materials covering phishing/social engineering, employee education, assessment, simulated scenarios, metrics and management reporting.

[Open Cybersecurity Awareness](./Cybersecurity-Awareness/)

## Core Competencies

**SOC & Defensive Security:** Splunk SIEM · Sysmon · alert triage · phishing investigation · PowerShell analysis · process-tree correlation · IOC analysis · DNS investigation · incident response · escalation · remediation

**GRC & Resilience:** cyber risk assessment · control analysis · third-party risk · compliance readiness · evidence management · BCDR · OT/SCADA resilience · AI governance · IAM · privileged access · Zero Trust

**Awareness & Human Risk:** social engineering · phishing awareness · employee education · knowledge assessment · security culture · management reporting

**Ethical Hacking Methodology:** reconnaissance · vulnerability assessment concepts · attack-path understanding · defensive interpretation of offensive techniques

## Certifications

**CompTIA Security+ · CompTIA Network+ · CompTIA CySA+ · Certified Ethical Hacker (CEH)**

## Analyst Workflow Demonstrated

**Alert / Scenario → Validate evidence → Correlate indicators → Determine scope → Assess risk → Classify → Document 5Ws / findings → Escalate or contain → Recommend remediation → Record lessons learned**

## Repository Structure

```text
index.html
README.md
SOC-Portfolio/
  01-PowerShell-Network-Share-Investigation/
  02-Multi-Stage-Endpoint-Compromise/
GRC-Portfolio/
Cybersecurity-Awareness/
styles.css
script.js
```

## How to Review

Recruiters and hiring managers can start with **Featured Work** for the strongest evidence-backed investigations, then move into the professional track most relevant to the role. Individual project READMEs explain the scenario, objectives, methodology, findings, evidence, security/risk analysis, controls, remediation, demonstrated skills and lessons learned.

The portfolio is intentionally structured around **evidence over claims**: where screenshots or supporting records exist, they are curated to show the analytical sequence rather than uploaded as an unstructured collection.

## Portfolio Website

[Open the live cybersecurity portfolio](https://odogwu-source.github.io/)

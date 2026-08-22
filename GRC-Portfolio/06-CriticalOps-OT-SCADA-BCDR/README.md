# Project 06 — CriticalOps OT/SCADA Business Continuity & Disaster Recovery Assessment

> **Portfolio case study:** Educational/simulated OT/SCADA resilience assessment. No real industrial-control environment was accessed, tested or audited.

## Executive Summary
This project assesses business continuity and disaster-recovery risk in a simulated operational technology (OT)/SCADA environment. It demonstrates how critical operational assets, ransomware exposure, backup/recovery controls and redundancy weaknesses can be converted into risk-based resilience decisions.

## Assessment Snapshot
- **Domain:** OT/SCADA resilience and BCDR
- **Priority findings:** Critical ransomware exposure, backup-failure risk and redundancy gap
- **Curated evidence:** 6 screenshots
- **Decision focus:** recoverability, redundancy, operational impact and resilience

## Scenario
A simulated critical-operations environment depends on OT/SCADA assets whose prolonged loss could disrupt essential operations. The assessment evaluates ransomware and availability threats, recovery capability and whether resilience controls are sufficient for high-impact systems.

## Objectives
- Identify and classify critical OT/SCADA assets.
- Evaluate ransomware, backup and availability risks.
- Assess recovery and redundancy controls.
- Prioritise continuity improvements based on operational impact.
- Document evidence and risk rationale for management review.

## Environment / Tools
Educational GRC simulation, OT/SCADA asset inventory, risk register, likelihood-impact matrix, backup/recovery assessment, redundancy review and evidence screenshots.

## Methodology
1. Built and reviewed the critical-asset inventory.
2. Identified availability and ransomware risk scenarios.
3. Applied likelihood-impact scoring and documented rationale.
4. Assessed backup/recovery and redundancy control effectiveness.
5. Prioritised treatment actions according to operational consequence.

## Assessment
The assessment examined whether critical assets were inventoried, whether ransomware could create unacceptable operational disruption, whether backups were sufficiently recoverable and whether redundancy provided genuine resilience rather than a single hidden dependency.

## Key Findings
The curated evidence demonstrates a **Critical ransomware exposure**, a material backup-failure risk, a documented backup/recovery control and a redundancy-effectiveness gap. Together these findings show that resilience requires both recoverable data and alternative operational capability.

## Risk / Security Analysis
For OT/SCADA environments, availability and safety-related consequences can dominate traditional information-security priorities. Recovery controls must therefore be evaluated for actual restoration capability, isolation, dependency and operational recovery time—not simply for the existence of backups.

## Controls and Recommendations
- Maintain offline/immutable backup capability where appropriate.
- Define and test recovery procedures and restoration capability.
- Establish business-defined RTO/RPO targets.
- Reduce single points of failure and validate redundancy.
- Segment OT and IT networks according to operational/security requirements.
- Restrict and monitor privileged access.
- Maintain incident, continuity and disaster-recovery playbooks.
- Conduct periodic resilience and restoration exercises.

## Evidence
The final recruiter-facing evidence set contains six screenshots:

1. `01-asset-inventory-summary.png`
2. `02-risk-register-summary.png`
3. `03-ransomware-critical-risk.png`
4. `04-backup-failure-high-risk.png`
5. `05-backup-recovery-control.png`
6. `06-redundancy-effectiveness-gap.png`

The sequence tells the assessment story: **assets → risk register → ransomware → backup failure → recovery control → redundancy gap**.

## Skills Demonstrated
OT/SCADA risk assessment; BCDR analysis; ransomware-risk analysis; asset criticality; risk scoring; backup/recovery governance; redundancy assessment; resilience recommendations.

## Frameworks / Standards Referenced
- NIST Cybersecurity Framework (CSF) 2.0
- NIST SP 800-82 Rev. 3 — Guide to Operational Technology (OT) Security
- NIST SP 800-34 Rev. 1 — Contingency Planning Guide for Federal Information Systems
- ISO 22301:2019 — Business Continuity Management Systems
- IEC 62443 series — industrial automation and control systems security

## Project Outcomes
The project produced an evidence-backed resilience assessment linking critical assets, operational risks, recovery controls, redundancy weaknesses and prioritised treatment actions.

## Lessons Learned
A backup is not equivalent to recoverability, and redundancy is not equivalent to resilience. Critical operations require tested recovery paths, understood dependencies and controls designed around operational consequences.

## Disclaimer
This is an educational/simulated OT/SCADA GRC case study. It does not represent a penetration test, safety assessment, audit or assurance engagement against any real industrial-control system.
# Project 08 — SecureBank Zero Trust, IAM & Access-Control Risk Assessment

> **Portfolio case study:** Educational/simulated banking IAM assessment. It demonstrates risk and control methodology and is not an assessment of a real bank or financial institution.

## Executive Summary
This project evaluates identity and access-management risk across simulated critical banking assets. It demonstrates how privileged-account compromise, weak authentication, excessive access and privilege escalation can create systemic risk, and how Zero Trust principles can guide proportionate access-control improvements.

## Scenario
A simulated banking environment contains critical customer, transaction, identity and monitoring systems. The assessment evaluates whether authentication, privilege management and access-control weaknesses could enable account takeover, fraud, sensitive-data exposure or wider compromise.

## Objectives
- Identify and classify critical banking assets.
- Assess privileged-access and authentication risks.
- Evaluate least-privilege and privilege-escalation exposure.
- Translate technical IAM weaknesses into business consequences.
- Recommend Zero Trust-aligned identity and access controls.

## Environment / Tools
Educational GRC simulation, critical-asset inventory, IAM/access-control risk register, likelihood-impact matrix and curated evidence screenshots.

## Methodology
1. Inventoried critical banking systems and data assets.
2. Identified identity, authentication and privilege threat scenarios.
3. Applied likelihood-impact scoring and documented rationale.
4. Assessed business consequences across confidentiality, integrity and availability.
5. Recommended preventive, detective and governance controls.

## Investigation or Assessment
The assessment considered privileged-account compromise, online-banking authentication, customer-database access, payment-processing privileges and IAM privilege escalation. Particular attention was given to risks capable of crossing system boundaries through trusted identities.

## Key Findings
The evidence demonstrates critical privileged-access and authentication exposure alongside high-impact database and payment-processing access risks. IAM privilege escalation is especially significant because compromise of the identity control plane can affect multiple downstream systems.

## Risk / Security Analysis
Identity is a primary security control plane. Excessive standing privilege, weak authentication and inadequate access governance can turn a single compromised account into broad enterprise exposure. Zero Trust therefore requires explicit verification, least privilege and continuous evaluation rather than implicit trust based on network location.

## Controls and Recommendations
Require phishing-resistant MFA where feasible for privileged/high-risk access; implement least privilege and RBAC/ABAC as appropriate; use privileged access management; separate administrative identities; adopt just-in-time/time-bound privileged access; perform periodic access reviews; monitor anomalous authentication and privilege changes; protect service accounts and secrets; segment critical services; and maintain auditable joiner-mover-leaver processes.

## Evidence
The final recruiter-facing evidence set contains seven screenshots:

1. `01-critical-banking-asset-inventory.png`
2. `02-banking-asset-inventory-table.png`
3. `03-privileged-account-critical-risk.png`
4. `04-online-banking-authentication-risk.png`
5. `05-customer-database-access-risk.png`
6. `06-payment-processing-privilege-risk.png`
7. `07-iam-privilege-escalation-risk.png`

The sequence presents: **critical assets → inventory detail → privileged compromise → authentication → sensitive-data access → payment privilege → IAM escalation**.

## Skills Demonstrated
IAM risk assessment; Zero Trust reasoning; privileged-access analysis; authentication risk; least privilege; access governance; risk scoring; banking-system impact analysis; security-control recommendations.

## Frameworks / Standards Referenced
- NIST SP 800-207 — Zero Trust Architecture
- NIST Cybersecurity Framework (CSF) 2.0
- NIST SP 800-53 Rev. 5 — Access Control and Identification & Authentication control families
- CIS Critical Security Controls v8.1
- ISO/IEC 27001:2022

## Lessons Learned
Zero Trust is an architecture and operating principle, not a product. Strong IAM reduces systemic risk when identities are explicitly verified, privileges are minimised, access is continuously reviewed and high-impact actions are observable.

## Disclaimer
This is an educational/simulated banking GRC case study. It does not represent a real bank's architecture, vulnerabilities, compliance status, penetration test or security assurance.
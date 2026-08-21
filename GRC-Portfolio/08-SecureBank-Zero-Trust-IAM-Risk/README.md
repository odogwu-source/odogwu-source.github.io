# Project 08 — Banking Zero Trust, IAM & Access-Control Risk Assessment

## Executive Summary
This hands-on GRC simulation assessed identity, access-control and Zero Trust risks in a digital-banking environment. Six critical production assets were mapped, five access-risk scenarios were assessed, Zero Trust controls were documented, and four supporting security policies were created. The project demonstrates how IAM, privileged access, least privilege, segmentation, continuous verification and monitoring reduce systemic risk across core banking services.

## Scenario
SecureBank Digital operates customer-facing and back-end banking services that depend on strong identity assurance and tightly controlled access. The simulated engagement required a Zero Trust-oriented assessment of critical assets and access risks, followed by documentation of controls and governance policies.

## Objectives
- Map six critical banking assets.
- Assess five identity and access-control risks.
- Document Zero Trust security controls.
- Establish four supporting Zero Trust policies.
- Apply likelihood-impact scoring and document decision rationale.
- Connect access-control weaknesses to financial, regulatory and operational consequences.

## Environment / Tools
- Educational GRC simulation platform
- Asset inventory and criticality classification
- Risk register and 5x5 likelihood-impact scoring
- ISO/IEC 27001 control references
- Zero Trust architecture concepts
- IAM, MFA, RBAC, least privilege, SIEM and continuous-monitoring concepts

## Methodology
1. Identified critical production assets and accountable owners.
2. Classified the assets according to business criticality.
3. Developed threat-vulnerability-impact risk statements.
4. Assessed likelihood and impact and calculated risk severity.
5. Documented decision rationale for each rating.
6. Designed Zero Trust controls addressing identity, authorization, monitoring and segmentation.
7. Created policies governing IAM, privileged access, network segmentation/secure access, and continuous monitoring/incident response.

## Investigation or Assessment
The six critical assets were the Core Banking System, Customer Database, Online Banking Portal, Payment Processing System, Identity and Access Management System, and Security Monitoring/SIEM Platform. Access-risk scenarios included privileged-account compromise, weak authentication against online banking, malicious-insider misuse of excessive privileges, payment-system abuse through excessive privileges, and privilege escalation through inadequate IAM controls.

## Key Findings
- Privileged-account compromise was rated **20/25 — Critical**.
- Weak authentication affecting the Online Banking Portal was rated **20/25 — Critical**.
- Malicious-insider misuse of excessive Customer Database privileges was rated **15/25 — High**.
- Excessive privileges enabling fraudulent payment activity were rated **15/25 — High**.
- IAM privilege escalation was rated **20/25 — Critical**.
- Identity infrastructure represents a systemic dependency: compromise can enable access across several downstream banking systems.

## Risk / Security Analysis
The assessment demonstrated that identity is a primary security control plane in a digital bank. Weak authentication, excessive privileges and compromised administrator accounts can convert a single credential event into broad access to customer data, payment functions and core systems. The Zero Trust approach therefore emphasizes explicit verification, least privilege, strong authentication, segmentation and continuous monitoring rather than implicit trust based on network location.

## Controls and Recommendations
Controls included MFA for privileged accounts, least-privilege enforcement, role-based access control, secure authorization practices, segmentation and continuous monitoring. Recommended treatment includes centralized MFA, privileged-access management, periodic entitlement reviews, segregation of duties, joiner-mover-leaver governance, strong customer authentication, privileged-session monitoring, network micro-segmentation, SIEM correlation for anomalous identity activity, and rapid revocation/containment procedures.

## Evidence
Supporting screenshots document the six-asset inventory, access-risk statements, likelihood-impact scoring, decision rationales, Zero Trust control entries and supporting policy records. Evidence is retained as educational simulation output and not represented as a production assessment of a financial institution.

## Skills Demonstrated
GRC · Zero Trust · IAM · privileged access · MFA · RBAC · least privilege · asset classification · threat-scenario development · risk scoring · insider-threat analysis · banking cybersecurity · security monitoring · policy governance · control documentation

## Frameworks / Standards Referenced
- ISO/IEC 27001:2022 control concepts
- NIST SP 800-207 Zero Trust Architecture concepts
- NIST Cybersecurity Framework concepts
- Identity and access-management governance principles

## Project Outcomes
Produced a structured banking-security case study connecting critical assets to identity-driven risk scenarios, quantified risk decisions, Zero Trust controls and governance policies. The project demonstrates how access-control design can be treated as both a technical security problem and an enterprise risk-management issue.

## Lessons Learned
Zero Trust is not a single product or an MFA deployment. Effective implementation requires coordinated identity governance, least privilege, segmentation, device/user verification, monitoring, policy enforcement and rapid response when trust conditions change.

## Disclaimer
This project was completed in a simulated educational environment for cybersecurity training and portfolio demonstration. The organisation, systems, risks and assessment activities do not represent a production engagement with a real bank or client.
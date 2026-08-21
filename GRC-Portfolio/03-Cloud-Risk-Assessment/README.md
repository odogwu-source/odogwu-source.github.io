# Project 03 — Cloud Security Risk Assessment

> **Portfolio case study:** Educational/simulated cloud GRC assessment demonstrating risk identification, control mapping and treatment planning.

## Executive Summary
This project evaluates cyber risk associated with cloud-hosted assets and services. It demonstrates the application of governance principles to identity, configuration, data protection, logging, resilience and shared-responsibility risks in a cloud environment.

## Scenario
A simulated organisation uses cloud infrastructure and services for business-critical workloads. The assessment examines how misconfiguration, excessive privileges, exposed data, insufficient monitoring and resilience gaps could affect confidentiality, integrity and availability.

## Objectives
- Identify critical cloud assets, data and dependencies.
- Assess identity, configuration, data-protection and monitoring risks.
- Distinguish customer responsibilities from provider responsibilities.
- Prioritise risks and propose practical cloud controls.

## Environment / Tools
Educational GRC simulation, cloud asset inventory, risk register, control assessment, configuration/security evidence and likelihood-impact scoring.

## Methodology
1. Classified cloud assets and data according to business impact.
2. Identified threats and vulnerabilities across identity, configuration and data flows.
3. Assessed likelihood and impact.
4. Evaluated relevant controls and evidence.
5. Recommended risk treatment and continuous assurance activities.

## Investigation or Assessment
The assessment considered IAM design, privileged access, public exposure, encryption, logging, backup/recovery, configuration management and the shared-responsibility model. Particular attention was given to controls whose failure could expose multiple cloud resources simultaneously.

## Key Findings
Cloud risk is frequently driven by configuration and identity rather than by the cloud platform alone. Excessive privileges, weak authentication, unmanaged configuration drift and incomplete logging can create broad attack paths and reduce incident visibility.

## Risk / Security Analysis
Risk was prioritised according to business criticality, exposure and control effectiveness. Cloud controls should be preventative where possible and continuously verifiable because configuration can change rapidly.

## Controls and Recommendations
Enforce MFA and least privilege; use role-based access; separate privileged accounts; encrypt sensitive data in transit and at rest; centralise audit logging; implement secure configuration baselines and configuration monitoring; scan for vulnerabilities; test backups and recovery; manage secrets securely; and periodically review cloud access and architecture.

## Evidence
Portfolio evidence should demonstrate completed cloud-asset identification, risk scoring, control gaps and remediation decisions. Screenshots are educational evidence and not proof of production cloud assurance.

## Skills Demonstrated
Cloud risk assessment; IAM analysis; configuration-risk analysis; shared-responsibility reasoning; data-protection controls; logging/monitoring governance; risk scoring; remediation planning.

## Frameworks / Standards Referenced
- NIST Cybersecurity Framework (CSF) 2.0
- ISO/IEC 27001:2022
- CIS Critical Security Controls v8.1
- Cloud Security Alliance Cloud Controls Matrix (CCM)

## Lessons Learned
Cloud governance must be continuous. Strong architecture can still become insecure through identity sprawl, configuration drift or incomplete visibility, so control ownership and evidence are as important as technical capability.

## Disclaimer
This project uses an educational/simulated cloud environment. It does not claim that any real cloud tenant, provider or customer environment was audited or certified.
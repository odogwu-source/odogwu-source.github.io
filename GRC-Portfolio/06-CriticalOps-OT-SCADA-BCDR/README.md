# Project 06 — OT/SCADA Business Continuity & Disaster Recovery Assessment

## Executive Summary
This hands-on GRC simulation assessed business continuity and disaster recovery risks in a critical-infrastructure environment supporting SCADA, water-treatment and power-grid operations. The work covered critical-asset identification, continuity-risk assessment, ISO/IEC 27001 control mapping, control-effectiveness evaluation, and planned recovery exercises. The assessment prioritized ransomware, extended power failure, backup failure and physical-site disruption as material threats to operational resilience.

## Scenario
CriticalOps Inc operates operational-technology services supporting water-treatment facilities and power-grid substations. The simulated engagement required a structured BCDR assessment of the systems and dependencies needed to sustain or restore essential operations following cyber, infrastructure or physical disruption.

## Objectives
- Identify and classify five critical production assets.
- Assess four material continuity risks using likelihood and impact scoring.
- Document eight business-continuity and disaster-recovery controls.
- Map relevant safeguards to ISO/IEC 27001 controls.
- Evaluate control effectiveness and identify resilience gaps.
- Plan two exercises to validate technical recovery and organizational continuity.

## Environment / Tools
- Educational GRC simulation platform
- Risk register and 5x5 likelihood-impact scoring
- Control catalog and effectiveness assessment
- ISO/IEC 27001:2022 control mapping
- Business Impact Analysis and BCDR concepts
- SCADA / OT critical-infrastructure scenario

## Methodology
1. Established the critical-asset inventory and ownership.
2. Identified cyber, utility, recovery and physical-disaster threat scenarios.
3. Rated likelihood and impact and calculated risk scores.
4. Documented rationale for each risk decision.
5. Selected and assessed continuity and recovery controls.
6. Mapped controls to relevant ISO/IEC 27001:2022 Annex A references.
7. Identified partially effective controls requiring additional assurance.
8. Planned technical disaster-recovery and business-continuity exercises.

## Investigation or Assessment
Five critical production assets were assessed: the SCADA Control System, Power Grid Control Server, Water Treatment Operations Database, OT Network Infrastructure, and OT Configuration & Backup Repository. Four continuity scenarios were then evaluated: ransomware disruption of SCADA operations, extended power failure, backup failure preventing timely recovery, and physical disaster causing facility unavailability.

## Key Findings
- Ransomware disruption of SCADA and critical operational systems was the highest-rated scenario at **20/25 — Critical**.
- Extended power failure was rated **15/25 — High** because a prolonged outage could disable multiple critical OT dependencies.
- Backup failure preventing restoration was rated **15/25 — High**, highlighting recoverability as a core resilience dependency.
- Physical disaster causing facility unavailability was rated **10/25 — Medium**: less likely, but potentially catastrophic.
- Redundancy and end-to-end recovery assurance were treated as areas requiring continued validation rather than assumed to be fully effective.

## Risk / Security Analysis
The assessment demonstrated that availability and recoverability are dominant risk drivers in critical-infrastructure environments. SCADA services depend on interconnected servers, networks, operational data, power and recovery repositories; therefore a single disruption can propagate across several assets. Risk decisions were documented using likelihood, impact, affected assets and operational consequences rather than severity labels alone.

## Controls and Recommendations
Controls documented included business-continuity planning, ICT readiness for continuity, protected backups and restoration, infrastructure redundancy, incident-response coordination, BCDR testing, emergency communications, and critical-supplier continuity management. Key recommendations are to strengthen IT/OT segmentation, maintain protected recovery copies of critical configurations, test restoration and failover regularly, eliminate remaining single points of failure, enforce strong privileged-access controls, validate supplier recovery capability, and exercise both technical recovery and executive continuity decision-making.

## Evidence
Supporting screenshots document the asset inventory, risk register, likelihood-impact scoring, decision rationales, ISO/IEC 27001 control mappings, effectiveness ratings, and planned BCDR exercises. Evidence is retained as educational simulation output and is not presented as production audit evidence.

## Skills Demonstrated
GRC · OT/ICS and SCADA security · critical-infrastructure resilience · asset classification · risk-register development · likelihood and impact analysis · BCDR · control-effectiveness assessment · ISO/IEC 27001 control mapping · ransomware risk analysis · backup governance · recovery planning

## Frameworks / Standards Referenced
- ISO/IEC 27001:2022
- A.5.29 — Information security during disruption
- A.5.30 — ICT readiness for business continuity
- A.8.13 — Information backup
- A.8.14 — Redundancy of information processing facilities
- Business continuity and disaster recovery principles

## Project Outcomes
Produced a structured OT/SCADA continuity-risk assessment linking critical assets to threat scenarios, quantified risk decisions, recovery controls and planned validation exercises. The project demonstrates how governance requirements translate into operational resilience decisions in a critical-infrastructure context.

## Lessons Learned
BCDR assurance depends on more than having written plans. Recovery capability must be supported by protected backups, resilient dependencies, clear ownership, tested failover, realistic exercises and evidence that recovery objectives can actually be met.

## Disclaimer
This project was completed in a simulated educational environment for cybersecurity training and portfolio demonstration. The organisation, systems, risks and assessment activities do not represent a production engagement with a real client.
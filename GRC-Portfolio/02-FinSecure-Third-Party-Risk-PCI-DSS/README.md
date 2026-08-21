# FinSecure — Third-Party Risk & PCI DSS

## Executive Summary
This educational GRC simulation demonstrates third-party risk management in a payment-security context. The project assesses how external service providers can introduce security, operational and compliance exposure, with particular attention to PCI DSS-oriented governance, vendor due diligence, control evidence and remediation tracking.

## Scenario
FinSecure relies on third-party providers that may process, store, transmit or otherwise support services involving payment data. The simulated assessment required evaluation of vendor criticality, access, data exposure, control maturity and remediation obligations.

## Objectives
- Identify security and compliance risks introduced by third parties.
- Assess vendor criticality and exposure to payment information.
- Evaluate due-diligence and control evidence.
- Record findings, risk ratings, ownership and remediation actions.
- Support ongoing monitoring and audit readiness.

## Environment / Tools
- Educational GRC practice environment
- Third-party/vendor inventory and assessment workflow
- Risk scoring and due-diligence review
- Control-gap and remediation tracking
- Screenshot-based evidence capture

## Methodology
The assessment followed a third-party risk lifecycle: scope the relationship, identify system/data access, assess inherent risk, evaluate control evidence, determine residual exposure, document findings, assign remediation and define ongoing monitoring expectations.

## Investigation or Assessment
The exercise examined vendor dependence and payment-security exposure by considering the sensitivity of information involved, the provider's access and responsibilities, the adequacy of controls and the evidence available to support assurance. Findings were documented and prioritised according to risk.

## Key Findings
- Third-party access can create material security and compliance risk even when core services are outsourced.
- Vendor criticality should reflect business dependency and data exposure, not supplier size alone.
- Due-diligence evidence is essential for assessing whether controls are operating as represented.
- Remediation and monitoring are necessary because vendor risk changes throughout the relationship lifecycle.

## Risk / Security Analysis
The project demonstrates the distinction between inherent third-party risk and the exposure that remains after relevant controls are considered. Payment-data access, service dependency, weak controls or insufficient evidence can increase the potential for confidentiality, integrity, availability and compliance impacts.

## Controls and Recommendations
- Maintain a risk-tiered third-party inventory.
- Perform security and compliance due diligence before onboarding critical vendors.
- Define contractual security, incident-notification and evidence requirements.
- Track identified control gaps and remediation commitments.
- Reassess critical vendors periodically and after material service changes.
- Validate PCI DSS responsibilities where payment-card data or services are in scope.

## Evidence
The `screenshots/` directory contains 18 captured artefacts documenting the completed vendor-risk and PCI DSS simulation workflow.

[View the evidence screenshots](./screenshots/)

## Skills Demonstrated
- Third-party risk management (TPRM)
- PCI DSS governance concepts
- Vendor due diligence
- Risk assessment and prioritisation
- Control-gap identification
- Remediation tracking
- Evidence review
- Risk ownership and accountability
- Compliance documentation

## Frameworks / Standards Referenced
- PCI DSS governance and third-party responsibility concepts
- Risk-based third-party security assessment practices

## Project Outcomes
The project produced a structured vendor-risk assessment trail connecting supplier criticality, payment-data exposure, control evidence, findings, accountable ownership and remediation activities.

## Lessons Learned
Outsourcing a service does not outsource accountability. Effective third-party risk management requires visibility into what the vendor can access, how critical the relationship is, what controls exist, what evidence supports them and how identified deficiencies will be monitored to closure.

## Disclaimer
This project was completed in an educational/practice simulation environment for cybersecurity skills development and portfolio demonstration. It is not a PCI DSS assessment, attestation of compliance, production audit or client engagement.
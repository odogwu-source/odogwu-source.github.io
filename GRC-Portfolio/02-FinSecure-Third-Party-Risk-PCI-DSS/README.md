# FinSecure — Third-Party Risk & PCI DSS

## Executive Summary
This educational GRC simulation demonstrates third-party risk management (TPRM) in a payment-security context. The project assesses how external service providers can introduce security, operational and compliance exposure, with particular attention to vendor due diligence, service/data access, control evidence, responsibility allocation, remediation tracking and PCI DSS-oriented governance.

## Assessment Snapshot

| Measure | Result |
| --- | --- |
| Assessment domain | **Third-Party / Vendor Risk Management** |
| Compliance context | **Payment security / PCI DSS** |
| Evidence artefacts | **18 screenshots** |
| Primary activities | **Due diligence, risk assessment, control review, remediation tracking** |
| Governance focus | **Vendor accountability and ongoing monitoring** |

## Scenario
FinSecure relies on third-party providers that may process, store, transmit or otherwise support services involving payment data. The simulated assessment required evaluation of vendor criticality, access, data exposure, control maturity and remediation obligations.

## Objectives
- Identify security and compliance risks introduced by third parties.
- Assess vendor criticality and exposure to payment information.
- Evaluate due-diligence and control evidence.
- Record findings, risk ratings, ownership and remediation actions.
- Clarify security responsibilities between the organisation and relevant service providers.
- Support ongoing monitoring and audit readiness.

## Environment / Tools
- Educational GRC practice environment
- Third-party/vendor inventory and assessment workflow
- Risk scoring and due-diligence review
- Control-gap and remediation tracking
- Screenshot-based evidence capture

## Methodology
The assessment followed a third-party risk lifecycle: scope the relationship, identify system/data access, assess inherent risk, evaluate control evidence, determine remaining exposure, document findings, assign remediation and define ongoing monitoring expectations.

The methodology also considered the governance principle that use of a third-party service provider does not remove the organisation's responsibility to understand and manage the security and compliance implications of the outsourced service.

## Assessment
The exercise examined vendor dependence and payment-security exposure by considering the sensitivity of information involved, the provider's access and responsibilities, the adequacy of controls and the evidence available to support assurance. Findings were documented and prioritised according to risk.

Particular attention was given to whether vendor relationships were supported by appropriate due diligence, documented responsibilities, evidence of relevant controls and a mechanism for tracking remediation and continuing oversight.

## Key Findings
- Third-party access can create material security and compliance risk even when core services are outsourced.
- Vendor criticality should reflect business dependency and data exposure, not supplier size alone.
- Due-diligence evidence is essential for assessing whether controls are operating as represented.
- Security and compliance responsibilities should be explicitly understood rather than assumed to transfer to the vendor.
- Remediation and monitoring are necessary because vendor risk changes throughout the relationship lifecycle.

## Risk / Security Analysis
The project demonstrates the distinction between inherent third-party risk and the exposure that remains after relevant controls are considered. Payment-data access, service dependency, weak controls or insufficient evidence can increase the potential for confidentiality, integrity, availability and compliance impacts.

For PCI DSS-oriented governance, the assessment emphasises maintaining visibility over third-party relationships, conducting due diligence, documenting responsibilities and monitoring relevant compliance status. These are governance activities rather than a claim that this simulation constitutes a formal PCI DSS assessment.

## Controls and Recommendations
- Maintain a risk-tiered inventory of relevant third-party service providers and the services they provide.
- Perform security and compliance due diligence before onboarding critical vendors.
- Maintain written agreements that clearly define relevant security responsibilities.
- Document which security/compliance responsibilities are handled by the organisation and which are handled by the provider.
- Track identified control gaps and remediation commitments to closure.
- Reassess critical vendors periodically and after material service changes.
- Monitor relevant provider compliance/assurance status on an ongoing basis, including at least annual review where PCI DSS obligations apply.
- Retain evidence supporting vendor-risk decisions and assurance conclusions.

## Evidence
The `screenshots/` directory contains **18 captured artefacts** documenting the completed vendor-risk and PCI DSS simulation workflow. The evidence sequence supports the portfolio narrative without representing the exercise as a formal PCI DSS attestation or production audit.

[View the evidence screenshots](./screenshots/)

## Skills Demonstrated
- Third-party risk management (TPRM)
- PCI DSS third-party governance concepts
- Vendor due diligence
- Vendor criticality and exposure analysis
- Risk assessment and prioritisation
- Control-gap identification
- Responsibility allocation
- Remediation tracking
- Evidence review
- Risk ownership and accountability
- Compliance documentation and ongoing monitoring

## Frameworks / Standards Referenced
- PCI DSS third-party service-provider governance concepts, particularly Requirement 12.8 principles
- Risk-based third-party security assessment practices

## Project Outcomes
The project produced a structured vendor-risk assessment trail connecting supplier criticality, payment-data exposure, control evidence, findings, accountable ownership, responsibility allocation and remediation activities. The **18 evidence artefacts** provide a traceable record of the completed simulation workflow.

## Lessons Learned
Outsourcing a service does not outsource accountability. Effective third-party risk management requires visibility into what the vendor can access, how critical the relationship is, what controls exist, what evidence supports them, who is responsible for applicable security obligations and how identified deficiencies will be monitored to closure.

## Disclaimer
This project was completed in an educational/practice simulation environment for cybersecurity skills development and portfolio demonstration. It is not a PCI DSS assessment, attestation of compliance, production audit or client engagement.
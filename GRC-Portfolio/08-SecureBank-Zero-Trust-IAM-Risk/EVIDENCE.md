# Project 08 Evidence Index — SecureBank Zero Trust, IAM & Access-Control Risk

This evidence index identifies the strongest screenshots to retain for the portfolio case study. The screenshots originate from an educational GRC simulation and are presented as training evidence rather than production banking-assurance evidence.

## Recommended Evidence Set

1. **Asset inventory dashboard** — six total production assets, all classified Critical.
2. **Core Banking System asset record** — Critical application.
3. **Customer Database asset record** — Critical database containing sensitive banking data.
4. **Online Banking Portal asset record** — Critical customer-facing application.
5. **Payment Processing System asset record** — Critical financial-transaction application.
6. **Identity and Access Management System asset record** — Critical security dependency.
7. **Security Monitoring and SIEM Platform asset record** — Critical detection and visibility platform.
8. **Completed asset inventory table** — demonstrates asset ownership, classification and production environment.
9. **Privileged Account Compromise risk** — 4 × 5 = 20, Critical, with rationale.
10. **Compromised privileged-credentials threat statement** — explicit threat-vulnerability-impact formulation.
11. **Weak-authentication risk affecting Online Banking Portal** — 4 × 5 = 20, Critical.
12. **Online Banking risk rationale** — account takeover, fraudulent transaction and customer-data exposure consequences.
13. **Malicious-insider/excessive-privilege risk affecting Customer Database** — 3 × 5 = 15, High.
14. **Insider-risk rationale** — least-privilege and sensitive-data implications.
15. **Excessive privilege risk affecting Payment Processing System** — 3 × 5 = 15, High.
16. **Payment-processing risk rationale** — fraud, financial loss, service disruption and regulatory exposure.
17. **IAM privilege-escalation risk** — 4 × 5 = 20, Critical.
18. **IAM privilege-escalation rationale** — demonstrates systemic access risk across multiple banking systems.

## Recruiter-Facing Evidence Order

Lead with the completed six-asset inventory, then show privileged-account compromise, weak authentication, insider/excessive privilege and IAM privilege escalation. This creates a coherent story: **critical banking assets → identity/access threats → likelihood-impact scoring → business consequences → Zero Trust/IAM control needs**.

## Evidence Handling Note

Use screenshots with readable completed fields and decision rationales. Avoid duplicate entry screens unless they prove a different assessment step. Suggested filenames include `01-critical-asset-inventory.png`, `02-privileged-account-critical-risk.png`, `03-online-banking-authentication-risk.png`, `04-insider-database-risk.png`, `05-payment-processing-privilege-risk.png`, and `06-iam-privilege-escalation-risk.png`.

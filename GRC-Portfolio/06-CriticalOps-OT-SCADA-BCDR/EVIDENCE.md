# Project 06 Evidence Index — CriticalOps OT/SCADA BCDR

This evidence index identifies the strongest screenshots to retain for the portfolio case study. The screenshots originate from an educational GRC simulation and should be presented as training evidence, not production audit evidence.

## Recommended Evidence Set

1. **Asset inventory dashboard** — five total production assets, all classified Critical.
2. **SCADA Control System asset record** — critical application supporting water-treatment and power-grid operations.
3. **Power Grid Control Server asset record** — critical production server supporting substation control.
4. **Water Treatment Operations Database asset record** — critical operational database.
5. **OT Network Device asset record** — critical network infrastructure connecting SCADA, control servers and field devices.
6. **OT Configuration & Backup Repository asset record** — critical recovery datastore.
7. **Completed asset inventory table** — demonstrates classification, ownership and production environment.
8. **Risk register summary** — four open risks with one Critical, two High and one Medium rating.
9. **Ransomware risk assessment** — 4 × 5 = 20, Critical.
10. **Extended power failure risk assessment** — 3 × 5 = 15, High.
11. **Backup failure risk assessment** — 3 × 5 = 15, High.
12. **Physical-disaster risk assessment** — 2 × 5 = 10, Medium.
13. **Business Continuity Plan control** — ISO/IEC 27001 A.5.29, Effective, annual review.
14. **ICT Readiness for Business Continuity control** — ISO/IEC 27001 A.5.30.
15. **Backup and Recovery of Critical Systems control** — ISO/IEC 27001 A.8.13, Effective, continuous.
16. **Backup control rationale** — demonstrates testability, monitoring and restoration assurance.
17. **Redundancy of Critical Processing Facilities control** — ISO/IEC 27001 A.8.14, Partially Effective.

## Recruiter-Facing Evidence Order

Use the completed asset inventory first, followed by the risk-register summary, the ransomware risk, one continuity risk, the backup/recovery control, and the partially effective redundancy control. This tells a clear story: **assets → risks → scoring → controls → effectiveness gap**.

## Evidence Handling Note

Retain screenshots with readable field values and remove duplicates, empty forms and transitional screens. Where possible, use descriptive filenames such as `01-asset-inventory-summary.png`, `02-risk-register-summary.png`, `03-ransomware-risk.png`, `04-backup-recovery-control.png`, and `05-redundancy-gap.png`.

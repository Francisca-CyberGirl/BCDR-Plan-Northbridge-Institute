# Task 6 – Governance and performance measures (10 marks)


## 6.1 Ownership, testing, maintenance and framework mapping


The BC/DR Coordinator owns and maintains the plan, the Director approves it, and function owners own their minimum-service procedures. The Registrar, the Technical Recovery Lead and the IRL review it. Testing is risk-based: backup restores most often because they are the weakest evidence today, and a full exercise before the flood season.


*Table 21. Governance arrangements and testing calendar.*


| **Element** | **Arrangement** |
|---|---|
| **Ownership and approval** | Owner: BC/DR Coordinator. Approver: Director. Reviewers: Registrar, Technical Recovery Lead, IRL, function owners. Risk acceptance as in Table 10. |
| **Testing calendar** | Monthly: sample restore of DB01 and FS01, and a call-tree test. Quarterly: full DB01 and application restore and a tabletop with a rotating scenario. Six-monthly: alternate facility test (25 workstations). Yearly: full failover exercise in March, before the heavy rains. |
| **Review triggers** | After any real incident or exercise; a change in systems, suppliers, the site or the alternate facility; a failed test; an audit finding; a published flood warning; or a change to RTO, RPO or legal retention. |
| **Training** | Induction for plan roles within 30 days; yearly role training; invigilator and coordinator briefing before each exercise; two technicians cross-trained with a vendor contact. |
| **Version control and distribution** | Version number, change log, approval signature and effective date on every copy; superseded copies archived. Controlled distribution list; printed copies at the alternate facility and with the Director; offline copies on the phones of plan roles. |


The mapping below is an **educational alignment**, with no claim of conformity or certification. NIST SP 800-34 Rev. 1 sets out seven planning steps (NIST, 2010), and I map them to my outputs; ISO 22301:2019 clauses 4 to 10 are the themes I use for the management-system view (ISO, 2019; ICDFA, n.d., slide 9).


*Table 22. Mapping to NIST SP 800-34 planning steps and ISO 22301 themes.*


| **Framework element** | **Output in this report** |
|---|---|
| NIST step 1: contingency planning policy | Scope, activation thresholds and authority (4.1); ownership (Table 21) |
| NIST step 2: business impact analysis | Task 1 BIA and impact calculations; Task 2 objectives |
| NIST step 3: identify preventive controls | Backup design, separate identities, immutable copy (Task 2) |
| NIST step 4: create contingency strategies | Package comparison and recommendation (Task 3) |
| NIST step 5: develop the contingency plan | Plan, RACI, temporary procedures and runbook (Task 4) |
| NIST step 6: testing, training and exercises | Schedule, tabletop and corrective actions (Task 5); calendar (Table 21) |
| NIST step 7: plan maintenance | Review triggers, version control (Table 21) |
| ISO 22301 clause 4 Context | Flood risk and interested parties (6.1 below); scope |
| ISO 22301 clause 5 Leadership | Director approval, roles and RACI (Table 12) |
| ISO 22301 clause 6 Planning | Objectives (Table 4); corrective actions (Table 20) |
| ISO 22301 clause 7 Support | Resources, training, communication (Tables 14 and 21) |
| ISO 22301 clause 8 Operation | BIA, strategies, plans and exercising (Tasks 1–5) |
| ISO 22301 clauses 9 and 10 Evaluation and improvement | KPIs (Table 23); corrective actions (Table 20) |


**Flood and climate consideration.** ISO 22301:2019 Amendment 1:2024 requires the organisation to determine whether climate change is a relevant issue and notes that interested parties may have climate-related requirements (ISO, 2024; PECB, n.d.). For a flood-prone site (A8), I treat flooding as a recurring risk, not a one-off: a published flood warning triggers pre-emptive activation; the alternate facility and the offsite backup region must be outside the same flood plain; and the yearly exercise is scheduled before the rains.


## 6.2 KPIs and maturity


*Table 23. Key performance indicators.*


| **KPI** | **Formula or method** | **Data source** | **Target** | **Frequency** | **Owner** |
|---|---|---|---|---|---|
| Recovery performance | Functions restored within RTO ÷ functions tested × 100; also elapsed ÷ RTO per function | Exercise and drill log | 100% within RTO; elapsed ≤ 80% of RTO by month 12 | Quarterly | Technical Recovery Lead |
| RPO attainment | Tests where restored checkpoint age ≤ RPO ÷ tests × 100 | Restore test reports | 100% | Quarterly | Technical Recovery Lead |
| Restoration-test coverage | In-scope systems (7) with a successful full restore in the last 90 days ÷ 7 × 100 | Test register | 100% within 6 months | Monthly | BC/DR Coordinator |
| Corrective-action closure | Actions closed by target date ÷ actions due × 100 | Action tracker | ≥ 90%; none overdue by more than 30 days | Monthly | BC/DR Coordinator |
| Training | Plan roles trained and exercised in the last 12 months ÷ plan roles × 100 | Training record | 100% | Quarterly | Registrar |
| Backup verification | Backups verified ÷ backups scheduled × 100 | Backup logs | ≥ 99% | Weekly | Technical Recovery Lead |


**Maturity.** I use the five-level maturity model on slide 13 (1 initial, 2 developing, 3 defined, 4 managed, 5 optimised) (ICDFA, n.d., slide 13). I assess Northbridge at **level 2, developing**: roles, objectives and a drill exist, but restores are untested in 90 days, the administrator identity is shared, the exam workaround is unproven and RTOs are not validated at the alternate site. A realistic 12-month target is **level 3, defined**: restore tests for all seven systems, separate identities, a rehearsed workaround, actions closed on time and quarterly KPI reporting. Level 4 needs a year of measured results.

---

[⬅ Back to README](../README.md) | [⬅ Previous: Task 5 – Recovery schedule and exercise (15 marks)](05-task5-schedule-and-tabletop.md) | [Next: References ➡](07-references.md)

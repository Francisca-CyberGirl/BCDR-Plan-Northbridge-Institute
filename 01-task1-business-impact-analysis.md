# Task 1 – Business Impact Analysis (20 marks)


## 1.1 Five-function BIA


The BIA covers the five functions that the main-site outage stops. For each, Table 2 records the owner, users, resources, dependencies and minimum acceptable service, with the people, premises, supplier and information needs. Minimum service is the least that the business can accept without unmanaged harm; where it relies on a manual process, I name the approval and record controls (NIST, 2010, BIA step; ICDFA, n.d., slide 5).


*Table 2. Business Impact Analysis, F1–F5 (impact over time uses the supplied NGN-per-hour rates).*


|   | **F1 Teaching and assessment** | **F2 Fees and finance** | **F3 Admissions and registration** | **F4 Staff payroll** | **F5 Records and certificates** |
|---|---|---|---|---|---|
| **Owner role** | Head of Examinations | Finance Manager | Admissions Manager | HR and Payroll Manager | Registrar |
| **Users affected** | 1,800 learners (120 in 11:00 exam), teaching staff | Learners paying fees, finance staff, 90 staff for receipts | Applicants, admissions staff, learners changing registration | 90 staff | Learners and alumni requesting records, records staff |
| **Critical resources (systems)** | LMS01, DB01 | FIN01, DB01 | REG01, DB01 | HR01, DB01 | FS01 |
| **Shared dependencies** | Recovery network, AUTH01, secure access, communication, staff | Same, plus payment provider portal | Same | Same, plus bank submission channel | Network, AUTH01, FS01, staff |
| **People** | Invigilators, 2 technicians, business coordinators | Finance officers (dual control) | Admissions staff | HR and finance approvers | Records officers |
| **Premises** | Alternate facility (25 workstations from 10:00), learners’ own devices | Remote work or alternate facility | Remote work | Remote work | Remote work |
| **Suppliers** | Alternate facility, ISP and mobile data, Microsoft | External payment provider | Microsoft (online forms) | Bank | Offsite backup provider |
| **Information needs** | Approved question set, learner list, accessibility needs | Provider transaction reports, fee ledger | Applications received, closing list | Last approved payroll, approved changes | Request log, access list |
| **Impact over time** | Exam missed at 11:00 (3 h): NGN 120,000 one-off. NGN 60,000/h; learner access and assessment integrity at risk | Fee confirmation due 16:00 (8 h). NGN 40,000/h; unreconciled receipts | Admissions close 18:00 (10 h). NGN 20,000/h; lost applicants | Bank file due 12:00 Day 1 (28 h). NGN 10,000/h; wrong or duplicate pay | 48 h service commitment. NGN 5,000/h; integrity and access control risk |
| **Minimum acceptable service** | Authenticated exam delivery via temporary tool with invigilation; read access to course materials | Manual receipt register using provider reports, dual control; no wallet edits | Online application form with restricted list; closing time honoured or formally extended | Last approved payroll plus dual-approved changes only; single bank file | Request queue logged; urgent issues signed by Registrar after verification |
| **Priority** | 1 | 2 | 3 | 4 | 5 |


## 1.2 Worked impact calculations


The hourly figures are separate, non-overlapping incremental costs, so they add. The combined rate is 60,000 + 40,000 + 20,000 + 10,000 + 5,000 = NGN 135,000 per hour. With no recovery or continuity measures the examination at 11:00 is missed in both periods, so the one-off NGN 120,000 is added once.


*Table 3. Disruption cost with no measures (NGN). Cost = rate × hours; the exam trigger is added once.*


| **Function** | **Rate/h** | **8 h calculation** | **8 h cost** | **24 h calculation** | **24 h cost** |
|---|---|---|---|---|---|
| F1 Teaching and assessment | 60,000 | 60,000 × 8 | 480,000 | 60,000 × 24 | 1,440,000 |
| F2 Fees and finance | 40,000 | 40,000 × 8 | 320,000 | 40,000 × 24 | 960,000 |
| F3 Admissions and registration | 20,000 | 20,000 × 8 | 160,000 | 20,000 × 24 | 480,000 |
| F4 Staff payroll | 10,000 | 10,000 × 8 | 80,000 | 10,000 × 24 | 240,000 |
| F5 Records and certificates | 5,000 | 5,000 × 8 | 40,000 | 5,000 × 24 | 120,000 |
| Subtotal (hourly) | 135,000 | 135,000 × 8 | 1,080,000 | 135,000 × 24 | 3,240,000 |
| Examination rescheduling (one-off) | – | Trigger met (no service by 11:00) | 120,000 | Trigger met | 120,000 |
| **Total estimated disruption cost** |   | 1,080,000 + 120,000 | **1,200,000** | 3,240,000 + 120,000 | **3,360,000** |


**Limitations.** First, the rates are exercise estimates and are linear, whereas real harm is uneven: a missed payroll or fee deadline costs far more than an average hour, and the first hours of a quiet night cost less. Second, the figures include only the supplied incremental costs and omit recovery spending, overtime, lost future enrolment and contractual penalties, so they neither measure nor cap the true loss. Financial impact must also be kept apart from learner welfare (120 learners’ examination and assessment fairness), reputation, and **data integrity** (duplicate payroll, lost receipts, altered records), none of which the hourly rates price. I therefore treat integrity and welfare as constraints on how fast I recover, not as items to trade for money.


## 1.3 Priorities and dependency risks


**Business priority** follows deadline and cost: F1 first because the 11:00 examination is 3 hours away and the rate is highest, then F2 (16:00, 40,000/h), F3 (18:00), F4 (12:00 Day 1) and F5 (48 hours). **Technical restoration order** is different because the systems share infrastructure (ICDFA, n.d., slide 5): the recovery network, AUTH01, and the security and backup checks must precede DB01, and every application needs DB01 (F5 needs only AUTH01 and FS01). The order is therefore: network, AUTH01, security and backup checks, DB01, then LMS01, FIN01, REG01, HR01 in business order, with FS01 restored on the second technician during the DB01 restore because it does not delay the critical path.


Restoration alone cannot protect the 11:00 examination: Task 5 shows the fastest assessment service is 11:45, so the examination gets a continuity workaround (Task 4) and DB01 restoration serves the later deadlines.


**Dependency risks:**


- **DB01 is a single point of failure.** F1–F4 all wait for its 90-minute restore, and one corrupt incremental stops them all.
- **Identity split.** Microsoft 365 is available but local applications use AUTH01, whose snapshot is 8 hours old, so accounts or passwords changed since 00:00 may not match.
- **Shared administrator identity.** Production and backup administration share one identity, so one compromise could damage both, and the offsite provider’s access independence is undocumented.
- **Two technicians.** Only two people can run actions, fatigue grows over 72 hours (or longer), and an absence halves capacity.
- **Alternate facility capacity and mobile internet.** 25 workstations serve 40 learners who need them, and M365 reaches everyone only through mobile data.

---

[⬅ Back to README](../README.md) | [⬅ Previous: Reading guide and assumptions](00-reading-guide-and-assumptions.md) | [Next: Task 2 – Recovery objectives and backup design (15 marks) ➡](02-task2-objectives-and-backup-design.md)

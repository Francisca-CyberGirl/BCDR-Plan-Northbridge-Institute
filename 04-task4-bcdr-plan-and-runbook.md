# Task 4 – Business continuity and disaster recovery plan (20 marks)


## 4.1 Coordinated plan: scope, activation and teams


**Scope.** The plan covers the main site, its local systems (LMS01, FIN01, REG01, HR01, FS01, DB01, AUTH01), the five functions and the people and suppliers they need, for outages up to the five-day stress case in Task 5. It is activated when any trigger is met: the site or a critical system is expected to be unavailable for more than 4 hours; a function deadline within 8 hours is at risk; AUTH01 or DB01 is lost; or life-safety or facility closure occurs. The 08:00 outage meets all four.


**Authority and escalation.** The Director is the authorised decision maker and the Registrar the deputy if the Director cannot be reached within 15 minutes (A10). I, as BC/DR Coordinator, can activate the plan on the thresholds above and the Director confirms within 30 minutes. Escalation runs from function owner to me, then to the Director, with an immediate call to the Incident Response Lead (IRL) for any suspected compromise. Contact roles (Director, Registrar, coordinator, technical lead, Facilities, Communications, IRL, function owners, suppliers) are on a printed and an offline call tree.


**Alternative communication and reporting.** Voice calls and SMS are primary because M365 relies on mobile data; M365 Teams or email is secondary, with no sensitive details by email until IR clears it. Situation reports go to the Director every 2 hours for the first 12 hours (10:00, 12:00, 14:00 and so on), then every 4 hours: the status of each function against its RTO and MTD, issues, decisions needed and the next report time.


**How the teams work together.** The continuity team (business coordinators and function owners) runs manual procedures and learner support. The technical recovery team (two technicians) runs the runbook. Facilities manages the site, power, alternate facility access and safety. The IR team (the IRL) joins when compromise is possible. I coordinate the four, hold the single decision log, and decide when technology is restored versus when a service has resumed (NIST, 2010; ICDFA, n.d., slides 12 and 14).


## 4.2 RACI matrix


R is Responsible, A is Accountable, C is Consulted and I is Informed. Each row has exactly one A and at least one R.


*Table 12. RACI (A/R = one role both accountable and responsible).*


| **Activity** | **DIR** | **BCDRC** | **TRL** | **BC** | **FO** | **FAC** | **IRL** | **COM** |
|---|---|---|---|---|---|---|---|---|
| **Plan activation** | A | R | C | I | I | C | I | I |
| **Temporary assessment delivery** | I | C | C | R | A | C | I | C |
| **Backup selection and validation** | I | C | A/R | – | I | – | C | – |
| **System restoration (runbook)** | I | C | A/R | – | I | C | C | – |
| **Business acceptance and service resumption** | I | R | C | I | A | – | I | I |
| **Stakeholder communication** | C | C | I | R | C | I | I | A |
| **Incident response escalation** | I | C | R | – | I | – | A | I |
| **Facilities clearance and site safety** | I | C | I | – | I | A/R | – | I |
| **Return to the primary site** | A | R | R | C | C | R | C | I |
| **Situation reporting** | I | A/R | C | R | C | C | I | I |


*Key: DIR Director; BCDRC BC/DR Coordinator; TRL Technical Recovery Lead (senior technician); BC business coordinators (continuity team); FO function owner (Head of Examinations for F1); FAC Facilities Manager; IRL Incident Response Lead; COM Communications Lead.*


## 4.3 Temporary procedures for F1–F5 (72-hour outage)


The temporary procedures keep each function at its minimum service until its system is accepted. The alternate facility offers 25 workstations from 10:00, but 40 of the 120 examination learners need one, so I run two sittings: sitting 1 at 11:00 for 25 centre learners and the 80 learners using their own devices, and sitting 2 at 12:30 for the remaining 15 (A1, A4). Learners with accessibility needs are placed in sitting 1 and get their adjustments, such as extra time or assistive tools. Sitting 2 learners are held apart and receive a different paper version, to protect assessment integrity.


*Table 13. Temporary procedures, controls and resources.*


| **Function** | **Temporary procedure** | **Approvals and controls** | **Records and reconciliation** | **Minimum staff, equipment, supplier** |
|---|---|---|---|---|
| F1 Exam | Deliver the approved question set through the M365 assessment tool (A2). Sign-in with M365 identity, one attempt, time-limited, randomised order. Invigilate centre learners in person and own-device learners on a video call. | Head of Examinations approves the tool and the go/no-go by 09:45. Invigilator checklist; incident log for any irregularity. Privacy: access restricted to examination staff; no papers or answers on personal devices. | Responses exported to a restricted folder. After restoration, import to LMS01 and check a sample of 10% against the originals. Delete temporary copies after sign-off. | 5 invigilators, 2 coordinators, 1 technician on call; 25 workstations, hotspots, power banks; alternate facility, Microsoft |
| F2 Fees | Record each receipt in a restricted register using the payment provider’s transaction report. No wallet or ledger edits. Confirm fees by 16:00 from the provider report. | Two people per entry (record, approve). Unique provider reference prevents duplicates. | On FIN01 restoration, reconcile the register to the provider report and to FIN01 totals; investigate every difference. | 2 finance officers, 1 approver; payment provider portal |
| F3 Admissions | Receive applications and registration changes through an online form with restricted access, open until 18:00. | Admissions Manager approves any extension; Registrar approves a deadline change. Only admissions staff see the list. | Import to REG01 after restoration; remove duplicates by applicant ID and email; confirm counts. | 2 admissions staff; Microsoft |
| F4 Payroll | Freeze changes except urgent, dual-approved ones on a signed form. Build the payroll file from HR01 once restored; otherwise from the last approved run plus approved changes. | HR Manager and Finance Manager both approve. One bank submission only, by 12:00 Day 1, after a duplicate check. | Compare totals with the last run; log every manual change; correct any error through the bank within one cycle. | HR Manager, 1 finance approver; bank channel |
| F5 Records | Log requests in a controlled queue. Issue certificates only after FS01 is restored and the Registrar has verified the file. | Registrar signs urgent requests. Access follows AUTH01 groups; no records by personal email. | Match issued items to the queue; no request older than 48 hours. | 2 records officers; offsite backup provider |


## 4.4 DR runbook


The runbook follows the drill sequence. After each restore a snapshot is taken **before** acceptance, so rollback is possible. A step is finished only when its completion check passes. Restoring technology (a check on the system) is separate from resuming the business service (acceptance by the function owner).


*Table 14. DR runbook.*


| **#** | **Action** | **Prerequisite** | **Owner** | **Completion check** | **Contingency if the step fails** |
|---|---|---|---|---|---|
| 1 | Activate the plan; assign technicians T1 and T2; open the decision log | Director decision | BCDRC | Log open; roles confirmed by call | Deputy decides; start without confirmation if 30 min pass |
| 2 | Build the recovery network (30 min) | None | TRL (T1) | Connectivity, DNS and firewall tested; mobile-data link tested | Use the alternate facility network; call the ISP |
| 3 | Restore AUTH01 from B03 (30 min) | Step 2 | TRL (T1) | Directory starts; test user and administrator log on with the separate backup-admin identity | Use the previous snapshot; use M365 identity for exam access meanwhile |
| 4 | Security and backup checks (30 min): verify checksums and chain (B01 plus incrementals), malware scan, review backup-admin logons; select the restore point | Step 3 | TRL (T1) | Chain complete from B01 to the latest point; no unexplained administrator activity | Fall back to the last complete point (04:00 or B01); if activity is unexplained, stop and call the IRL |
| 5 | Restore DB01 (90 min); snapshot | Step 4 | TRL (T1) | Consistency check; row counts and control totals match the last report | Restore the next-oldest point; last resort is the 17:00 Day −3 full with manual reconciliation |
| 6 | Restore FS01 (60 min) in parallel; snapshot | Steps 3 and 4 | T2 | Files open; permissions match AUTH01 groups | Restore the previous full backup; keep manual F5 queue |
| 7 | Restore LMS01 (45 min); snapshot | Step 5 | T1 | Login works; test submission saved and read back | Keep M365 assessment tool; rebuild from the application image |
| 8 | Restore REG01 (30 min) and HR01 (45 min); snapshot | Step 5 | T2 | Test application accepted; payroll totals match the last run | Keep the manual form and the last approved payroll file |
| 9 | Restore FIN01 (30 min); snapshot | Step 5 | T1 | Test receipt posts; ledger agrees with provider report to 07:00 | Keep the manual register |
| 10 | Business acceptance by each function owner | Steps 6–9 | FO (BCDRC coordinates) | Written sign-off against the Table 4 service level | Stay on the manual procedure; fix and retest |
| 11 | Rollback if acceptance fails or a new compromise appears | Snapshot | TRL | Service reverts to the snapshot or manual procedure; decision logged | Isolate the system and escalate to the IRL |
| 12 | Declare recovery and update stakeholders | Step 10 | BCDRC | Declaration time and function status in the log and report | Delay declaration until sign-off |


## 4.5 Communications, incident response and return to site


*Table 15. Stakeholder communication matrix.*


| **Stakeholder** | **Trigger** | **Message owner** | **Channel (primary / alternative)** | **Approver** | **Frequency** |
|---|---|---|---|---|---|
| Learners | Activation; any change to exam or services | COM | SMS and M365 email / phone call to the exam cohort, public status page | Director | First notice 09:00; then 10:15 and every 4 h |
| Staff | Activation; instructions | COM | SMS and call tree / M365 Teams | BC/DR Coordinator | Every 4 h and at shift change |
| Director and deputy | Activation; decision needed | BCDRC | Call / SMS | – | Situation report every 2 h |
| Alternate facility | Activation; extension | Facilities Manager | Phone / email | Director | On activation and daily |
| Payment provider | Finance restoration or mismatch | Finance Manager | Provider support line / account manager | Finance Manager | On trigger |
| Offsite backup provider | Retrieval request; integrity failure | Technical Recovery Lead | Support line / account manager | BC/DR Coordinator | On trigger |
| Bank (payroll) | Payroll submission or change | HR Manager | Bank channel / phone | Director | On trigger |
| IR team | Possible compromise | BCDRC | Phone / secure channel | Director | Immediately |
| Data-protection regulator and legal adviser | Possible personal-data breach (via IRL and legal adviser) | Legal adviser | Official route / phone | Director | As advised |
| Media | Enquiry | COM | Holding statement | Director | Once, then on approval |


*Initial learner notice (141 words; required 120–180).*


> Notice to learners – Monday, 09:00 WAT
>
> Flooding and a power failure have closed our main site. No one has been hurt. The site is expected to stay closed for about 72 hours.
>
> **Your examination today:** it will go ahead, using a temporary online arrangement approved by the Head of Examinations. If you need a centre workstation, we will text you by 09:30 with your sitting: 25 learners sit at 11:00 at our alternate facility, which opens at 10:00, and the others sit at 12:30. If you are using your own device, charge it, test your Northbridge Microsoft 365 sign-in now, and join at 10:45.
>
> **Support:** tell us about any accessibility or connection need at examsupport@northbridge.example or +234 000 000 0000 (fictional details).
>
> **Safety:** we will never ask for your password. Trust only official messages from us.
>
> Next update: 10:15 WAT.


**When possible compromise brings in incident response.** No cyberattack is confirmed, but any of these triggers an immediate call to the IRL: an unexpected backup-administrator logon, a failed integrity check, ransom or encryption indicators, unexplained changes in restored data, unusual M365 sign-ins, or provider records that disagree with restored records in a way that suggests tampering. I then pause the affected restore step, preserve logs and the backup in question, and keep business continuity running on manual procedures while the IRL leads the investigation (A11).


**Safe return to the primary site.** Return needs written Facilities clearance (structure, dry, electrical safety, stable power for 24 hours, network tested). I freeze changes at a planned cut-over, take a final backup at the recovery site, restore it at the primary site, and move the manual records through a controlled import with counts and control totals checked by each function owner. A day of parallel running follows. Final sign-off by the Director comes only after both Facilities clearance and business validation, and the rollback is to stay at the alternate facility.

---

[⬅ Back to README](../README.md) | [⬅ Previous: Task 3 – Recovery strategy and budget (15 marks)](03-task3-strategy-and-budget.md) | [Next: Task 5 – Recovery schedule and exercise (15 marks) ➡](05-task5-schedule-and-tabletop.md)

# Task 5 – Recovery schedule and exercise (15 marks)


## 5.1 Baseline recovery schedule (modelled)


> **MODELLED BASELINE, NOT A LIVE TEST.** The schedule below applies the supplied drill timings and assumes every backup passes its integrity check. No restoration was performed.


The schedule starts at 08:00 with two technicians (T1, T2). An action takes one technician for its whole duration, cannot be split, and starts only when its prerequisites are complete. The recovery network, AUTH01 and the security checks run in sequence on T1, because each depends on the one before. FS01 needs only AUTH01 and the checks, so T2 restores it while T1 restores DB01, which uses time that would otherwise be idle. After DB01, the four applications total 45 + 30 + 30 + 45 = 150 minutes of work, so two technicians need at least 75 minutes. I order LMS01 first, as the highest priority.


*Table 16. Baseline schedule, current warm-site timings.*


| **Action** | **Technician** | **Start** | **Finish** | **Minutes** | **Prerequisite** |
|---|---|---|---|---|---|
| Recovery network | T1 | 08:00 | 08:30 | 30 | None |
| AUTH01 restoration | T1 | 08:30 | 09:00 | 30 | Recovery network |
| Security and backup checks | T1 | 09:00 | 09:30 | 30 | AUTH01 restoration |
| DB01 restoration | T1 | 09:30 | 11:00 | 90 | Security and backup checks |
| FS01 restoration (F5) | T2 | 09:30 | 10:30 | 60 | AUTH01 and security checks |
| LMS01 restoration (F1) | T1 | 11:00 | 11:45 | 45 | DB01 |
| REG01 restoration (F3) | T2 | 11:00 | 11:30 | 30 | DB01 |
| HR01 restoration (F4) | T2 | 11:30 | 12:15 | 45 | DB01 |
| FIN01 restoration (F2) | T1 | 11:45 | 12:15 | 30 | DB01 |


**Critical path.** Recovery network (30) → AUTH01 (30) → security and backup checks (30) → DB01 (90) → LMS01 (45) = 225 minutes, so **the earliest assessment service is 11:45**, 45 minutes after the examination starts. All five functions are complete by **12:15**, and no schedule can finish earlier: 11:00 plus 75 minutes. The other path (FS01) has 90 minutes of slack.


*Table 17. Elapsed recovery time and comparison with objectives (modelled; elapsed minutes from 08:00).*


| **Function** | **Restored** | **Elapsed** | **RTO** | **Margin to RTO** | **MTD** | **Restored checkpoint age** | **RPO** | **RPO result** |
|---|---|---|---|---|---|---|---|---|
| F1 (LMS01) | 11:45 | 225 (3 h 45) | 240 | 15 | 480 | 1 h | 1 h | Met, no margin |
| F2 (FIN01) | 12:15 | 255 (4 h 15) | 270 | 15 | 480 | 1 h | 1 h | Met, no margin |
| F3 (REG01) | 11:30 | 210 (3 h 30) | 360 | 150 | 600 | 1 h | 1 h | Met, no margin |
| F4 (HR01) | 12:15 | 255 (4 h 15) | 720 | 465 | 1,680 | 1 h | 4 h | Met |
| F5 (FS01) | 10:30 | 150 (2 h 30) | 1,440 | 1,290 | 2,880 | 9 h | 4 h | Not met: 5 h over |


All RTOs and MTDs are met in the baseline, though F1 and F2 have only 15 minutes of margin. F5 misses its RPO by 5 hours because FS01 is restored to 23:00 (Task 2). **Workarounds are needed even though restoration succeeds:** the 11:00 examination (LMS01 arrives at 11:45); F2 receipts between 07:00 and the restoration time; F3 applications entered after 07:00; and F5 changes since 23:00. The 16:00, 18:00 and Day 1 deadlines themselves are met.


## 5.2 Tabletop exercise (simulation)


> **SIMULATION.** I ran this 45-minute tabletop alone on paper, playing each role in turn: BC/DR Coordinator, Director, Technical Recovery Lead, Head of Examinations, Finance Manager, Facilities, IRL and Communications Lead. Nothing was restored and no real person acted. The results are my reasoned simulation and are **kept separate from the baseline schedule**.


**Objectives:** test activation, the examination workaround under capacity change, backup validation with handover to incident response, and finance reconciliation. Minutes map to the scenario times shown (a time jump at minute 40 reaches the restored state). Success criteria: a logged decision within 5 minutes of each inject; no restore from an unvalidated backup; an owner and revised action for every inject.


*Table 18. Stress schedule (separate from the baseline) after the minute-30 inject.*


| **Action** | **Tech.** | **Start** | **Finish** | **Note** |
|---|---|---|---|---|
| Network, AUTH01, security checks | T1 | 08:00 | 09:30 | Failure found at 09:20 |
| Re-validate chain; choose 04:00 differential point | T1 | 09:30 | 10:00 | Adds 30 min |
| Capture backup and logon logs with the IRL | T2 | 09:30 | 10:00 | Evidence for IR |
| DB01 restoration to 04:00 | T1 | 10:00 | 11:30 | 90 min |
| FS01 restoration | T2 | 10:00 | 11:00 | After checks |
| LMS01 | T1 | 11:30 | 12:15 | F1: 255 min, RTO breached by 15 min |
| REG01 | T2 | 11:30 | 12:00 | F3 |
| HR01 | T2 | 12:00 | 12:45 | F4 |
| FIN01 | T1 | 12:15 | 12:45 | F2: 285 min, RTO breached by 15 min |


*Table 19. Simulated decision log (every inject).*


| **Inject (minute, scenario time)** | **Decision and revised action** | **Owner** | **Rationale** | **Effect on service or recovery** |
|---|---|---|---|---|
| 10 – 08:30. Closure extends from 72 hours to five days | Extend the alternate facility booking to 120 hours; set a technician rota (12 hours on, 12 off) with vendor support for relief; re-plan teaching on M365; move the return decision to Day 5 | Director (BCDRC prepares) | Two technicians cannot sustain 120 hours of continuous work; the warm site must outlast the outage | Baseline schedule unchanged; adds a sustainment workload and cost; F4 and F5 deadlines unaffected |
| 20 – 09:00. Only 15 working workstations | Three sittings: 15 centre learners and the 80 own-device learners at 11:00, 15 at 12:30, 10 at 14:00; accessibility needs first; three paper versions; ask the facility to repair within the day | Head of Examinations (FO) | Preserves fairness and integrity; keeps the exam on Day 0; avoids the NGN 120,000 trigger if A2 holds | Last sitting ends 15:00 (A1); more invigilators; learner notice updated at 09:30 |
| 30 – 09:20. Incremental fails its check; unexpected backup-admin logon | Stop the DB01 start; call the IRL; re-validate the chain and restore to the 04:00 differential; capture logs; use the Task 5 stress schedule | BC/DR Coordinator (IRL accountable for the investigation) | A chain with a bad file or tampering could damage restored data; the logon may be a compromise | LMS01 12:15 and FIN01 12:45: both 15 min over RTO; data loss 4 h, 3 h over RPO; exam unaffected through the workaround |
| 40 – 13:00. Provider shows successful payments missing from restored finance records | Freeze receipts and wallet credits; reconcile every provider payment after 04:00 to FIN01 with dual approval; post missing items by unique reference; check whether any payment before 04:00 is missing | Finance Manager | The restore point is 04:00, so a gap after it is expected; one before it would suggest tampering | F2 acceptance delayed to 15:00 (reconciliation); no double charges; if earlier payments are missing, the IRL is called again |


## 5.3 Corrective actions


The simulation shows five weaknesses: shared administrator identity, an unproven exam workaround, a DB01 chain that depends on every incremental, a critical path with no slack, and weak finance reconciliation. Actions are dated assuming the exercise is run on 23 October 2026 (A9).


*Table 20. Corrective actions.*


| **#** | **Action** | **Priority** | **Owner** | **Target date** | **Measurable completion evidence** |
|---|---|---|---|---|---|
| 1 | Separate backup and production admin identities, add MFA, and document provider access independence | High | Technical Recovery Lead | 6 Nov 2026 | Access review shows no shared identity; delete test against the offsite copy fails and is logged |
| 2 | Pre-stage and rehearse the M365 assessment workaround with the question set | High | Head of Examinations | 13 Nov 2026 | Mock exam with 105 users completed; 100% of responses exported; sign-off recorded |
| 3 | Implement the new DB01 schedule (04:00 and 12:00 differentials) with a nightly chain check | High | Technical Recovery Lead | 20 Nov 2026 | Seven consecutive nights of verified chains; one timed restore report |
| 4 | Shorten the critical path: pre-stage the network and AUTH01 images; time a drill to LMS01 ≤ 180 min | Medium | BC/DR Coordinator | 22 Dec 2026 | Timed drill log shows LMS01 within 180 min |
| 5 | Write and test a payment-provider reconciliation procedure and a daily settlement export | Medium | Finance Manager | 20 Nov 2026 | Dry run reconciles 100% of test payments within 2 hours |
| 6 | Obtain a signed alternate facility addendum guaranteeing 25 working workstations and a 5-day option | Medium | BC/DR Coordinator | 27 Nov 2026 | Signed addendum; on-site test shows 25 working machines |

---

[⬅ Back to README](../README.md) | [⬅ Previous: Task 4 – Business continuity and disaster recovery plan (20 marks)](04-task4-bcdr-plan-and-runbook.md) | [Next: Task 6 – Governance and performance measures (10 marks) ➡](06-task6-governance-and-kpis.md)

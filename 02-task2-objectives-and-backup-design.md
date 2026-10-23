# Task 2 – Recovery objectives and backup design (15 marks)


## 2.1 Objectives and recovery declaration


**Recovery Time Objective (RTO)** is the target time from disruption (08:00) to restoring a function at its agreed minimum service. **Recovery Point Objective (RPO)** is the maximum age of recoverable data, the data loss I accept. **Maximum Tolerable Downtime (MTD)** is the point where disruption becomes unacceptable to the business, normally a deadline in the brief (NIST, 2010; ICDFA, n.d., slide 6). RTO must sit inside MTD with a buffer, because the first attempt can fail.


My rule is RTO ≤ 60% of MTD, leaving at least 40% of the tolerable time for failed steps, business acceptance and manual workarounds. MTDs come from deadlines: F2 16:00 (8 h), F3 18:00 (10 h), F4 12:00 Day 1 (28 h), F5 the 48-hour commitment. For F1 I use 8 hours (the end of the working day), because the single 11:00 examination (3 hours) cannot be guaranteed by restoration; it is covered by a workaround in Task 4. RTOs are checked against the current drill: LMS01 is available at 3 h 45 min, so a 4-hour target is achievable but has only a 15-minute margin, and I will not promise a shorter one until a test shows it.


*Table 4. Objective matrix (all times from 08:00 Day 0).*


| **Function** | **MTD** | **RTO** | **Buffer (MTD − RTO)** | **RTO ÷ MTD** | **RPO** | **Recovery declared when** |
|---|---|---|---|---|---|---|
| F1 Teaching and assessment | 8 h (16:00) | 4 h (12:00) | 4 h | 50% | 1 h | LMS01 login works for the exam cohort; a test submission is saved and read back; integrity check passed; Head of Examinations signs |
| F2 Fees and finance | 8 h (16:00) | 4 h 30 min (12:30) | 3 h 30 min | 56% | 1 h | FIN01 posts a test receipt; ledger agrees with the provider report to 07:00; Finance Manager signs |
| F3 Admissions and registration | 10 h (18:00) | 6 h (14:00) | 4 h | 60% | 1 h | REG01 accepts and retrieves a test application; Admissions Manager signs |
| F4 Staff payroll | 28 h (12:00 Day 1) | 12 h (20:00) | 16 h | 43% | 4 h | HR01 payroll totals match the last approved run; dual approval of any changes |
| F5 Records and certificates | 48 h (08:00 Day 2) | 24 h (08:00 Day 1) | 24 h | 50% | 4 h | FS01 files open, permissions match AUTH01 groups; Registrar signs |
| Shared: network / AUTH01 / DB01 | – | 30 min / 1 h / 3 h | – | – | – / 4 h / 1 h | Each passes its own validation before dependants start |


Recovery is **declared** at the minimum-service level in the last column, not when the technology starts. Restoring LMS01 is technical recovery; recovery is declared only after business acceptance, because a restored system that holds stale or corrupt data does not resume the function.


## 2.2 Checkpoint ages and data-loss windows


Using 08:00 on Day 0 as the reference point, the age of each checkpoint is the time between the checkpoint and 08:00.


*Table 5. Checkpoint calculations and gaps against proposed RPOs.*


| **Item** | **Checkpoint** | **Calculation** | **Age at 08:00** | **Potential data-loss window** | **Functions affected** | **Proposed RPO** | **Gap** |
|---|---|---|---|---|---|---|---|
| B02 DB01 (B01 + incrementals) | 07:00 Day 0 | 08:00 − 07:00 | 1 h | 07:00–08:00 Day 0 | F1, F2, F3, F4 | F1–F3: 1 h; F4: 4 h | None (F1–F3 at limit, no margin) |
| B01 DB01 full (fallback base) | 23:00 Day −1 | 24:00 − 23:00 + 08:00 = 1 + 8 | 9 h | 23:00–08:00 | F1–F4 if chain breaks | 1 h / 4 h | 8 h over F1–F3; 5 h over F4 |
| B03 AUTH01 snapshot | 00:00 Day 0 | 08:00 − 00:00 | 8 h | 00:00–08:00 (accounts, groups, settings) | F1–F5 | AUTH01: 4 h | 4 h over |
| B04 FS01 full | 23:00 Day −1 | 1 + 8 | 9 h | 23:00–08:00 | F5 | 4 h | 5 h over |
| Disconnected full | 17:00 Day −3 | 3 × 24 − 9 = 72 − 9 | 63 h | 17:00 Day −3 to 08:00 | Last-resort fallback | – | Not usable for RPO |


DB01 meets its RPOs only if every incremental from B01 to 07:00 passes its check, which is why Task 5 stresses that assumption. AUTH01 (4 h over) and FS01 (5 h over) fail the proposed RPOs, so the backup design below closes those gaps.


**What can be reconciled, and what cannot be assumed.** Payments from 07:00 can be rebuilt from the payment provider’s transaction report (A12). Applications or requests received by email or online forms can be re-entered. M365 audit logs and helpdesk tickets can show account changes since 00:00. I cannot assume that anything held only on local disks (receipts or wallet entries from 07:00 to 08:00, new FS01 files since 23:00, AUTH01 changes) is recoverable, so staff must declare these and I treat them as lost until found.


## 2.3 Backup and restoration design


The design closes the RPO gaps, applies the 3-2-1 rule (three copies, two media types, one offsite) and adds an independent immutable or offline copy that nobody with production administrator access can alter (CISA et al., 2023). The 400,000 add-on in Task 3 pays for the independent copy.


*Table 6. Proposed backup schedule.*


| **Asset** | **Full** | **Differential** | **Incremental** | **Retention** | **Copies and placement** |
|---|---|---|---|---|---|
| DB01 (F1–F4) | Daily 23:00 | 04:00 and 12:00 | Hourly on the hour | 14 daily, 8 weekly, 12 monthly (confirm legal retention) | Copy 1 production; Copy 2 local backup repository (separate disk); Copy 3 encrypted offsite within 15 minutes, in a region outside the flood zone; Copy 4 immutable (object-lock, 30 days) under a separate identity |
| AUTH01 | Snapshot every 4 h and after any change | – | – | 14 days | As above, plus an exported configuration file stored offline |
| FS01 (F5) | Daily 23:00 | – | Every 4 h | 30 daily, 12 monthly | As above; file versioning on |
| LMS01, FIN01, REG01, HR01 (application images) | Weekly and after upgrade | – | – | 8 weeks | Offsite copy plus offline copy |


**Exact DB01 restore chain.** Today, to reach 07:00: B01 (23:00 full) plus every hourly incremental at 00:00, 01:00, 02:00, 03:00, 04:00, 05:00, 06:00 and 07:00 (A3), eight incrementals in sequence. One failed file stops the chain at the point before it. With the proposed schedule, the chain to 07:00 is B01, the 04:00 differential, then incrementals 05:00, 06:00 and 07:00, which is five pieces and a fallback to 04:00 or to B01 even if a later incremental fails.


**Security controls.** Production and backup administration will use **separate identities** with MFA, no standing privilege, and alerting on every backup-administrator logon. Backups are encrypted (AES-256 at rest, TLS in transit); two key custodians hold the keys in a vault that is not only at the flooded site (A5). Restoration is tested monthly (sample restore of DB01 to a test server), quarterly (full DB01 and application restore) and yearly (full failover).


**Why offsite storage or a replica is not proof of recoverability.** An offsite copy proves only that data was copied. It may be incomplete, corrupt, encrypted by attackers, or unreadable without keys, and a live replica copies deletions, corruption and malicious changes within seconds, so it can destroy both sides together. Recovery is proved only when a restore from that copy has produced usable data inside the target time.


*Table 7. Verification of integrity, access independence and usability.*


| **Check** | **Method** | **Frequency** | **Pass criterion** | **Owner** |
|---|---|---|---|---|
| Backup integrity | Checksum on creation and at the offsite copy; list the chain and confirm no gap; catalogue comparison | Every backup; chain check nightly | 100% of files verified; no gaps from base to latest point | Technical Recovery Lead |
| Access independence | Review the provider’s identity and access settings; test that production administrator credentials cannot delete or change the offsite or immutable copy; remove the shared identity | Quarterly and after any staff change | Delete attempt fails and is logged; no shared identity remains | BC/DR Coordinator |
| Usability of restored data | Restore to an isolated network; database consistency check; row counts and control totals against the last production report; application smoke test; function owner signs | Monthly sample; quarterly full | Totals agree; test transaction succeeds; owner sign-off recorded | Function owners |
| Unverified assumptions | A3 (hourly cadence), A5 (provider retrieval and keys), provider independence (undocumented) | Before 30 days | Each confirmed in writing or replaced by a tested alternative | BC/DR Coordinator |

---

[⬅ Back to README](../README.md) | [⬅ Previous: Task 1 – Business Impact Analysis (20 marks)](01-task1-business-impact-analysis.md) | [Next: Task 3 – Recovery strategy and budget (15 marks) ➡](03-task3-strategy-and-budget.md)

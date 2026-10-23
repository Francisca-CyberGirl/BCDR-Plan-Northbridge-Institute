# Task 3 – Recovery strategy and budget (15 marks)


## 3.1 Option comparison


I compared the three packages with the Task 2 targets (F1 RTO 4 h, F2 4 h 30 min, F3 6 h, F4 12 h, F5 24 h; RPO 1 h for F1–F3). The package timings are planning estimates that I must validate, so I do not treat any of them as a guarantee (ICDFA, n.d., slides 7 and 15).


*Table 8. Option matrix against objectives and risks.*


| **Criterion** | **Cold** | **Warm** | **Hot** |
|---|---|---|---|
| **First critical service (estimate)** | 24–48 h | 3–5 h (drill: 3 h 45 min) | About 1 h |
| **Data-loss window (estimate)** | Up to 24 h | 1–6 h | Up to 15 min |
| **Meets RTOs (F1–F5)** | No: fails F1–F4; F5 only at the 24 h edge | Mostly: F1 (4 h) at 3–4 h but not at 5 h; F2–F5 yes | Yes, with large margin |
| **Meets RPO 1 h (F1–F3)** | No (24 h) | Only at the 1 h end; needs the Task 2 backup schedule to hold the claim | Yes (15 min) |
| **Staffing (2 technicians)** | Very high manual build; rebuild for 24–48 h under fatigue | Fits the drill: 2 technicians for about 4 h 15 min | Needs less manual work but continuous monitoring and failover skills |
| **Geography** | Depends on where hardware is delivered | Alternate site must be outside the flood zone (A8) | Second region; network path must be diverse |
| **Supplier dependencies** | Hardware delivery time | Alternate facility, provider restore support | Replication provider, link provider, licences |
| **Corruption or malicious-change risk** | Low propagation, but old data | Moderate: restore point can be chosen | High: live replica copies bad changes within seconds; independent backups still needed |


## 3.2 Itemised annual budget


Every package carries two mandatory additions, NGN 400,000 for independent protected backups and NGN 200,000 for training and tests, a combined NGN 600,000. The annual limit is NGN 3,000,000.


*Table 9. Itemised annual cost (NGN) against the NGN 3,000,000 limit.*


| **Package** | **Base** | **Independent protected backups** | **Training and tests** | **Total** | **Share of limit** | **Headroom** | **Fits limit?** |
|---|---|---|---|---|---|---|---|
| Cold | 600,000 | 400,000 | 200,000 | **1,200,000** | 40% | 1,800,000 | Yes |
| Warm | 1,500,000 | 400,000 | 200,000 | **2,100,000** | 70% | 900,000 | Yes |
| Hot | 2,400,000 | 400,000 | 200,000 | **3,000,000** | 100% | 0 | Yes, exactly at the limit; no contingency |


Totals: Cold 600,000 + 400,000 + 200,000 = 1,200,000; Warm 1,500,000 + 600,000 = 2,100,000; Hot 2,400,000 + 600,000 = 3,000,000. All three fit, but Hot leaves nothing for price changes, extra testing or a fault.


## 3.3 Recommendation


**I recommend the Warm package at NGN 2,100,000.** It is the lowest option that can meet F2–F5 and the 1-hour DB01 data-loss target with evidence, it fits the drill and the two-technician team, and it leaves NGN 900,000 for validation and contingency. Cold cannot meet any short RTO or the 1-hour RPO. Hot meets everything on paper, but it uses the full limit, is exposed to propagated corruption unless the independent backups work, and its claim of an hour is as unproven as Warm’s.


**What Warm cannot meet.** It cannot guarantee LMS01 by 11:00: the drill gives 11:45 and even the best estimate (3 hours) has no margin. The examination is therefore protected by the continuity workaround, not by recovery. F1’s 4-hour RTO has a 15-minute margin, and the 1-hour data-loss window relies on the redesigned backups.


*Table 10. Residual risks and who must accept them.*


| **Residual risk** | **Treatment** | **Accepted by** |
|---|---|---|
| Examination at 11:00 cannot be met by restoration; depends on the temporary tool (A2) | Workaround approved in advance; mock exam tested; NGN 120,000 if it fails | Director |
| F1 RTO margin is 15 minutes; one failed step breaches it | Pre-stage network and AUTH01 images; target 3 h (Task 5 actions) | Director and Registrar |
| Data loss up to 1 h (DB01), 4 h (AUTH01, FS01) and any local-only records | Reconciliation from provider reports and staff declarations | Function owners |
| Supplier recovery claims unproven; alternate facility capacity (25) unverified | Contract addendum and timed acceptance test before approval | Director |
| Shared administrator identity until separated; provider access independence undocumented | Separate identities within 14 days; documented provider review | Technical Recovery Lead (risk owner); Director (acceptance of delay) |
| Two-technician single point of failure | Cross-training and vendor support | Director |


## 3.4 Comparison with disruption costs and investment reasoning


Warm costs NGN 2.1 million a year, which is 1.75 times the 8-hour no-measures estimate (NGN 1.2 million) and 62.5% of the 24-hour estimate (NGN 3.36 million). I modelled each package’s cost for one outage by applying the hourly rates until each function is restored (A6): Warm 225,000 + 170,000 + 70,000 + 42,500 + 12,500 = NGN 520,000 from the drill timings in Task 5; Cold at its best case of 24 hours, 135,000 × 24 = NGN 3,240,000; Hot at about one hour, NGN 135,000. The examination trigger is left out of all three because the workaround applies whichever package is bought.


*Table 11. One-outage comparison and break-even frequency (illustrative; costs in NGN).*


| **Comparison** | **Extra annual cost** | **Cost avoided per outage** | **Calculation** | **Break-even outages per year** |
|---|---|---|---|---|
| Warm instead of Cold | 900,000 | 2,720,000 | 3,240,000 − 520,000 | 0.33 (about one in three years) |
| Hot instead of Warm | 900,000 | 385,000 | 520,000 − 135,000 | 2.34 outages a year |


These numbers show a sensitivity, not a return on investment. A single-outage estimate has no probability, so it cannot give the annual expected saving, which needs the frequency and duration of floods and power failures and the effectiveness of each option. It also leaves out harm that the rates do not price. The table does say what must be believed: Warm beats Cold if a comparable outage happens more often than about once in three years, whereas Hot beats Warm only if more than two happen every year, which I do not expect.


**Evidence needed before approving the spend or accepting supplier claims:** the site’s flood and outage history; written service levels with penalties and references; a timed proof-of-concept restore of DB01 and LMS01; an on-site test of 25 working workstations; backup-provider access independence; written prices within the limit; and the exam board’s acceptance of the temporary tool.

---

[⬅ Back to README](../README.md) | [⬅ Previous: Task 2 – Recovery objectives and backup design (15 marks)](02-task2-objectives-and-backup-design.md) | [Next: Task 4 – Business continuity and disaster recovery plan (20 marks) ➡](04-task4-bcdr-plan-and-runbook.md)

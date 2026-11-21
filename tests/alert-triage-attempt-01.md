# Test: L1 alert triage — attempt 01

Date: 2026-11-21 · Score: **3/5** · Repeat scheduled with a fresh queue.
(Original answers written in RU, paraphrased here.)

Task: classify 5 alerts (≤3 min each): verdict + one-line reasoning + one action.

| # | Alert | My verdict | Correct verdict | Grade | Lesson |
|---|---|---|---|---|---|
| 1 | sshd auth failed ×3/60s, L5 | Escalate to L2 | **Watchlist** — below brute-force threshold (40112); recheck counter in 1h | 6/10 | Actions were right; label overloaded |
| 2 | SQLi in URL `' OR 1=1--`, L12 | True positive + escalate | True positive + escalate | 10/10 | Payload recognized, route correct |
| 3 | Suricata ET SCAN, L3 | True positive + escalate | TP, but L1 route = **register + watch**; escalate only on growth/critical target | 5/10 | Over-escalation |
| 4 | preauth close, no failures, L3 | False positive (justification: "employee finished work") | False positive / noise — correct verdict, **but the reasoning was invented** (src is external gateway = internet background scan; no "employee" in the alert) | 7/10 | Reason only from what is visible + known patterns |
| 5 | crontab execution at night, L10 | True positive, "recon", P1 | TP / NEEDS-L2 — technique is **persistence (T1053.003), not recon**; P2; first ask the host owner | 5/10 | Technique mislabeled; severity overcooked |

## Systemic mistakes

1. **Hyper-escalation** — pulling L2 into what L1 must digest alone (watch / register / verify). Calibration is the core queue skill.
2. **Technique confusion** — recon vs persistence vs impact used interchangeably. Correct technique name → correct severity follows.

*Mistakes kept visible on purpose. Attempt 02 — fresh queue, next session.*

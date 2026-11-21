# Test: L1 alert triage — attempt 01

> Graded honestly. Score: **3/5**. Two systemic mistakes found and documented.
> Repeat scheduled with a fresh queue.

Date: 2026-11-21 · Level: Junior / L1 · Task: classify 5 alerts (≤3 min each): verdict + one-line reasoning + one action.

---

## Alert 1 — sshd auth failed ×3 in 60s (rule 5760, L5), src 192.168.100.20

**My answer (verbatim):**
> Verdict: L2 Escalate. Reasoning: without context could be an employee typo, but need context and checks for similar logs on other hosts. Action: check other hosts for similar log entries, check IP authorization and its log history.

**Correct answer:** NEEDS-L2 / watchlist. 3 failures is below the brute-force threshold (correlation 40112 fires on a burst) — the correct L1 route is **watch** (recheck the counter in an hour), not escalation. My *actions* were textbook, the *label* was overloaded.

**Grade: 6/10** — right instinct, wrong calibration.

## Alert 2 — SQLi pattern in URL (rule 100100, L12), `q=' OR 1=1--`, HTTP 500

**My answer:**
> True Positive. SQL injection, indicated by OR 1=1-- in the search string. Action: escalate to L2 to verify and restrict the attack source.

**Correct answer:** True Positive + escalation. Matches exactly.

**Grade: 10/10** — payload recognized, route correct.

## Alert 3 — Suricata ET SCAN (rule 86601, L3), multi-port probes in 2s

**My answer:**
> True Positive. Could be a legit script/monitoring or reconnaissance, so I don't close it myself — I pass to L2 to verify the role of 192.168.100.20. Action: escalate to L2.

**Correct answer:** TP (recon), but the L1 route is **incident register + watchlist for repeats** — escalation only if the series grows or the target is critical. L3 scan is routine; L2 drowns if every scan becomes a ticket.

**Grade: 5/10** — over-escalation again.

## Alert 4 — sshd connection closed, preauth, no failures (rule 5715, L3), src 192.168.100.1

**My answer:**
> False Positive. SSH session ended, an employee finished work, no other alerts from this IP found. Action: —

**Correct answer:** FP / noise — correct verdict. But the reasoning invented a story ("employee finished work") that is **not in the alert**: src is an external gateway, preauth close without login = background internet scanner. Reasoning must contain only what is visible + known patterns, no fiction.

**Grade: 7/10** — verdict right, justification sloppy.

## Alert 5 — crontab execution at night (rule 100200, L10), user u3, cron child spawned

**My answer:**
> True Positive. Crontab logs outside working hours and auditd usage — someone is doing reconnaissance on our host. Action: P1, escalate to L2.

**Correct answer:** TP / NEEDS-L2 — but the technique is **persistence (T1053.003), not reconnaissance**; severity P2, and the step before escalation is "ask the host owner" (may be their own automation). Technique name drives the severity — recon ≠ persistence ≠ impact.

**Grade: 5/10** — direction right, technique mislabeled, severity overcooked.

---

## Systemic mistakes (the real result of this test)

1. **Hyper-escalation** — pulling L2 into what L1 must digest alone (watch / register / verify). Calibration is THE queue skill.
2. **Technique confusion** — recon vs persistence vs impact named interchangeably. Correct technique name → correct severity follows.

*Mistakes kept visible on purpose. Attempt 02 with a fresh queue — tomorrow.*

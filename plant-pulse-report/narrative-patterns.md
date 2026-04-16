# Narrative Patterns

Patterns the Plant Pulse skill uses to synthesize executive commentary and the predictive "Forward Signal" line from raw KPI data. Each pattern has a **data signature** (mechanical check the skill can evaluate) and an **interpretation** (plain-English narrative) grounded in reliability-engineering literature.

When multiple patterns fire, the skill picks the one with the strongest severity and cites the others briefly in the concern-area deep-dives.

---

## NP-01 — Reactive Creep Leading Indicator
- **Signature:** Schedule Compliance < 80% AND Reactive % > 35% within 2–3 consecutive periods.
- **Narrative:** "The planning system is losing control. Break-ins are displacing planned work; expect PM Compliance to fall and MTBF on bad actors to degrade within 1–2 quarters if the pattern continues."
- **Basis:** Palmer ch. 2 (cascade documentation); Moore, "reactive trap" feedback loop.

## NP-02 — PM Compliance → Emergency Work Lag
- **Signature:** PM Compliance drops > 10 points in period N; Emergency Work % rises in periods N+2 to N+4.
- **Narrative:** "Today's emergency spike is explained by PM deferral 1–2 quarters ago. The P–F interval means failures develop over time, not instantly — the remedy is restoring PM compliance now, even though the emergencies won't stop for another quarter."
- **Basis:** Moubray, *RCM II* ch. 7 (P–F interval theory); Nowlan & Heap (1978) failure-pattern curves.

## NP-03 — Flat Backlog Hiding Aged Work
- **Signature:** Total backlog (crew-weeks) is stable but average WO age is rising.
- **Narrative:** "Headline backlog is steady, but a pool of undoable 'waiting' work is silently aging. The real story is in the > 90-day bucket, not the total."
- **Basis:** Palmer ch. 3 warns explicitly about this reporting failure mode.

## NP-04 — OEE Decomposition Tell
- **Signature:** OEE is flat but the Availability × Performance × Quality split shifts (e.g., A rises, P falls).
- **Narrative:** "A flat OEE composite hides a regime change. Rising availability with falling performance often indicates operators or maintenance are running equipment slower to avoid failures — hidden derating. Always report A, P, Q separately."
- **Basis:** Hansen, *Overall Equipment Effectiveness* (Industrial Press, 2001), ch. 3–4.

## NP-05 — Planner Ratio Signal in Wrench Time
- **Signature:** Wrench time < 30% with adequate backlog and stable staffing.
- **Narrative:** "This is a structural planning problem, not a labor problem. Adding technicians without adding planning capacity will not move wrench time — Palmer shows it typically drops further."
- **Basis:** Palmer ch. 1, 6.

## NP-06 — MTBF Improving, MTTR Flat
- **Signature:** MTBF rising; MTTR flat or rising over same window.
- **Narrative:** "The reliability program is working (fewer failures), but failure response is not. RCA is finding causes while repair standards, kitting, and procedures lag. Availability gains will plateau until repair practice catches up."
- **Basis:** ISO 14224 reliability-maintainability framework; SMRP 5.5.1/5.5.3 paired interpretation.

## NP-07 — Stockout-Driven Schedule Miss
- **Signature:** Schedule Compliance drops in a period where stockouts or "waiting for parts" codes rise.
- **Narrative:** "Schedule failure is an MRO problem, not a maintenance-execution problem. Intervention belongs in inventory policy, not crew management."
- **Basis:** Wireman, *MRO Inventory and Purchasing* ch. 4.

## NP-08 — Safety Leading Indicator via Reactive Work
- **Signature:** Sustained Reactive % > 40% with rising overtime hours.
- **Narrative:** "TRIR is a lagging indicator. Reactive work with fatigue is a well-documented precursor — expect an elevated incident rate within 2–4 quarters if the pattern persists."
- **Basis:** HSE (UK) *Reducing Error and Influencing Behaviour* (HSG48, 2nd ed., 1999); Gulati ch. 10.

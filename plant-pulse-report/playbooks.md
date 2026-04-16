# Playbook Library

Ten named plays the Plant Pulse skill can recommend in the "Top 3 Actions" and "Concern Areas" sections. Each play is tied to a specific trigger pattern and cites a primary source.

When the skill detects a triggering pattern (from narrative-patterns.md or a direct KPI threshold), it picks the matching playbook(s), ranks by severity, and outputs the top three in priority order.

---

## PB-PM-001 — Restore PM Compliance
- **Trigger:** PM Compliance < 80% for 2+ periods, OR declining trend with Reactive % rising.
- **Steps:**
  1. Segment missed PMs by asset criticality (RCM tier) and root cause (parts, labor, access, production hold).
  2. Confirm the PM compliance window definition — many "failures" are measurement artifacts.
  3. Kill-or-keep review: eliminate PM tasks with no documented failure-mode justification (Nowlan & Heap found ~11% of PMs are value-adding without RCM review).
  4. Re-baseline PM labor demand vs. available planned hours; escalate if structurally understaffed.
  5. Install a weekly PM-compliance huddle with planner + supervisor.
- **Expected outcome:** PM Compliance rises 5–10 points within 2 months; Reactive % begins to fall 1–2 quarters later (P–F lag).
- **Sources:** Moubray, *RCM II* (Industrial Press, 1997); Palmer ch. 9.

## PB-SCH-001 — Close the Schedule Compliance Gap
- **Trigger:** Schedule Compliance < 80% while backlog is adequate (> 2 crew-weeks).
- **Steps:**
  1. Audit weekly schedule for "jobs not ready" (missing parts, permits, lockout plans) — Palmer's #1 root cause.
  2. Enforce planner-packaged work only; reject break-ins without supervisor + ops sign-off.
  3. Measure break-in rate separately; target < 10% of weekly hours.
  4. Publish schedule attainment by crew weekly.
- **Expected outcome:** Schedule Compliance rises toward 85%+ within 6–8 weeks; wrench time improves as a downstream effect.
- **Sources:** Palmer ch. 3–4; SMRP Body of Knowledge, Pillar 3.

## PB-RX-001 — Break the Reactive Spiral
- **Trigger:** Reactive Work % > 40%, OR rising 5+ points across 2 periods.
- **Steps:**
  1. Freeze non-critical improvement projects; redirect to stabilization.
  2. Pareto the last 90 days of emergency work orders; identify the top 5 bad actors.
  3. Run a rapid RCM / FMEA on bad actors; install interim mitigations (PdM route, operator care task).
  4. Protect a minimum weekly planned-work window (Palmer's 80% rule).
- **Expected outcome:** Reactive % falls 5–10 points in 2–3 months; MTBF on bad actors doubles within 2 quarters.
- **Sources:** Moore, *Making Common Sense Common Practice* ch. 3–4; SMRP 5.4.1.

## PB-BL-001 — Control Backlog Growth
- **Trigger:** Backlog > 6 crew-weeks OR aged items (> 6 months) exceed 20% of backlog.
- **Steps:**
  1. Backlog scrub: cancel duplicates, obsolete-equipment WOs, and non-value work older than 12 months.
  2. Classify remaining backlog by priority and readiness (ready-to-schedule vs. waiting).
  3. Forecast burndown at current crew capacity; if > 8 weeks, justify overtime or contractor burst.
  4. Gate new non-emergency WOs until backlog is within 4–6 crew-weeks.
- **Expected outcome:** Backlog normalizes to 4–6 crew-weeks in 1–2 quarters; ready-to-schedule ratio improves.
- **Sources:** Palmer ch. 3; SMRP 5.4.8 series.

## PB-RF-001 — Eliminate Repeat Failures
- **Trigger:** Single asset with ≥ 3 failures in 90 days, OR MTBF declining on an asset class.
- **Steps:**
  1. Trigger formal RCA (5-Why, Apollo, or TapRooT) — no "replaced part" closure allowed.
  2. Validate the failure mode against an FMEA; update if missing.
  3. Check for systemic causes: lubrication, alignment, installation, operating envelope.
  4. Implement a corrective action with an owner and due date; verify effectiveness at 90 days.
- **Expected outcome:** Repeat-failure rate on the asset drops to zero for 2+ quarters; MTBF trend reverses.
- **Sources:** Latino et al., *Root Cause Analysis* (4th ed., CRC Press, 2011); SAE JA1012:2011.

## PB-MRO-001 — Fix Chronic MRO Stockouts
- **Trigger:** Spare-parts stockouts causing > 5% of schedule-compliance misses, OR critical-spare fill rate < 95%.
- **Steps:**
  1. Classify MRO by criticality × usage (ABC-XYZ analysis).
  2. For critical/high-usage (AX): set min/max with safety stock based on lead-time demand distribution.
  3. For critical/low-usage (AZ): hold insurance spares even at 0 turns; document justification.
  4. Install vendor-managed inventory or kanban for C-class consumables.
- **Expected outcome:** Critical fill rate > 97%; stockout-driven schedule misses drop by half within 1–2 quarters.
- **Sources:** Wireman, *MRO Inventory and Purchasing* (Industrial Press, 2008); Gulati, *Maintenance and Reliability Best Practices* (2nd ed.).

## PB-PLN-001 — Right-size the Planner-to-Tech Ratio
- **Trigger:** Ratio outside the 1:15 to 1:30 range, OR persistent "jobs not ready" gap.
- **Steps:**
  1. Measure planner time allocation; true planning should exceed 70% of a planner's day (Palmer: often < 30%).
  2. Remove clerical and scheduling work from planners; separate planner and scheduler roles if volume supports it.
  3. If ratio is too lean (e.g., 1:40), build a business case using wrench-time uplift math.
  4. Train planners formally — SMRP CMRP or equivalent, not on-the-job drift.
- **Expected outcome:** Schedule compliance and wrench time both improve within 2 quarters.
- **Sources:** Palmer ch. 2 & 6; SMRP planner-ratio guidance.

## PB-LUB-001 — Strengthen the Lubrication Program
- **Trigger:** Bearing- or seal-related failures > 25% of rotating-equipment failures, OR no documented lube-program audit in 12 months.
- **Steps:**
  1. Audit against ICML MLT-I or Noria lubrication-excellence criteria.
  2. Standardize lubricant identification (color-coding, tagging) to prevent cross-contamination.
  3. Implement oil analysis on critical rotating assets; set alarm limits.
  4. Train and certify lube technicians (ICML Machinery Lubrication Technician).
- **Expected outcome:** Rotating-equipment MTBF improves 20–40% within 12–18 months.
- **Sources:** Moore ch. 6; ICML standards (icmlonline.com).

## PB-PDM-001 — Deploy Condition-Based Maintenance on Bad Actors
- **Trigger:** Time-based PMs on critical rotating equipment with no condition monitoring, or repeat failures between PM intervals.
- **Steps:**
  1. Identify the P–F interval for dominant failure modes (Moubray ch. 7).
  2. Select the right PdM technology (vibration, thermography, ultrasonic, oil analysis).
  3. Set inspection interval at ½ the P–F interval.
  4. Integrate PdM alerts into CMMS work-order triggers.
- **Expected outcome:** Catastrophic failures on monitored assets drop > 80%; PM labor shifts from calendar- to condition-based.
- **Sources:** Moubray ch. 6–7; SAE JA1011:2009.

## PB-WRENCH-001 — Increase Wrench Time
- **Trigger:** Measured wrench time < 35% via formal work sampling.
- **Steps:**
  1. Run a formal work-sampling study (Palmer ch. 5) — self-reported data is unreliable.
  2. Attack the biggest bucket: typically parts waiting, travel, or instructions.
  3. Install job kitting and staging for planned work.
  4. Review shop layout and tool cribs for travel-time reduction.
- **Expected outcome:** Wrench time rises 10–15 points within 2–3 quarters.
- **Sources:** Palmer ch. 5; SMRP Body of Knowledge, Pillar 3.

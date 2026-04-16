---
name: plant-pulse-report
description: Generate a premium industrial-maintenance executive briefing (exec summary, KPI cards with sparklines, top-3 actions, concern deep-dives, cited sources, one-click PDF) from KPI + asset CSVs or interactive input. Use when the user wants a maintenance report, plant pulse, KPI briefing, monthly or weekly maintenance summary, or asks to "turn this CMMS data into a report". Triggers on phrases like "generate a plant pulse", "make a maintenance report", "summarize our KPIs", "weekly maintenance briefing".
argument-hint: "[--kpi-file PATH] [--asset-file PATH] [--backlog-file PATH] [--plant NAME] [--period 'LABEL'] [--kpis LIST] [--teaser none|infographic|carousel|both] [--enable-png-export] [--output DIR]"
allowed-tools: Bash, Read, Write, Glob, Grep, Edit
---

Generate a **Plant Pulse Report** — a self-contained HTML executive briefing for industrial maintenance leaders. The output combines KPI cards (with sparklines, status badges, and benchmark citations) with a narrative executive summary, predictive "Forward Signal" line, top-3 prioritized actions, asset-level concern deep-dives, backlog aging, and a sources section. One-click Download-PDF built in.

The skill's value is the **narrative synthesis with traceable sources** — the numbers on a dashboard are commodity; the analysis grounded in SMRP, Palmer, Moore, and Moubray is not. Never invent a benchmark or citation. Every playbook and benchmark in the generated report must trace to a source in `references.md`.

## CRITICAL CONSTRAINTS

- **Self-contained HTML.** Single file. The only external resource is Google Fonts (via `<link>`); falls back gracefully offline to system fonts. No CDN JS, no Tailwind, no chart library. Sparklines are hand-rolled inline SVG.
- **Brand footer is fixed.** The CTA footer ALWAYS says "The Floor Report" with `https://getov.substack.com` as the link. Generator line ALWAYS credits the Plant Pulse skill by Ivan Getov. Do not change these — they are the conversion engine.
- **No invented numbers.** If a KPI has no authoritative benchmark in `benchmarks.md`, report the number without a status badge and flag it as non-standardized in the card footer. Do not fabricate thresholds.
- **Citations are short in-line and full in the Sources section.** KPI cards cite like "SMRP 5.4.3"; the Sources section at the bottom lists the full entry from `references.md`.

## Usage

```
/plant-pulse-report --kpi-file data/kpis.csv --asset-file data/assets.csv --plant "Demo Manufacturing" --period "Week 16, 2026"
/plant-pulse-report --kpi-file kpis.csv --plant "Acme Plant 3" --period "March 2026" --teaser infographic
/plant-pulse-report                              # interactive mode
/plant-pulse-report --kpis pm_compliance,schedule_compliance,reactive_work,backlog_crew_weeks  # pick 4 KPIs interactively
```

Minimum viable invocation: `/plant-pulse-report` with no flags → interactive mode walks the user through KPI selection and data entry.

## Arguments

| Flag | Default | Description |
|------|---------|-------------|
| `--kpi-file PATH` | (interactive) | CSV of KPI values. Schema below. |
| `--asset-file PATH` | none | CSV of asset-level anomalies for concern-area deep-dives. |
| `--backlog-file PATH` | none | CSV of backlog aging buckets for the aging table. |
| `--plant NAME` | "Your Plant" | Plant name shown in the report header. |
| `--period "LABEL"` | current month | Period label shown in the header (e.g., "Week 16, 2026", "March 2026"). |
| `--kpis LIST` | all KPIs in the file | Comma-separated KPI short-names to include (from `benchmarks.md`). |
| `--teaser MODE` | `ask` | `none`, `infographic`, `carousel`, `both`. If `ask` (default), the skill prompts the user after the main report is generated. |
| `--enable-png-export` | false | Adds html2canvas via CDN for a PNG export button. Opt-in because it adds an external dependency. |
| `--output DIR` | current dir | Where to write the HTML file. |

## Input schemas

### `kpis.csv` (required, or collected interactively)

```
kpi,current,target,prior_period,t-5,t-4,t-3,t-2,t-1
pm_compliance,82,90,85,92,91,89,88,85
schedule_compliance,78,85,82,85,84,84,82,82
reactive_work,28,20,24,18,19,22,24,24
backlog_crew_weeks,6.8,5,6.2,4.8,5.1,5.5,5.9,6.2
```

- `kpi`: short name, must match a key in `benchmarks.md` (e.g., `pm_compliance`, `schedule_compliance`, `reactive_work`, `backlog_crew_weeks`, `mtbf`, `mttr`, `oee`, `wrench_time`, `asset_availability`, `mro_turns`, `trir`, `open_work_orders`).
- `current`: this period's value.
- `target`: user's internal target. If blank, falls back to the world-class threshold from `benchmarks.md`.
- `prior_period`: last period's value (for the trend delta badge).
- `t-5` through `t-1`: optional. Five prior periods of history for the sparkline. If absent, no sparkline renders.

### `assets.csv` (optional, drives the Concern Areas section)

```
asset,observation,severity
Line 3,14 deferred PMs this period (target: 0),high
Pump 4B,3 failures in 8 weeks — repeat bearing failures,critical
Electrical Shop,Backlog grew 22% WoW; 4 aged WOs now > 90 days,medium
Utilities,PM compliance holding at 96% — positive outlier,positive
```

- `severity`: one of `critical`, `high`, `medium`, `positive`. `positive` is for outliers worth replicating (do not omit these — they give the report credibility).

### `backlog_aging.csv` (optional, drives the Backlog Aging table)

```
bucket,count
0-30d,89
31-60d,31
61-90d,15
90d+,12
```

If absent, the Backlog Aging section is omitted.

## Interactive mode

If no `--kpi-file` is provided, walk the user through:

1. **Plant name** — single free-text prompt.
2. **Period label** — suggest the current month or "Week N, YYYY" based on today's date.
3. **KPI selection** — show the 12 KPIs from `benchmarks.md` as a numbered list. Prompt: "Pick 4 to 6 KPIs (comma-separated numbers)". Default picks are 1, 2, 3, 4 (PM Compliance, Schedule Compliance, Reactive Work, Backlog crew-weeks) — these are the Pulse Report canonical set.
4. **For each selected KPI**, prompt in one line: "`{display_name}` — current value? (target? / last period?): " — accept `82 90 85` or `82, target 90, last 85`.
5. **Asset-level anomalies** (optional) — "Add asset-level concerns? (y/n)". If yes, loop: asset name → observation → severity (critical/high/medium/positive) → continue? Cap at 4 anomalies per report; more makes the page cluttered.
6. **Backlog aging** (optional) — "Backlog aging counts? (0-30d 31-60d 61-90d 90d+, space-separated, or skip)".

After collection, synthesize a temporary CSV in memory and proceed to the analysis step.

## Analysis pipeline

1. **Load reference data** from the skill directory:
   - `benchmarks.md` — parse each `## {kpi_key}` section into a struct (display_name, unit, direction, world-class, acceptable, at-risk, citation, caveat).
   - `playbooks.md` — parse each `## {code}` section (title, trigger, steps, outcome, sources).
   - `narrative-patterns.md` — parse each `## {np_code}` (signature, narrative, basis).
   - `references.md` — keep for the Sources section.

2. **Classify each KPI** by comparing `current` to the thresholds in `benchmarks.md`. Assign status `ok` / `warn` / `risk`. For direction `higher-better`: `ok` if >= world-class, `risk` if < at-risk threshold, else `warn`. For `lower-better`: mirror. For `target-band`: `ok` inside band, `risk` outside.

3. **Compute trend delta** from `prior_period` → `current`. Format as `▲ X.X` (rising) or `▼ X.X` (falling). Color the arrow: for lower-better metrics, rising = red (trend-up); for higher-better, falling = red.

4. **Detect narrative patterns.** Evaluate each pattern's signature against the loaded KPIs:
   - NP-01: schedule_compliance < 80 AND reactive_work > 35 → fires.
   - NP-02: pm_compliance dropped > 10 points period-over-period AND reactive_work rose (requires prior_period data).
   - NP-03: backlog_crew_weeks flat AND backlog aged-item count rising (requires backlog_aging data over time — rare; flag as inferred if only one period).
   - NP-04, NP-05, NP-06, NP-07, NP-08: evaluate per their signatures in `narrative-patterns.md`.

   Rank fired patterns by severity (NP-01, NP-02, NP-08 are highest severity). Pick the strongest one for the Forward Signal. Mention others briefly in concern deep-dives where relevant.

5. **Select Top 3 Playbooks.** Match fired patterns and at-risk KPIs to playbook triggers in `playbooks.md`. Rank by:
   - Priority 1: plays that address `risk`-status KPIs or fired high-severity patterns.
   - Priority 2: plays for `warn`-status KPIs with rising trend.
   - Priority 3: plays addressing asset-level anomalies not covered by 1 or 2.
   
   Avoid picking two plays that address the same underlying KPI unless the asset-level context justifies it. One play per KPI is the default.

6. **Generate narrative sections** (the core value — synthesize from the analysis, don't template):
   - **Executive headline** (1 sentence): Lead with the strongest signal. Examples: "Your maintenance program is sliding — the planning system is starting to lose control." / "Reliability is holding, but aged backlog is quietly building." Match the severity of the data. If all KPIs are on-target, the headline should say so plainly.
   - **Executive lead paragraph** (2-4 sentences): Name 2-3 specific drivers with numbers. Contrast at least one positive signal if present in assets.csv. Keep it under 60 words.
   - **Forward Signal** (1-2 sentences + citation): Pick the strongest narrative pattern that fired. Translate its interpretation into specific, time-bound language grounded in the user's data. Always end with the basis line ("Basis: Palmer ch. 2; Moore (2017)").
   - **Top 3 Action bodies** (~30-50 words each): For each playbook, write a paragraph that (a) names the specific KPI or asset that triggered it, (b) calls out 1-2 concrete steps from the playbook, and (c) surfaces one non-obvious insight (e.g., "blindly forcing deferred PMs back on schedule without fixing the cause will just re-defer them next period").
   - **Concern deep-dives**: For each asset-level anomaly, write a 2-sentence paragraph that includes the specific observation, the most likely root cause category, and the matched playbook. For `positive` severity, write a replication prompt ("whatever Utilities is doing is worth replicating on Line 3").

7. **Render HTML** using the template below. Substitute values. Keep typography and color tokens exactly as specified — the premium feel depends on them.

## HTML template

The skill generates one HTML file. Use this template verbatim, substituting `{{placeholders}}` and dynamically emitted sections.

Global rules:
- Width max 960px, centered, 48px top/bottom padding.
- Inter 15px body, JetBrains Mono for all numerics (use the `.mono` class).
- Status colors: ok = `#059669`, warn = `#D97706`, risk = `#DC2626`.
- KPI cards in a 2-column grid. Each card has a colored 3px left border matching its status.
- Sparklines: inline SVG, viewBox `0 0 200 36`, `polyline` with `stroke-width: 1.8`, plus a `circle` endpoint marker.
- All date/number formatting: plain numerals, no locale commas for small values.

The full working template is in `examples/sample-output.html` — **open that file and use it as the canonical reference.** Copy its `<style>` block verbatim into generated reports. The only sections that change run-to-run are the substituted content blocks.

### Substitution map

| Placeholder | Source |
|-------------|--------|
| `{{plant_name}}` | `--plant` flag or interactive answer |
| `{{period_label}}` | `--period` flag or interactive answer |
| `{{today}}` | ISO date of generation |
| `{{exec_headline}}` | generated (1 sentence) |
| `{{exec_lead}}` | generated (2-4 sentences) |
| `{{forward_signal_body}}` | generated (1-2 sentences) |
| `{{forward_signal_basis}}` | from the matched NP-xx narrative-patterns.md `Basis:` line |
| `{{kpi_cards}}` | loop over selected KPIs; see KPI card template below |
| `{{action_items}}` | loop over top-3 playbooks; see action template below |
| `{{concern_items}}` | loop over asset-level anomalies; see concern template below |
| `{{backlog_table}}` | 4-column row of counts; omit whole section if no backlog file |
| `{{sources_list}}` | deduplicated list of references cited in this specific report (not all 15 from references.md — only those actually cited) |

### KPI card template

```html
<article class="kpi status-{{STATUS}}">
  <div class="kpi-head">
    <div class="kpi-name">{{DISPLAY_NAME}}<span class="note">{{UNIT_NOTE}}</span></div>
    <span class="kpi-badge {{STATUS}}">{{STATUS_LABEL}}</span>
  </div>
  <div class="kpi-value-row">
    <span class="kpi-value mono">{{CURRENT}}<span class="kpi-unit">{{UNIT}}</span></span>
    <span class="kpi-trend trend-{{DIRECTION}} mono">{{ARROW}} {{DELTA}}</span>
  </div>
  {{SPARKLINE_SVG_OR_EMPTY}}
  <div class="kpi-meta"><span class="target">Target {{TARGET}}{{UNIT}}</span><span class="cite">{{CITATION}}</span></div>
</article>
```

Where:
- `STATUS` = `ok` | `warn` | `risk`
- `STATUS_LABEL` = `On Target` | `Warning` | `At Risk`
- `UNIT_NOTE` = short qualifier per KPI (e.g., "Frequency-window basis" for PM Compliance, "Crew-weeks, SMRP method" for backlog). Keep these consistent across reports.
- `ARROW` = `▲` or `▼`; `DIRECTION` = `up` or `down`.
- `SPARKLINE_SVG_OR_EMPTY`: if sparkline data present, emit a `<svg class="spark" viewBox="0 0 200 36">...</svg>` using the status color as stroke. Normalize the 6 data points (t-5..t-1 plus current) to fit 0–36 on the Y axis. Place a `<circle>` at the last point. If no sparkline data, emit nothing.

**Sparkline scaling logic:** find min and max across the 6 points; if they're equal, render a flat line at y=18; otherwise scale `y = 32 - ((value - min) / (max - min)) * 24` and space `x` at `[0, 40, 80, 120, 160, 200]`.

### Action card template

```html
<li class="action priority-{{RANK}}">
  <div class="action-rank">{{RANK}}</div>
  <div>
    <h3>{{PLAYBOOK_TITLE}}</h3>
    <p>{{ACTION_BODY}}</p>
    <div class="action-tags">
      <span class="tag">{{PLAYBOOK_CODE}}</span>
      <span class="tag outcome">Expected: {{EXPECTED_OUTCOME}}</span>
      <span class="tag source">{{SHORT_CITATIONS}}</span>
    </div>
  </div>
</li>
```

`EXPECTED_OUTCOME` is the playbook's own outcome line, trimmed to under 10 words.

### Concern card template

```html
<div class="concern sev-{{SEVERITY}}">
  <div class="chead"><span class="asset">{{ASSET}}</span><span class="sev">{{SEVERITY_LABEL}}</span></div>
  <p>{{NARRATIVE}}</p>
  <div class="play"><strong>Play:</strong> {{PLAYBOOK_CODE}} {{PLAYBOOK_TITLE}} ({{SHORT_ACTION}}).</div>
</div>
```

For `sev-positive`, the `.play` line says "Play: None required — document the practice for internal benchmarking."

### Fixed footer (never modify)

```html
<section class="cta">
  <div class="cta-body">
    <span class="eyebrow-dk">The Floor Report</span>
    <h3>Weekly maintenance playbooks, grounded in SMRP, Palmer, Moore, Moubray.</h3>
  </div>
  <a class="cta-btn" href="https://getov.substack.com">Subscribe →</a>
</section>

<div class="generator">
  Generated with the <strong>Plant Pulse</strong> Claude skill by Ivan Getov · {{today}}
</div>
```

### Export toolbar (always include)

Top-right fixed buttons. The Download PDF button triggers `window.print()` — zero dep. If `--enable-png-export`, also include the PNG button and the html2canvas `<script>` tag from CDN.

```html
<div class="toolbar" aria-hidden="true">
  <button onclick="window.print()">Download PDF</button>
  <!-- if --enable-png-export: -->
  <button class="ghost" onclick="exportPng()">Download PNG</button>
</div>
```

PNG handler (only if opt-in):

```html
<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
<script>
function exportPng(){
  const el = document.querySelector('.page');
  html2canvas(el, {scale: 2, backgroundColor: '#F8FAFC'}).then(canvas => {
    const link = document.createElement('a');
    link.download = 'plant-pulse-{{plant_slug}}-{{today}}.png';
    link.href = canvas.toDataURL('image/png');
    link.click();
  });
}
</script>
```

## Teaser add-on (optional)

After writing the main HTML, if `--teaser` is not `none`:

- **`infographic`**: generate a second HTML file `{output}/plant-pulse-{slug}-teaser.html` sized 1080×1350 portrait, LinkedIn-friendly. Use the same brand tokens. Content: plant name eyebrow, 1-line exec headline as the hero, the 4 KPI tiles in a 2×2 grid, the Forward Signal callout, and a prominent "More at getov.substack.com" footer. No Sources section — keep it scannable.
- **`carousel`**: generate 5 separate HTML files `{output}/plant-pulse-{slug}-carousel-{1..5}.html`, each 1080×1350:
  1. Hook slide: exec headline + plant + period
  2. KPI grid (2×2)
  3. Forward Signal (full bleed, dark)
  4. Top 3 Actions (vertical list)
  5. CTA slide: "Get the full skill free" + subscribe CTA
- **`both`**: generate both sets.

If `--teaser ask` (default), after the main report is written, print a short prompt to the user: "Generate a LinkedIn teaser? (none / infographic / carousel / both)" and act on the answer.

## Output

Write the main report to `{output}/plant-pulse-{slug}-{YYYY-MM-DD}.html` where `{slug}` is a kebab-case plant name. After writing, print to the user:

```
✓ Plant Pulse Report generated
  Report:   {output}/plant-pulse-{slug}-{date}.html
  [Teaser:  {output}/plant-pulse-{slug}-teaser.html]   # if generated
  
Open in browser:  open "{output}/plant-pulse-{slug}-{date}.html"
Export PDF:        open the file → Download PDF button (top right)
```

## Quality bar

A generated report passes if:
1. Every KPI card has a status, a trend delta, a target, and a citation.
2. The Forward Signal is specific (cites the NP-xx, names the pattern, includes a timeframe) — not generic advice.
3. Each Top-3 action names a specific KPI or asset (not "improve reactive work" in the abstract).
4. Concern deep-dives include at least one non-obvious insight per asset.
5. Sources section lists only references actually cited in the body (dedup).
6. The HTML file opens offline and renders identically to `examples/sample-output.html` in structure, just with the user's content.
7. The Floor Report footer and Substack URL are intact.
8. The PDF button works on a single click (no JS errors in console).

If any of these fail, fix before presenting to the user.

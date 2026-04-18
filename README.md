# Floor Report Skills

Free Claude skills and field guides for industrial maintenance and reliability leaders. Grounded in SMRP, Palmer, Moore, Moubray — no AI-slop. Companion library to **[The Floor Report](https://getov.substack.com)** Substack.

---

## Skills in this repo

| Skill | What it does | Status |
|-------|--------------|--------|
| [`plant-pulse-report`](./plant-pulse-report) | Turns maintenance KPIs into a cited executive briefing (exec summary, KPI cards with sparklines, top-3 actions, one-click PDF) | v1.0.0 |

More shipping soon.

## Field guides

| Guide | What it is | Download |
|-------|-----------|----------|
| [`maintenance-playbook`](./maintenance-playbook) — **Fix the System** | A 25-page field guide to getting work under control in 90 days. Foundations → self-assessment → ten named plays → tools and cadences. | [📥 PDF](https://github.com/ivanegetov-lab/floor-report-skills/releases/download/fix-the-system-v1.0.0/Fix-the-System.pdf) |

---

## Install any skill from this repo

Every skill in this repo follows the same install pattern — drop the folder into your Claude skills directory.

**macOS / Linux:**
```bash
git clone https://github.com/ivanegetov-lab/floor-report-skills.git
cp -r floor-report-skills/plant-pulse-report ~/.claude/skills/
```

**Windows (PowerShell):**
```powershell
git clone https://github.com/ivanegetov-lab/floor-report-skills.git
Copy-Item -Recurse floor-report-skills/plant-pulse-report $env:USERPROFILE\.claude\skills\
```

**No git?** Click the green **Code** button at the top of this repo → **Download ZIP** → unzip → copy the skill folder you want into `~/.claude/skills/`.

Restart Claude Code (or reload skills in claude.ai) and invoke with `/skill-name`.

---

## What's a Claude skill?

A Claude skill is a folder with a `SKILL.md` instruction file (plus any supporting data). When installed, Claude picks it up automatically and lets you invoke it with `/skill-name`. No code changes, no CLI tools — just drop the folder and go.

Each skill in this repo has its own README with specific usage instructions.

---

## Why these exist

Most maintenance dashboards show numbers. Very few show the *analysis* — what the pattern means, what play to run, what's likely to happen next quarter if you don't act. That analysis is what separates a reliability engineer from a spreadsheet.

These skills encode the analysis. Every benchmark and playbook traces to a real source: SMRP Best Practices, Doc Palmer's *Maintenance Planning and Scheduling Handbook*, Ron Moore's *Making Common Sense Common Practice*, John Moubray's *RCM II*, ISO 14224, SAE JA1011/1012, US BLS. No invented numbers.

Free to use, share, and modify.

---

## About

Built by **Ivan Getov**. I write at [**The Floor Report**](https://getov.substack.com) — a weekly briefing for maintenance and reliability leaders.

> *I help plants and manufacturing teams perform better by fixing the system, not blaming the people.*

Want more playbooks and the next skills as they ship? Subscribe at [getov.substack.com](https://getov.substack.com).

---

## Contributing

Issues and PRs welcome. If you're adapting a skill for your plant and want to contribute the generalized version back, even better.

If you're a maintenance or reliability professional with a process you wish was a skill, open an issue and describe it — I'm actively building.

---

## License

[MIT](./LICENSE) — use, share, modify. Attribution appreciated but not required (the skills credit themselves in their generated output).

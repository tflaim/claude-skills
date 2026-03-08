# Claude Skills

Skills I use daily with Claude Code. Each one solves a specific problem I kept running into: AI-generated text that sounds like a robot, system explanations that stay surface-level, reviews that pull punches, and session learnings that evaporate between conversations.

## Installation

### Plugin marketplace (recommended)

```
/plugin marketplace add tflaim/claude-skills
```

### Manual install

Clone the whole collection:

```bash
git clone https://github.com/tflaim/claude-skills.git ~/.claude/skills/tflaim-claude-skills
```

Or grab a single skill:

```bash
# Example: install just deslop
cp -r skills/deslop ~/.claude/skills/deslop
```

## Skills

| Skill | What it does | Lines |
|-------|-------------|-------|
| [deslop](#deslop) | Strips AI writing patterns and injects human voice | 165 |
| [explain-system](#explain-system) | Builds mental models of technical systems you can reason with | 290 |
| [expert-review](#expert-review) | Summons a domain expert persona for genuinely critical feedback | 95 |
| [remember](#remember) | Routes session learnings to the right memory tier so they persist | 303 |

---

### deslop

Two jobs: remove AI-generated patterns, then add genuine voice. Stripping the robot isn't enough if what's left is sterile.

Based on Wikipedia's [WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup) research and the [humanizer](https://github.com/blader/humanizer) skill by blader (MIT). Adds register calibration (casual/professional/formal), voice injection, banned phrase tiers, and pattern 25 (negation-correction framing).

**25 patterns detected** across content, language, style, communication, filler, and hedging. Each pattern has a "words to watch" list and before/after examples in the included `patterns.md` reference.

**When to use:**
- Text reads like ChatGPT output
- Editing AI-assisted drafts before publishing
- Someone flags "this sounds like AI"

**Example trigger:** Just ask Claude to "humanize this" or "deslop this text", or invoke directly with `/deslop`.

---

### explain-system

Builds mental models of technical systems through structured exploration. The kind you can take into a meeting and reason with, not skim and forget.

Starts by asking what's driving your curiosity (a meeting tomorrow vs. deep architecture understanding). Explores the codebase or documentation, proposes a custom outline, then delivers an explanation with confidence signaling on every claim (verified / inferred / uncertain).

**Includes:**
- Adaptive section library: analogy, architecture diagram (Mermaid), integration map, data flow, failure modes, key terminology
- Depth limits (3 hops, 8-10 files max) to prevent rabbit holes
- Comprehension check at the end to catch gaps
- Exportable artifacts (Mermaid diagrams, question checklists)

**When to use:**
- Preparing for a technical discussion about an unfamiliar system
- Onboarding onto a new codebase
- Understanding how a service works before making product decisions

---

### expert-review

Steps into the role of a world-class domain expert to make your work better. The goal is excellence, and the expert holds you to that standard.

Three modes:
1. **User-specified:** "Review this as a senior platform engineer"
2. **Auto-select:** Analyzes context and picks the most impactful expert
3. **Multi-persona:** Panel of 2-3 experts when the work spans domains

Reviews hit these beats: gut reaction, what works, what doesn't (with consequences and alternatives), what's missing, and a verdict. Calibrates depth to the artifact stage (rough draft vs. near-final spec vs. production code).

**Sticky mode:** Ask the expert to stay active throughout the conversation. They'll track whether previous feedback was addressed and escalate recurring issues.

---

### remember

Captures durable learnings from sessions and routes them to the right memory tier. Three modes:

- **Explicit:** You tell it what to remember. It validates, routes, and confirms.
- **Session scan:** Scans the conversation for friction (tool failures, corrections, workflow dead ends) and proposes learnings.
- **Maintenance:** Audits existing memory for staleness, duplicates, and misplacement.

Routes to three tiers:
| Tier | Scope | Example |
|------|-------|---------|
| Global (`~/CLAUDE.md`) | Every session, every project | "Sandbox blocks DNS resolution" |
| Project (`./CLAUDE.md`) | Team-shared conventions | "CI requires pnpm format" |
| Personal knowledge (`~/.claude/projects/*/memory/`) | Your accumulated understanding | "Auth flow starts in src/auth/handler.ts" |

Pushes back on low-quality candidates. Checks all tiers for duplicates and conflicts before adding anything. Manages capacity limits (MEMORY.md hard-capped at 200 visible lines).

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT. The `deslop` skill carries its own MIT license with attribution to [blader/humanizer](https://github.com/blader/humanizer) for patterns 1-24.

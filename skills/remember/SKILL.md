---
description: Capture and maintain learnings across memory tiers with session review and critical evaluation
argument-hint: [optional: learning to remember]
---

## Task

Capture durable learnings from sessions and route them to the right memory tier. Three modes:

- **Explicit** (user provides input): Validate → route → confirm → add
- **Session scan** (no input or `/remember` alone): Scan conversation for friction → evaluate → present → confirm → add
- **Maintenance** (user says "clean up memory", "audit", "update [entry]"): Review existing entries for staleness, duplicates, and misplacement → propose changes → confirm → apply

Memory is precious. Quality over quantity.

## Memory Architecture

Three tiers, each with a distinct purpose and audience:

| Tier | Path | Who Sees It | Purpose |
|------|------|-------------|---------|
| **Global** | `~/CLAUDE.md` | Every session, every project | Personal behavioral rules, tool gotchas, output preferences |
| **Project Instructions** | `./CLAUDE.md` (in repo) | Every session in this project, all contributors | Team-shared conventions, deployment procedures, CI requirements |
| **Personal Project Knowledge** | `~/.claude/projects/<project>/memory/` | Every session in this project, only you | Accumulated understanding: architecture, file paths, debugging insights |

The key distinction: Project Instructions are *prescriptive* (how things should work), Personal Knowledge is *descriptive* (what you've learned). A deployment procedure is an instruction. Knowing which endpoint maps to which agent is knowledge.

### Routing Decision

```
What kind of learning is this?
│
├─ Behavioral rule that applies everywhere?
│  → ~/CLAUDE.md (Global)
│  "Use MM-DD-YYYY dates", "Sandbox blocks DNS", "Ask about plugin scope"
│
├─ Convention or instruction the whole team should follow?
│  → ./CLAUDE.md (Project Instructions)
│  "CI requires pnpm format", "Use DC-XXXXX branch naming",
│  deployment procedures, PR description format
│
└─ Knowledge you personally accumulated about this codebase?
   → ~/.claude/projects/<project>/memory/ (Personal Knowledge)
   "Auth flow starts in src/auth/handler.ts",
   "Auth service endpoint is /v1/auth/verify",
   architecture patterns, debugging discoveries
```

When ambiguous, ask: "Is this a team rule (project CLAUDE.md), personal knowledge (memory), or universal (global)?"

## Step 1: Read All Memory Tiers

Read ALL three tiers before evaluating candidates:

1. `~/CLAUDE.md` — structure, existing learnings
2. `./CLAUDE.md` (if it exists) — project conventions already documented
3. The project's `memory/MEMORY.md` and any topic files referenced from it

You're checking for:
- **Duplicates**: Does this learning already exist in any tier?
- **Conflicts**: Does it contradict an existing entry somewhere?
- **Merge targets**: Should this update an existing entry rather than create a new one?

## Step 2: Gather Candidates

### Explicit Mode (user provided input)

Parse input as candidate learning(s). Go directly to Step 3 — skip the session scan unless the input is vague or you notice significant unmentioned friction.

### Session Scan Mode (no input or bare `/remember`)

Review the conversation for friction signals:

| Signal Type | What to Look For |
|-------------|-----------------|
| **Tool Friction** | Sandbox bypasses, permission errors, MCP failures, retries, workarounds |
| **Knowledge Gaps** | User corrections ("no, I meant..."), wrong assumptions, definitions provided |
| **Workflow Issues** | Dead ends, backtracking, repeated attempts at same task |
| **Preference Misalignment** | Format corrections, style feedback, explicit preferences |

Look for patterns, not every bump. One-offs don't belong in memory.

### Maintenance Mode (user wants to update/audit/clean up)

Scan the relevant tier(s) for:
- **Stale entries**: Learnings about tools, APIs, or workarounds that may no longer apply
- **Cross-tier duplicates**: Same knowledge stored in multiple places
- **Misplaced entries**: Project knowledge that crept into global, team instructions in personal memory
- **Overgrown files**: MEMORY.md approaching 200 lines, topic files exceeding useful size
- **Contradictions**: Entries in one tier that conflict with another

Present findings in the same candidate format as Step 4, with recommendations to update, relocate, merge, or remove.

## Step 3: Evaluate Candidates

For each candidate, apply three checks:

| Check | Question | Red Flags |
|-------|----------|-----------|
| **Value** | Will this prevent friction in future sessions? | Too vague, too obvious, common sense, already known |
| **Pattern** | Is this a recurring situation, not a one-off? | One-time edge case, task-specific, too narrow to recur, too broad to be actionable |
| **Risk** | Does this conflict with existing entries or could it break workflows? | Direct contradiction, overly restrictive, blocks valid use cases |

### Cross-Tier Awareness

Before accepting any candidate, verify across all tiers:
- **Contradicts** something in another tier? → Flag and resolve (don't silently add a conflicting entry)
- **Duplicates** something in another tier? → Skip, or propose merging into one location
- **Misrouted?** A team convention masquerading as personal knowledge, or vice versa?

### Generalize Before Finalizing

Before accepting, ask:

1. **What's the underlying mechanism?** — not just the symptom
   - Bad: "Skill didn't reload" → Good: "Session caches configuration at startup"
2. **What else shares this mechanism?** — broader learnings are more valuable
   - Skills → also MCPs, plugins, settings (all session-cached)
3. **Capture WHY, not just WHAT**
   - Incomplete: "Use dangerouslyDisableSandbox for git"
   - Complete: "...because sandbox blocks DNS resolution"

### Push Back on Low-Quality Candidates

Don't accept everything. Push back. When the user asks to remember a specific env var name, secret, or API key (e.g., "remember that X_SECRET needs to be set"), assign ❌ Skip. These belong in `.env.example` or project settings, not Claude memory. A single missing env var is a setup checklist item, not a behavioral learning.

- "This seems task-specific — worth adding permanently?"
- "This conflicts with [existing entry] — update that instead?"
- "This is very broad — can we make it more actionable?"
- "This already exists in [tier] — skip?"
- "This is a team convention — should it go in project CLAUDE.md instead of memory?"

Assign each candidate:
- ✅ **Add** — High value, well-scoped, no conflicts
- ✅ **Update** — Existing entry needs revision (supersedes, refines, or corrects it)
- ⚠️ **Refine** — Good intent, needs adjustment
- ❌ **Skip** — Low value, duplicate, or problematic

## Step 4: Present Candidates

For each candidate, craft the **actual final text** as it would appear in the target file. This is what the user is confirming — not a summary, but the real entry.

For **Update** candidates, show both the old text being replaced and the new text.

Show ALL candidates in a structured format:

```
## Learning Candidates

### From You
| # | Your Input | Destination | Assessment | Rec |
|---|------------|-------------|------------|-----|
| 1 | [verbatim] | [tier → file > section] | [reasoning] | ✅/⚠️/❌ |

**Proposed edit for #1** → [~/CLAUDE.md > Captured Learnings]
> [Exact text as it would appear in the file]

### From Session Review
| # | Friction | Candidate | Destination | Assessment | Rec |
|---|---------|-----------|-------------|------------|-----|
| 2 | [what happened] | [brief description] | [tier → file] | [reasoning] | ✅/⚠️/❌ |

**Proposed edit for #2** → [memory/sdk-patterns.md]
> [Exact text as it would appear in the file]

**Actions**: "add 1, 2" / "refine 1 to say [text]" / "skip" / "none"
```

The user confirms the exact text they see — no surprises at write time.

If no candidates found:
```
No learnings to capture — session was smooth. Not every session needs one.
```

## Step 5: Add Confirmed Learnings

Based on user response:

**For global CLAUDE.md:**
1. Format with action verbs and date: `(MM-DD-YYYY)`
2. Place in the correct section (see placement guide below)
3. Show the exact edit, then make it

**For project CLAUDE.md:**
1. Find the right section (or create one if needed)
2. Match the file's existing formatting conventions
3. Show the exact edit, then make it

**For personal project knowledge (auto-memory):**
1. Determine the right topic file using the Topic File Strategy below
2. Update the topic file with the learning
3. If creating a new topic file, add a link in MEMORY.md
4. If updating MEMORY.md directly (quick refs, index entries), keep it concise

**For updates to existing entries:**
1. Show the old text and the proposed replacement side by side
2. Make the edit after confirmation

**For removals (maintenance mode):**
1. Show what will be removed and why
2. Remove after confirmation

**For "skip" or "none":** Acknowledge and end.

## Topic File Strategy

Auto-memory uses MEMORY.md as an always-loaded index, with topic files for detailed knowledge.

**MEMORY.md constraints:**
- Lines after 200 are **silently truncated** — content beyond line 200 is invisible in future sessions
- Use it for: topic file links, quick one-liner references, unique compact learnings
- Do NOT use it for: multi-line explanations, detailed architecture notes, lengthy entries

**Topic file conventions:**
- Name with descriptive kebab-case: `api-backend.md`, `sdk-patterns.md`
- One domain per file, organized semantically (not chronologically)
- Check existing topic files before creating new ones — append when the topic fits
- If a topic file exceeds ~300 lines, consider splitting by subtopic

**Routing within auto-memory:**
```
Quick reference or one-liner?
├─ YES → MEMORY.md (under "Quick References" or "Unique Learnings")
└─ NO (detailed, multi-line)
    ├─ Existing topic file covers this domain? → Append to it
    └─ New domain? → Create topic file + add link in MEMORY.md
```

## Step 6: Capacity Management

After adding, check capacity across all tiers:

**Global CLAUDE.md** (target: under ~150 lines):
- Consolidate related entries that could merge
- Relocate project-specific items that crept into global
- Archive stale entries (check dates — old tool workarounds may no longer apply)

**MEMORY.md** (hard limit: 200 visible lines):
- Count current lines — if approaching 180, act now
- Move detailed content into topic files, leave only index links and one-liners
- Merge related quick-reference entries

**Topic files** (soft limit: ~300 lines):
- Flag files that have grown unwieldy
- Suggest splitting by subtopic if warranted

If action is needed, propose specific changes. Don't modify without confirmation.

## Step 7: Confirm Completion

Summarize what happened:

```
Added to ~/CLAUDE.md > Captured Learnings:
- [Learning 1]

Updated ./CLAUDE.md > Code Formatting:
- [Updated existing entry with new info]

Added to project memory/sdk-patterns.md:
- [Learning 2]

Skipped:
- [Learning 3] — duplicate of existing entry in ~/CLAUDE.md
```

## Formatting Guidelines

When writing learnings:

- **Action verbs**: Prefer, Always, Never, Ask, Use, Check
- **Concise**: 1-3 lines max
- **Specific**: Actionable, not vague
- **Dated**: Include `(MM-DD-YYYY)` for behavior patterns

**Good**: `Use dangerouslyDisableSandbox: true for git network operations — sandbox blocks DNS resolution`
**Bad**: `Be careful with sandbox mode`

## Section Placement Guide

| Learning Type | Destination |
|---------------|-------------|
| Output/format preferences | Global CLAUDE.md > Output Preferences |
| Tool/MCP gotchas | Global CLAUDE.md > Captured Learnings |
| Cross-project workflow patterns | Global CLAUDE.md > Captured Learnings |
| User context/role info | Global CLAUDE.md > About Me |
| Team conventions, CI/CD, formatting | Project CLAUDE.md (appropriate section) |
| Deployment procedures, git workflows | Project CLAUDE.md |
| Architecture patterns learned | Auto-memory topic file |
| File paths, endpoints discovered | Auto-memory (MEMORY.md quick refs or topic file) |
| Debugging insights for this codebase | Auto-memory topic file |
| One-off technical facts | DON'T ADD — transient, not a pattern |

## Rules

- **Never auto-add** — always wait for user confirmation
- **Never delete** existing content without asking
- **Quality over quantity** — push back on low-value candidates
- **Check all three tiers** for duplicates and conflicts before adding
- **Route correctly** — global behavioral rules vs team instructions vs personal knowledge
- **Keep files within limits** — CLAUDE.md ~150 lines, MEMORY.md under 200 lines
- **Prefer updates over additions** — if an existing entry covers the topic, update it rather than adding a near-duplicate

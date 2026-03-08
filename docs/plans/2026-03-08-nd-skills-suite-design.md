# Neurodivergent Support Skills Suite — Design

## Overview

A modular suite of Claude Code skills that help LLM agents work effectively with neurodivergent users. Each skill is independent — install one, some, or all. No build step, no config files, just markdown.

## Target Audience

- **Primary:** Neurodivergent developers who want Claude to adapt to their working style
- **Secondary:** Developers/leads working with ND team members who want to communicate better

## Repo Structure

```
Knx-Skills/
  README.md                        # Install instructions, skill descriptions
  SKILL_IDEAS.md                   # Future skill brainstorm (existing)
  SKILL_TEMPLATE.md                # DIY template for making new skills
  skills/
    nd-core/
      SKILL.md                     # Communication patterns, pacing, structure
    nd-frustration-support/
      SKILL.md                     # Overwhelm detection, gentle check-ins
    nd-focus-support/
      SKILL.md                     # ADHD drift nudges, task bookmarking
    nd-plural-support/
      SKILL.md                     # Fronting check-ins, JSON log guidance
```

## Skill Designs

### nd-core (recommended base)

**Purpose:** Adapt communication style for neurodivergent users.

**Behaviors:**
- Default to bullet points and short paragraphs over walls of text
- Be explicit — no implied meaning, no "you probably already know this"
- One concept per message when explaining
- Consistent structure (same heading patterns, same ordering)
- Avoid ambiguous language ("maybe try..." becomes "do X because Y")
- Number options — don't make people parse prose for choices
- Summarize before diving deep

**Scope boundary:** No emotional inference, no attention tracking, no identity.

### nd-frustration-support

**Purpose:** Notice overwhelm signals and check in gently.

**Signals:**
- Repeated attempts at the same thing without progress
- Tone shifts — "forget it", "whatever", "this is stupid", sudden brevity
- Increased typos/fragmented messages vs earlier in conversation
- Sudden topic abandonment mid-task

**Response style:**
- Reframe the problem: "Want me to try explaining this a different way?"
- Offer a pivot: "We can come back to this — want to work on something else?"
- Never suggest ending the session unless user brings it up
- Casual, not clinical — no "are you okay?" energy
- Don't diagnose, don't therapize, don't assume the cause

**v2 possibilities:**
- Configurable signal sensitivity
- Per-user pattern baselines

### nd-focus-support

**Purpose:** Catch ADHD drift and nudge back without judgment.

**Behaviors:**
- Track current task/goal internally
- When conversation wanders, casual nudge: "Oh hey we were working on X, wanna loop back?"
- Treat tangents as normal, not failures
- Nudge is an offer, not a correction
- If user wants to continue the tangent, update current task and move on

**v2 possibilities:**
- Signpost style: "Current task: X. We've moved into Y. Which one?"
- Bookmark tangents for later
- Task queue for parked ideas

### nd-plural-support

**Purpose:** Warm check-in for plural systems with simple session logging.

**Behaviors:**
- Session start: "Hey, welcome back! Mind if I ask who's fronting?"
- No pressure — if skipped, move on, don't re-ask
- Log responses to JSON file

**JSON format:**
```json
{
  "sessions": [
    { "date": "2026-03-08", "fronting": "Dusk", "notes": "" },
    { "date": "2026-03-07", "fronting": "unsure", "notes": "co-con" }
  ]
}
```

- `fronting`: whatever they say, no validation, no predefined list
- `notes`: optional, only if volunteered (co-fronting, blurry, etc.)
- File location: project root or `~/.claude/` — user's choice
- Uses community language ("fronting"), no clinical terms

**v2 possibilities:**
- Per-fronter preferences (communication style, tone)
- History: "last time Dusk was here you were working on X"
- PluralKit integration (safely, within reason)

## Modularity

| Skill | Standalone? | Notes |
|-------|-------------|-------|
| nd-core | Yes | Recommended base for all others |
| nd-frustration-support | Yes | Better with nd-core |
| nd-focus-support | Yes | Better with nd-core |
| nd-plural-support | Yes | Fully independent |

Install = copy folder to `~/.claude/skills/`. Uninstall = delete folder.

## Additional Deliverables

- **SKILL_TEMPLATE.md** — fill-in-the-blank template so the user can create their own skills (e.g. for the LoRA training and Kohya skills in SKILL_IDEAS.md)
- **README.md** — repo overview, install instructions, skill table

## Design Decisions

- **Modular over monolithic:** Users install only what applies to them
- **Casual over clinical:** These are pair-programming vibes, not therapy
- **Signal-based over diagnostic:** Respond to what you see, don't label why
- **Lean v1:** Ship minimal, note v2 ideas in each skill doc
- **Agent-agnostic:** Just markdown — any LLM agent that reads files can use these

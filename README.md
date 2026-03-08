# Knx-Skills — Practical Skills for LLM Agents

Skills that make AI coding assistants more useful — neurodivergent support, LoRA training guidance, and more. Built for Claude Code, works with any LLM agent that reads markdown.

## What's Included

### Neurodivergent Support

| Skill | What It Does | Standalone? |
|-------|-------------|-------------|
| [nd-core](skills/nd-core/) | Communication style — bullets over walls of text, explicit over ambiguous, structured over freeform | Yes (recommended base) |
| [nd-frustration-support](skills/nd-frustration-support/) | Notices overwhelm signals and checks in gently — no therapy, no patronizing | Yes |
| [nd-focus-support](skills/nd-focus-support/) | Catches ADHD drift and nudges back on topic without judgment | Yes |
| [nd-plural-support](skills/nd-plural-support/) | Warm check-in for plural systems with simple session logging | Yes |

### Wellbeing

| Skill | What It Does | Standalone? |
|-------|-------------|-------------|
| [sounding-board](skills/sounding-board/) | Active listening and talk-it-out space — reflects, asks light questions, reminds you of your own coping tools, knows when to refer out | Yes |

### AI Art & Training

| Skill | What It Does | Standalone? |
|-------|-------------|-------------|
| [lora-trainer-guide](skills/lora-trainer-guide/) | Preset selection + parameter reference for LoRA training — covers SD1.5, SDXL, SD3, Flux1 with 38 real-world presets | Yes |

## Install

### Claude Code

Copy the skills you want to `~/.claude/skills/`:

```bash
# All skills
cp -r skills/* ~/.claude/skills/

# Just the ones you want
cp -r skills/nd-core ~/.claude/skills/
cp -r skills/nd-plural-support ~/.claude/skills/
```

### Other LLM Agents

Any agent that loads context from files can use these. Point it at the SKILL.md files in whatever way your agent supports.

## Making Your Own Skills

See [SKILL_TEMPLATE.md](SKILL_TEMPLATE.md) for a fill-in-the-blank template.

## Philosophy

- **Modular** — install only what you need, remove what you don't
- **Casual, not clinical** — pair-programmer vibes, not therapist energy
- **Signal-based, not diagnostic** — respond to what you see, don't label why
- **Lean** — ship minimal, iterate later

## Future Skills

See [SKILL_IDEAS.md](SKILL_IDEAS.md) for ideas in the pipeline (cloud GPU deployment, model architecture guide, and more).

## Contributing

PRs welcome. If you've got a useful pattern for AI assistants, add it.

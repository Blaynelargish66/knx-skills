# Skill Ideas for Ktiseos-Nyx / Duskfallcrew

Personal brainstorm doc. Not committed to repo.


## Skills You Could Build (Based on What You Know)

### 1. lora-training-workflow
**What it teaches:** The end-to-end LoRA training workflow — dataset prep, tagging
strategies, caption best practices, parameter selection by model type, common
training pitfalls.

**Why it's useful:** You've done this enough to have strong opinions. New trainers
stumble on the same things every time (wrong resolution, bad learning rates,
over-training, bad captions). This skill would help any Claude instance guide
someone through training.

**Target audience:** Anyone using Kohya SS, sd-scripts, or your trainer.

**Skeleton:**
```
lora-training-workflow/
  SKILL.md        # Decision tree: model type -> parameters -> common mistakes
```


### 2. kohya-config-reference
**What it teaches:** How Kohya SS TOML configs work — what each parameter does,
which ones matter, which ones are traps, sane defaults by model type.

**Why it's useful:** The Kohya docs are... sparse. You've reverse-engineered a lot
of this. A skill that any Claude instance could load when helping someone configure
training would be genuinely valuable to the community.

**Target audience:** Anyone touching sd-scripts configs.

**Skeleton:**
```
kohya-config-reference/
  SKILL.md          # Overview, when to use, quick reference table
  parameters.md     # Full parameter reference (too big for inline)
  defaults.md       # Recommended defaults by model type (SD1.5, SDXL, Flux)
```


### 3. vastai-runpod-deployment
**What it teaches:** How to deploy Python+Node apps on rented GPU boxes — the
gotchas with supervisor, port binding, Caddy proxying, npm on Linux when you
dev on Windows, package-lock.json platform mismatches.

**Why it's useful:** You've hit every single pothole. This is hard-won knowledge
that barely exists anywhere else.

**Target audience:** Anyone deploying web apps to VastAI/RunPod.

**Skeleton:**
```
vastai-runpod-deployment/
  SKILL.md    # Decision tree, common failures, platform gotchas
```


### 4. neurodivergent-dev-patterns
**What it teaches:** How to structure development work, documentation, and
communication for neurodivergent developers. Clear patterns, consistent
structure, avoiding ambiguity, breaking complex tasks into discrete steps.

**Why it's useful:** You live this. Most dev tooling and documentation assumes
neurotypical processing. A skill that helps Claude adapt its communication
and work style would be genuinely novel.

**Target audience:** Neurodivergent developers, or anyone working with them.

**Skeleton:**
```
neurodivergent-dev-patterns/
  SKILL.md    # Communication patterns, task structure, anti-patterns
```


### 5. sd-model-architecture-guide
**What it teaches:** The differences between SD 1.5, SDXL, SD3, Flux, and
other architectures from a TRAINING perspective — what changes, what doesn't,
what parameters need adjusting, which LoRA types work with which architecture.

**Why it's useful:** This is scattered across Discord threads, GitHub issues,
and tribal knowledge. Having it in one searchable skill would be huge.

**Target audience:** Anyone training or merging models across architectures.

**Skeleton:**
```
sd-model-architecture-guide/
  SKILL.md           # Architecture comparison, training implications
  architecture.md    # Detailed block structures (for MBW merging etc.)
```


## How to Structure a Shareable Skill Collection

If you want to publish these as a GitHub repo:

```
duskfallcrew-skills/
  README.md                          # What's included, install instructions
  skills/
    lora-training-workflow/
      SKILL.md
    kohya-config-reference/
      SKILL.md
      parameters.md
      defaults.md
    vastai-runpod-deployment/
      SKILL.md
    neurodivergent-dev-patterns/
      SKILL.md
    sd-model-architecture-guide/
      SKILL.md
      architecture.md
```

### README Template

```markdown
# Duskfallcrew Skills for Claude Code

Skills for AI-assisted LoRA training, model merging, and GPU deployment.

## Install

Copy the skills you want to `~/.claude/skills/`:

    # All skills
    cp -r skills/* ~/.claude/skills/

    # Just one
    cp -r skills/lora-training-workflow ~/.claude/skills/

## What's Included

| Skill | Description |
|-------|-------------|
| lora-training-workflow | End-to-end LoRA training guidance |
| kohya-config-reference | Kohya SS parameter reference |
| vastai-runpod-deployment | Cloud GPU deployment gotchas |
| neurodivergent-dev-patterns | Dev workflow for ND developers |
| sd-model-architecture-guide | SD/SDXL/Flux architecture differences |

## Contributing

PRs welcome. If you've hit a training gotcha that isn't covered, add it.
```


## Tips for Writing Good Skills

1. **Start with what you explain repeatedly** — if you've told 5 people the same
   thing on Discord, that's a skill waiting to happen.

2. **Write for "Claude helping someone who isn't you"** — your CLAUDE.md is for
   YOUR repo. Skills are for anyone's repo.

3. **Tables > paragraphs** — Claude scans tables fast. Put quick-reference info
   in tables, explanations in prose below.

4. **Include the "why"** — don't just say "set learning rate to 1e-4". Say WHY
   that's the right value and when to change it.

5. **One skill = one concern** — don't make a mega-skill. Five focused skills
   beat one 2000-line monster.

6. **Test by asking Claude without context** — start a fresh conversation, load
   the skill, and ask a question it should handle. If Claude fumbles, the skill
   needs work.


## What NOT to Skill-ify

- Project-specific config (that's CLAUDE.md)
- Things Claude already knows well (basic git, standard Python, etc.)
- Opinions without experience behind them
- Anything that changes frequently (version-specific bugs, etc.)

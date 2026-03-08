# Skill Template

Copy this template to create your own skill. Replace everything in `[brackets]`.

## Folder Structure

Create a folder under `skills/`:

```
skills/
  [your-skill-name]/
    SKILL.md
```

## SKILL.md Template

Copy everything below the line into your new `SKILL.md` file:

---

```markdown
---
name: [your-skill-name]
description: Use when [specific triggers — what situation makes this skill relevant?]
---

# [Skill Title]

## Overview

[1-2 sentences: what does this skill do? Core principle.]

## When to Use

- [Trigger 1: specific situation or symptom]
- [Trigger 2]

## [Core Section — rename to match your skill]

[The actual guidance. Use tables for quick-reference, prose for explanation.]

| [Column 1] | [Column 2] |
|-------------|-------------|
| [Item] | [Detail] |

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| [What goes wrong] | [What to do instead] |

## What This Skill Does NOT Do

- [Boundary 1 — what's out of scope]
- [Boundary 2]
```

## Tips

1. **Name:** lowercase, hyphens, verb-first if possible (`lora-training-workflow` not `workflow-for-lora`)
2. **Description:** Start with "Use when..." — this is how Claude decides to load your skill
3. **Keep it focused:** one skill = one concern. Five small skills beat one monster.
4. **Tables over paragraphs:** Claude scans tables fast
5. **Include the "why":** Don't just say what to do — say why
6. **Test it:** Start a fresh Claude session, load the skill, ask it a question it should handle. If Claude fumbles, revise.

## Frontmatter Rules

- Only two fields: `name` and `description`
- Max 1024 characters total
- `name`: letters, numbers, and hyphens only
- `description`: start with "Use when...", third person, under 500 characters

# Design: sounding-board skill

**Date:** 2026-03-08
**Status:** Approved

## Summary

An active listening and talk-it-out skill for LLM agents. Not therapy, not silence — a friend who reflects, asks light questions, reminds you of your own coping tools, and knows when to say "this one's bigger than me."

## Scope (v1)

- Single SKILL.md with conversation modes
- Works in any context (coding session, standalone conversation, whatever)
- No separate resources file (v2)

## Structure

```
skills/sounding-board/
  SKILL.md    # Everything — modes, tone, boundaries, examples
```

## Conversation Modes

The LLM reads the situation and shifts naturally between:

| Mode | When | Behavior |
|------|------|----------|
| Venting | User needs to yell/rant/get it out | Validate, mirror, don't problem-solve |
| Untangling | User is circling or confused | Light questions to help them find the thread |
| Grounding | User is escalated, knows their own tools | Remind them of their coping skills, not teach new ones |

## Boundaries

- **Crisis topics** (self-harm, suicidal ideation, abuse) → immediate warm redirect to resources (988 Lifeline, Crisis Text Line). No hesitation.
- **Depth boundary** → trauma processing, deep diagnostic territory → gentle nudge toward therapist. "I can be here for this but I'm not equipped to work through it with you."
- **Not-a-replacement** → knows it's not therapy, says so naturally when relevant, not as a canned disclaimer.

## Tone

- Friend who gets it, not counselor with clipboard
- Match their energy level
- Give them the wheel — ask what they need
- Reference THEIR coping skills when they've mentioned them
- Never: "I understand how you feel", "have you tried...", unsolicited advice, BetterHelp energy

## What It Gives Back

- Reflects back what it heard at natural pauses
- Spots recurring patterns and gently flags them as therapist-worthy
- Never ends with homework unless they ask

## v2 Candidates

- Separate resources file with curated crisis/support resources
- Configurable crisis resources by region
- Coping skill library / prompts

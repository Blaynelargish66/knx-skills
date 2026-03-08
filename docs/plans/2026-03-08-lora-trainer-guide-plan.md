# lora-trainer-guide Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a skill that helps LLMs guide users through selecting and understanding LoRA training presets.

**Architecture:** Two-file skill (SKILL.md + reference.md) with a presets/ folder of 38 JSON configs. SKILL.md is the opinionated guide, reference.md is the lookup encyclopedia. Renamed from kohya-config to lora-trainer-guide.

**Tech Stack:** Pure markdown, JSON presets (no build step)

---

### Task 1: Rename skill folder

**Step 1:** Rename `skills/kohya-config/` to `skills/lora-trainer-guide/`

**Step 2:** Verify presets/ folder and README.md survived the rename

**Step 3:** Commit

```bash
git add skills/
git commit -m "rename: kohya-config -> lora-trainer-guide"
```

---

### Task 2: Write SKILL.md

**Files:**
- Create: `skills/lora-trainer-guide/SKILL.md`

**Content structure:**
1. Frontmatter (name, description)
2. Overview — what this skill does
3. Opening behavior — ask experience level
4. Preset selection flow — model type → training type → recommended preset
5. Preset file format — how to read the JSON
6. What to tweak — key params per model type with sane ranges
7. Common pitfalls table

**Step 1:** Write SKILL.md with all sections

**Step 2:** Commit

```bash
git add skills/lora-trainer-guide/SKILL.md
git commit -m "feat: add lora-trainer-guide SKILL.md"
```

---

### Task 3: Write reference.md

**Files:**
- Create: `skills/lora-trainer-guide/reference.md`

**Content structure (trimmed from kohya-ss docs):**
1. Key parameter definitions table
2. LyCORIS algorithm comparison (from Guidelines.md star table + Algo-List.md)
3. Optimizer quick reference
4. Training time / VRAM / file size benchmarks (from LyCORIS Guidelines.md)

**Sources:**
- `Ktiseos-Nyx-Trainer/documentation/guides/training/general/train_network.md`
- `Ktiseos-Nyx-Trainer/documentation/guides/lycoris/Algo-List.md`
- `Ktiseos-Nyx-Trainer/documentation/guides/lycoris/Guidelines.md`
- `Ktiseos-Nyx-Trainer/documentation/guides/configuration/config_README-en.md`

**Step 1:** Write reference.md with all sections

**Step 2:** Commit

```bash
git add skills/lora-trainer-guide/reference.md
git commit -m "feat: add lora-trainer-guide reference.md"
```

---

### Task 4: Update presets/README.md

**Files:**
- Modify: `skills/lora-trainer-guide/presets/README.md`

**Step 1:** Update to reflect skill context (not trainer app context)

**Step 2:** Commit

```bash
git add skills/lora-trainer-guide/presets/README.md
git commit -m "docs: update presets README for skill context"
```

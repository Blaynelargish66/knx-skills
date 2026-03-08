# Design: lora-trainer-guide skill

**Date:** 2026-03-08
**Status:** Approved

## Summary

A skill that helps LLMs guide users through selecting and understanding LoRA training configuration presets. Ships with 38 real-world presets from the Ktiseos-Nyx-Trainer project covering SD1.5, SDXL, SD3, and Flux1.

## Scope (v1)

- Preset selection and parameter understanding only
- No end-to-end training workflow (v2)
- No dataset prep guidance (v2)

## Structure

```
skills/lora-trainer-guide/
  SKILL.md          # Opinionated guide — selection flow + key advice
  reference.md      # Parameter definitions, algorithm comparison, optimizer info
  presets/           # 38 JSON preset files (already exist)
    README.md        # Preset format documentation (already exists)
```

Renamed from `kohya-config` to `lora-trainer-guide`.

## SKILL.md Design

### Opening behavior
- Ask user: "How familiar are you with LoRA training?"
- Adjust response depth based on answer (beginner gets more explanation, experienced gets concise answers)

### Preset selection flow
1. What model architecture? (SD1.5 / SDXL / SD3 / Flux1)
2. What training type? (Standard LoRA / LoHA / LoKR / LoCon / iA3 / finetune / dreambooth)
3. Recommend matching preset(s) from the presets/ folder
4. Explain what the key params are set to and why

### "What to tweak" guidance
- Per model type: the params that matter, sane ranges, what to leave alone
- Common adjustments: learning rate, network dim/alpha, batch size, epochs vs steps

### Common pitfalls
- LoHa + high dim = NaN loss → lower LR
- network_alpha misunderstanding (alpha=1 vs alpha=dim behave very differently)
- SDXL clip_skip defaults
- Flux1 fp8 requirements

## reference.md Design

Trimmed from kohya-ss documentation (no Japanese, no setup instructions).

### Content
- Key parameter definitions table (param → what it does → typical values)
- LyCORIS algorithm comparison (fidelity/flexibility/diversity/size tradeoffs)
- Optimizer quick reference (AdamW, AdamW8bit, Prodigy, Adafactor, CAME, DAdaptAdam, Lion — when to use each)
- Training time/VRAM/file size reference table (from LyCORIS benchmarks)

## Audience

Anyone using Kohya SS / sd-scripts / Ktiseos-Nyx-Trainer. Adjusts depth based on user's stated experience level.

## What this does NOT cover (v2 candidates)

- End-to-end training workflow
- Dataset preparation and captioning
- Troubleshooting training failures
- Model merging
- Per-fronter preset preferences

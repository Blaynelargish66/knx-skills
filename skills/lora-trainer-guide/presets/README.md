# Training Configuration Presets

38 real-world training presets for LoRA, LyCORIS, finetuning, and dreambooth. Usable with Kohya SS, sd-scripts, and the Ktiseos-Nyx-Trainer.

## Coverage

| Model Type | Count | Training Types |
|-----------|-------|----------------|
| SDXL | 23 | LoRA, LoHA, LoKR, LoCon, finetune |
| SD 1.5 | 13 | LoRA, LoHA, LoKR, iA3, GLoRA |
| SD3 | 2 | Dreambooth |
| Flux1 | 1 | LoRA (fp8) |

## File Format

Each preset is a self-contained JSON file:

```json
{
  "name": "Human-readable preset name",
  "description": "What this preset is optimized for",
  "model_type": "SDXL",
  "config": {
    "LoRA_type": "Standard",
    "optimizer": "AdamW8bit",
    "network_dim": 32,
    "network_alpha": 32,
    "learning_rate": 0.0001
  },
  "is_builtin": true,
  "source": "bmaltais/kohya_ss"
}
```

Key fields for filtering:
- `model_type` — SD1.5, SDXL, SD3, or Flux1
- `config.LoRA_type` — Standard, Flux1, or LyCORIS variant
- `config.optimizer` — which optimizer the preset uses

The `config` object contains training hyperparameters only (no dataset or model paths).

## Attribution

Presets adapted from [bmaltais/kohya_ss](https://github.com/bmaltais/kohya_ss). Credit to bmaltais and contributors.

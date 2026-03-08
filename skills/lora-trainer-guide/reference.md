# LoRA Training Parameter Reference

Quick-reference for training parameters, algorithms, and optimizers. Trimmed from sd-scripts and LyCORIS documentation.

## Key Parameters

### Model & Architecture

| Parameter | Type | Description |
|-----------|------|-------------|
| `v2` | bool | Set `true` for Stable Diffusion v2.x base models |
| `v_parameterization` | bool | Set `true` for v-prediction models (SD 2.x 768px) |
| `sdxl` | bool | Set `true` for SDXL models |
| `fp8_base` | bool | Load base model in fp8 precision — required for Flux1 |
| `flux1_checkbox` | bool | Enable Flux1-specific training behavior |
| `model_prediction_type` | string | Prediction type — "raw" for Flux1, leave default for SD |

### LoRA Network

| Parameter | Type | Description | Typical values |
|-----------|------|-------------|----------------|
| `LoRA_type` | string | Network type: Standard, Flux1, or LyCORIS variant | "Standard" for most |
| `network_dim` | int | Rank (dimension) of the LoRA. Higher = more expressive, bigger file | 8–128, commonly 16–32 |
| `network_alpha` | int | Scales effective learning rate by `alpha/dim`. | 1 to same-as-dim |
| `conv_dim` | int | Rank for convolution layers (LoCon/LoHa/LoKR) | 1–32 |
| `conv_alpha` | int | Alpha for convolution layers | 1 to same-as-conv_dim |
| `network_dropout` | float | Dropout rate for LoRA layers during training | 0–0.3 |
| `module_dropout` | float | Probability of dropping entire LoRA modules per step | 0–0.1 |
| `rank_dropout` | float | Drops random ranks during training for regularization | 0–0.1 |
| `scale_weight_norms` | float | Constrains LoRA weight magnitude. 0 = disabled | 0 or 1.0 |

**The alpha/dim relationship:** The effective learning rate for LoRA weights is `learning_rate * (alpha / dim)`. So:
- `dim=32, alpha=32` → LR multiplied by 1 (no scaling)
- `dim=32, alpha=16` → LR multiplied by 0.5
- `dim=32, alpha=1` → LR multiplied by 1/32 (much smaller effective LR)

This is the single most misunderstood parameter interaction in LoRA training.

### Learning Rate & Optimizer

| Parameter | Type | Description | Typical values |
|-----------|------|-------------|----------------|
| `learning_rate` | float | Base learning rate | 1e-5 to 1e-3 (depends on optimizer) |
| `unet_lr` | float | Separate LR for U-Net LoRA modules | Same as or near learning_rate |
| `text_encoder_lr` | float | Separate LR for text encoder LoRA | Usually lower than unet_lr |
| `optimizer` | string | Optimization algorithm | See optimizer table below |
| `optimizer_args` | string | Extra args passed to optimizer | Optimizer-specific |
| `lr_scheduler` | string | How LR changes over training | "constant", "cosine", "constant_with_warmup" |
| `lr_warmup` | int | Steps/percentage to gradually ramp up LR | 0–500 steps or 0–10% |
| `lr_scheduler_num_cycles` | int | Cycles for cosine_with_restarts scheduler | 1–3 |

### Training Duration & Batching

| Parameter | Type | Description | Notes |
|-----------|------|-------------|-------|
| `max_train_epochs` | int | Total training epochs | Overrides max_train_steps if set |
| `max_train_steps` | int | Total training steps | Ignored if epochs is set |
| `train_batch_size` | int | Images per training step | Higher = more VRAM, adjust LR |
| `gradient_accumulation_steps` | int | Steps to accumulate before weight update | Effective batch = batch_size × this |
| `gradient_checkpointing` | bool | Trade speed for VRAM savings | Enable if tight on memory |
| `save_every_n_epochs` | int | Save a checkpoint every N epochs | 1 is safe for picking best epoch |
| `save_every_n_steps` | int | Save a checkpoint every N steps | Use for very long training runs |

### Resolution & Bucketing

| Parameter | Type | Description | Typical values |
|-----------|------|-------------|----------------|
| `max_resolution` | string | Training resolution as "W,H" | "512,512" (SD1.5), "1024,1024" (SDXL) |
| `enable_bucket` | bool | Allow variable aspect ratio images | Usually `true` |
| `min_bucket_reso` | int | Smallest bucket resolution | 256 |
| `max_bucket_reso` | int | Largest bucket resolution | 1024–2048 |
| `bucket_reso_steps` | int | Resolution step size for buckets | 64 |
| `bucket_no_upscale` | bool | Don't upscale images to fill buckets | Usually `true` |

### Caption & Text

| Parameter | Type | Description |
|-----------|------|-------------|
| `caption_extension` | string | File extension for caption files | Usually ".txt" |
| `shuffle_caption` | bool | Randomly reorder caption tags each step |
| `keep_tokens` | int | Number of leading tokens to keep fixed when shuffling |
| `max_token_length` | int | Max token length for text encoder (75, 150, or 225) |
| `caption_dropout_rate` | float | Chance to drop entire caption per image (forces learning without text) |
| `caption_dropout_every_n_epochs` | int | Drop all captions every N epochs |
| `weighted_captions` | bool | Enable attention weighting syntax in captions |
| `clip_skip` | int | Skip last N layers of text encoder — 2 for anime SD1.5, 1 for most others |

### Precision & Memory

| Parameter | Type | Description |
|-----------|------|-------------|
| `mixed_precision` | string | "fp16", "bf16", or "no" — bf16 preferred if GPU supports it |
| `full_fp16` | bool | Run entire training in fp16 |
| `full_bf16` | bool | Run entire training in bf16 |
| `save_precision` | string | Precision for saved model: "fp16", "bf16", or "float" |
| `cache_latents` | bool | Pre-compute and cache VAE latents (saves VRAM during training) |
| `cache_latents_to_disk` | bool | Cache latents to disk instead of RAM |
| `xformers` | string/bool | Attention implementation: "xformers", "sdpa", or true |
| `mem_eff_attn` | bool | Memory-efficient attention (alternative to xformers) |

### Noise & Loss

| Parameter | Type | Description | Typical values |
|-----------|------|-------------|----------------|
| `noise_offset` | float | Adds noise offset for better dark/light training | 0–0.1, commonly 0.05 |
| `noise_offset_type` | string | "Original" or "Multires" | "Original" |
| `multires_noise_iterations` | int | Iterations for multires noise (0 = disabled) | 0 or 6–10 |
| `multires_noise_discount` | float | Discount for multires noise | 0.3 |
| `min_snr_gamma` | int | Minimum SNR gamma for loss weighting — stabilizes training | 0 (off) or 5–7 |
| `adaptive_noise_scale` | float | Scale noise offset based on latent mean | 0 |
| `scale_v_pred_loss_like_noise_pred` | bool | Scale v-prediction loss to match noise prediction scale | Rarely needed |

### Flux1-Specific

| Parameter | Type | Description |
|-----------|------|-------------|
| `flux1_cache_text_encoder_outputs` | bool | Cache T5/CLIP outputs (highly recommended) |
| `flux1_cache_text_encoder_outputs_to_disk` | bool | Cache to disk to save RAM |
| `t5xxl_max_token_length` | int | Max tokens for T5-XXL encoder (usually 512) |
| `timestep_sampling` | string | How timesteps are sampled — "sigmoid" for Flux1 |
| `discrete_flow_shift` | float | Flow matching shift — usually 3 for Flux1 |
| `guidance_scale` | float | Classifier-free guidance during training |
| `apply_t5_attn_mask` | bool | Apply attention mask to T5 outputs |
| `split_mode` | bool | Split model across devices for large models |

---

## LyCORIS Algorithm Comparison

From LyCORIS project benchmarks (tested on SD1, RTX 4090). Stars: ★ > ◉ > ● > ▲ (★ = best)

| | Full | LoRA | LoHa | LoKr (small) | LoKr (large) |
|---|---|---|---|---|---|
| **Fidelity** | ★ | ● | ▲ | ◉ | ▲ |
| **Flexibility** | ★ | ● | ◉ | ▲ | ● |
| **Diversity** | ▲ | ◉ | ★ | ● | ★ |
| **File size** | ▲ | ● | ● | ● | ★ |
| **Speed (linear)** | ★ | ● | ● | ★ | ★ |
| **Speed (conv)** | ● | ★ | ▲ | ● | ● |

- **Fidelity** — how accurately it reproduces training data
- **Flexibility** — generating images unlike training data, combining concepts, switching base models
- **Diversity** — variety in outputs
- **File size** — smaller is better in this table
- **LoKr small** — factor=-1 (900KB–2.5MB files)
- **LoKr large** — factor~8, full dimension (LoRA-like behavior)

### When to use what

- **Best overall quality:** Full (native fine-tuning via `algo=full`) — but largest files
- **General purpose:** Standard LoRA — good balance of quality, size, and flexibility
- **Easy concepts / multi-concept:** LoHa — natural dampening prevents over-learning
- **Tiny file size:** LoKr with factor=-1 — under 2.5MB, but may not transfer between base models
- **Style learning, ultra-tiny:** iA3 — under 1MB, needs high LR (5e-3 to 1e-2), hard to transfer
- **Unsure which dim is best:** DyLoRA — trains all ranks simultaneously, resize after

### Recommended dimensions

| Algorithm | Recommended dim | Alpha |
|-----------|----------------|-------|
| LoRA / LoCon | 8–64 | 1 to half-dim |
| LoHa | 4–32 | 1 to half-dim |
| LoKr (small) | factor=-1 | N/A |
| LoKr (large) | full (set dim very high like 10000) | Ignored |
| DyLoRA | 64–128 | dim/4 to dim |
| iA3 | N/A | N/A |

**Warning:** High dim with LoHa can cause loss to go NaN. Use lower LR if you need high dim LoHa.

---

## Optimizer Reference

| Optimizer | LR Range | VRAM | Best for | Notes |
|-----------|----------|------|----------|-------|
| AdamW | 1e-5 to 1e-3 | High | Baseline, reliable | Full precision, most VRAM |
| AdamW8bit | 1e-5 to 1e-3 | Medium | General use | 8-bit quantized, nearly as good as AdamW |
| PagedAdamW8bit | 1e-5 to 1e-3 | Medium | Low-VRAM setups | Pages optimizer states to CPU when needed |
| Prodigy | 1.0 (auto-adapts) | Medium | "Just works" LR | Self-adjusting, set LR=1 and let it figure it out |
| DAdaptAdam | 1.0 (auto-adapts) | Medium | Auto-tuning | Similar idea to Prodigy |
| Adafactor | 1e-5 to 1e-3 | Low | Memory-constrained | Very memory efficient but can be finicky |
| CAME | 1e-5 to 1e-3 | Medium | Newer alternative | Confidence-guided adaptive memory efficient |
| Lion | 1e-5 to 1e-4 | Low | Experimental | Simpler algorithm, mixed results |

**Key notes:**
- Prodigy and DAdaptAdam set their own LR — pass `learning_rate=1.0` and don't fight them
- AdamW8bit is the safe default for most people
- Adafactor saves the most VRAM but needs careful tuning
- If using Prodigy, the preset LR values in config are likely too low — check and set to 1.0

---

## Training Time / VRAM / File Size Benchmarks

RTX 4090, batch size 8, ~50K steps, AdamW8bit. From LyCORIS project testing.

| Algorithm | Preset | Dim [Factor] | Time | VRAM (MiB) | File Size |
|-----------|--------|-------------|------|------------|-----------|
| LoRA | attn-mlp | 1 | 4hr | 16,410 | 1.7 MB |
| LoRA | attn-mlp | 8 | 4hr | 16,358 | 9.5 MB |
| LoRA | attn-only | 32 | 3hr 25min | 15,222 | 18 MB |
| LoRA | attn-mlp | 32 | 4hr 5min | 15,420 | 37 MB |
| LoRA | full | 32 | 4hr 30min | 17,350 | 75 MB |
| LoRA | attn-mlp | 64 | 4hr 5min | 16,168 | 73 MB |
| LoHa | full | 4 | 4hr 10min | 15,960 | 9.6 MB |
| LoHa | attn-only | 16 | 3hr 30min | 15,244 | 18 MB |
| LoHa | attn-mlp | 16 | 4hr 10min | 16,876 | 37 MB |
| LoHa | full | 16 | 5hr 40min | 16,750 | 75 MB |
| LoKr | attn-mlp | 1 [-1] | 3hr 50min | 16,812 | 940 KB |
| LoKr | attn-mlp | full [-1] | 3hr 45min | 16,806 | 1.6 MB |
| LoKr | attn-mlp | full [8] | 3hr 40min | 16,034 | 12 MB |
| LoKr | attn-mlp | full [4] | 3hr 45min | 16,116 | 43 MB |
| iA3 | — | — | 3hr 7min | 14,954 | 596 KB |
| Full | attn-only | — | 3hr 15min | 15,912 | 233 MB |
| Full | attn-mlp | — | 3hr 50min | 18,668 | 673 MB |
| Full | full | — | 5hr 20min | 23,008 | 1.8 GB |

**Preset column meaning:**
- `attn-only` — train attention layers only
- `attn-mlp` — train attention + MLP layers (recommended default)
- `full` — train all layers including convolution (biggest files, most VRAM)

---

## Attribution

Parameter documentation adapted from [sd-scripts](https://github.com/kohya-ss/sd-scripts) by kohya-ss.
Algorithm comparison and benchmarks from [LyCORIS](https://github.com/KohakuBlueleaf/LyCORIS) by KohakuBlueleaf.
Presets originally from [bmaltais/kohya_ss](https://github.com/bmaltais/kohya_ss).

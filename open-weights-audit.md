# Never Ordinary — Open Weights Only audit

Performed Oct 29, 2026, against the live ComfyUI server (v0.37.2) that renders the entry, via its node registry and model inventory endpoints. Purpose: evidence that the submitted workflow uses **no partner nodes, no closed-source APIs, and open-weights models only**.

## Node audit — all 14 classes are ComfyUI core

| Node class | Python module | Source |
|---|---|---|
| UNETLoader | `nodes` | core |
| CLIPLoader | `nodes` | core |
| VAELoader | `nodes` | core |
| LoadImage | `nodes` | core |
| VAEDecode | `nodes` | core |
| MiniMaxH3ReferenceToVideo | `comfy_extras.nodes_minimax_h3` | core |
| RandomNoise | `comfy_extras.nodes_custom_sampler` | core |
| KSamplerSelect | `comfy_extras.nodes_custom_sampler` | core |
| BasicScheduler | `comfy_extras.nodes_custom_sampler` | core |
| BasicGuider | `comfy_extras.nodes_custom_sampler` | core |
| SamplerCustomAdvanced | `comfy_extras.nodes_custom_sampler` | core |
| VAEDecodeAudio | `comfy_extras.nodes_audio` | core |
| CreateVideo | `comfy_extras.nodes_video` | core |
| SaveVideo | `comfy_extras.nodes_video` | core |

No class resolves to `comfy_api_nodes` (ComfyUI's partner/API node package) and none to any third-party custom-node pack. The graph contains no API, HTTP, or cloud-service node of any kind; every input is a local file and every output is written locally.

## Model audit — the four files the workflow loads, all present on the local server

| Role | File | Verified in live inventory |
|---|---|---|
| Diffusion (reference-to-video) | `minimax_h3_ref2va_pruned_int8_convrot.safetensors` | yes — `diffusion_models` |
| Text encoder | `qwen3vl_32b_minimax_h3_int8_convrot.safetensors` | yes — `text_encoders` |
| Video VAE | `minimax_h3_video_vae_fp16.safetensors` | yes — `vae` |
| Audio VAE | `minimax_h3_audio_vae_fp32.safetensors` | yes — `vae` |

All four are MiniMax H3 open-weights releases (Hugging Face), run locally on one consumer GPU (RTX 5070 Ti, 16 GB). No other model is referenced by the workflow.

## Conclusion

The Never Ordinary workflow, as packaged (`everyday-epic.ui.json` / `everyday-epic.api.json`), satisfies the Open Weights Only condition by construction and by this audit: core nodes only, local open-weights models only, no external calls.

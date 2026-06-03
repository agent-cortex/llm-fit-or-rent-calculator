# LLM Fit-or-Rent Calculator

A static, no-backend calculator for estimating whether a large language model fits on a local GPU or should be rented on cloud hardware.

Live production domain: https://vram-calculator.agentcortex.space/

Vercel fallback URL: https://vram-calculator-agentcortex.vercel.app/

## What it calculates

- Model weight memory: `parameters_B × bitrate / 8`
- GQA-aware KV cache memory: `2 × layers × kv_heads × (hidden_size / query_heads) × context_tokens × kv_bytes / 1e9`
- Total VRAM: `weights + KV cache + runtime overhead`
- Generation time: `output_tokens / tokens_per_second`
- Simple rental cost estimate: `generation_time_hours × hourly_gpu_price`

The core formulas follow adidshaft's “Quick Estimator for LLM Fit-or-Rent Decisions” article on X.

## Data refresh — Jun 2026

Updated presets now include current local-inference targets:

- Qwen3 8B / 14B / 32B
- Llama 3.1 8B and Llama 3.3 70B
- Mistral Small 3.2 24B
- DeepSeek R1 Distill Qwen 32B
- Phi-4 Mini
- RTX 5090 32GB, RTX PRO 6000 Blackwell 96GB, and Mac Studio M3 Ultra unified-memory options

Sources used for this refresh:

- Hugging Face public model API/config metadata for Qwen3, DeepSeek, Mistral Small 3.2, Phi-4 Mini, and Meta Llama parameter counts
- Apple Mac Studio technical specifications
- NVIDIA GeForce RTX 5090 specifications
- RunPod public GPU pricing for cloud hourly defaults

Mac Studio entries use a conservative ~75% usable unified-memory budget because not all unified memory is practically available as model VRAM.

## Use locally

Open `index.html` in a browser:

```bash
xdg-open index.html
```

Or serve it locally:

```bash
python3 -m http.server 8765
```

Then visit `http://127.0.0.1:8765/`.

## Notes

This is a planning estimator, not a benchmark. Real memory usage depends on framework overhead, attention implementation, batch size, tensor parallelism, quantization format, GPU fragmentation, and model-specific architecture.

## Contributor

- [@megabyte0x](https://github.com/megabyte0x)
- X: [@weekend__builds](https://x.com/weekend__builds)

## Credits

Core fit-or-rent formulas and framing are inspired by adidshaft's [Quick Estimator for LLM Fit-or-Rent Decisions](https://x.com/adidshaft/status/2047312245290668440) X article.

## License

MIT

# LLM Fit-or-Rent Calculator

A static, no-backend calculator for estimating whether a large language model fits on a local GPU or should be rented on cloud hardware.

Live production domain: https://vram-calculator.agentcortex.space/

Vercel fallback URL: https://vram-calculator-agentcortex.vercel.app/

## What it calculates

- Model weight memory: `parameters_B × bytes_per_param`
- KV cache memory: `2 × layers × hidden_size × context_tokens × kv_bytes / 1e9`
- Total VRAM: `weights + KV cache + runtime overhead`
- Generation time: `output_tokens / tokens_per_second`
- Simple rental cost estimate: `generation_time_hours × hourly_gpu_price`

The core formulas follow adidshaft's “Quick Estimator for LLM Fit-or-Rent Decisions” article on X.

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

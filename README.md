# Vito Lin

I work on the open-source LLM serving stack: inference routing, GPU fault handling, and the vLLM engine. Most of my contributions start from a failure I traced to its root cause and then fixed upstream.

## In progress

**vLLM: engine lifecycle**

- Requests still running at the `--shutdown-timeout` deadline never received a final chunk, and the API server never exited. The fix aborts the remaining work at the deadline ([#64991](https://github.com/vllm-project/vllm/pull/64991), in review), with a matching drain-shutdown section for the Kubernetes guide ([#64995](https://github.com/vllm-project/vllm/pull/64995), in review).

**vLLM: Rust frontend parity with Python**

- A partially cached Hub snapshot made the frontend skip `generation_config.json`, which broke stop tokens ([#64768](https://github.com/vllm-project/vllm/pull/64768), in review).
- An `auto` tool parser that cannot resolve for the served model now warns at startup instead of failing on every request ([#64069](https://github.com/vllm-project/vllm/pull/64069), in review).

**NVSentinel**

- The MongoDB store refuses to start on Linux kernel 6.19 and later, including Ubuntu kernels that already carry the upstream fix ([#2410](https://github.com/NVIDIA/NVSentinel/issues/2410)).
- The demo setup script hides the Helm error when an install fails before any pod is created ([#2411](https://github.com/NVIDIA/NVSentinel/issues/2411), assigned).

**llm-d-router**

- Guidance on choosing `onMissing` for load metrics in the endpoint attribute filter ([#4244](https://github.com/llm-d/llm-d-router/pull/4244), in review).
- The latency SLO admitter's idle check reads a different signal than its README documents ([#4246](https://github.com/llm-d/llm-d-router/issues/4246), assigned).

## Investigations

**vLLM: memory accounting**

- A cold boot gets 256 MiB less KV cache than a warm one with identical flags. I traced the gap to a compile-time allocation that lands inside the memory-profiling window, then compared both proposed upstream fixes on the same machine ([write-up](https://vito-lin-dev.github.io/profile/writing/2026-10-vllm_cold_start_kv_cache/)).
- A startup log reports the CUDA graph memory estimate as 2150% too high. Measuring the allocation directly showed the estimate was within 3% ([write-up](https://vito-lin-dev.github.io/profile/writing/2026-10-vllm_cudagraph_estimate_log/)).

## Merged

- vllm-project/vllm [#63580](https://github.com/vllm-project/vllm/pull/63580): Rust frontend returns 400 instead of 500 when tool parsing is disabled, matching Python
- apache/spark [#59352](https://github.com/apache/spark/pull/59352), [#59377](https://github.com/apache/spark/pull/59377): PySpark test coverage and typing cleanup
- NVIDIA/cuda-rust [#38](https://github.com/NVIDIA/cuda-rust/pull/38), [#73](https://github.com/NVIDIA/cuda-rust/pull/73), [#75](https://github.com/NVIDIA/cuda-rust/pull/75): installation docs

## Links

[Blog](https://vito-lin-dev.github.io/profile/) · [LinkedIn](https://www.linkedin.com/in/vitothedev)

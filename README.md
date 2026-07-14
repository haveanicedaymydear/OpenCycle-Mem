# OpenCycle-Mem

**Reversible Evidence Convergence and Governed Memory for Long-Horizon Agents**

OpenCycle-Mem studies how an autonomous agent can consolidate repeatedly supported conclusions into reusable checkpoints without turning memory into an irreversible store of model-generated claims.

## Research question

> Can reversible, provenance-aware checkpoints reduce redundant long-horizon deliberation while preventing unsupported, contaminated, or obsolete beliefs from becoming persistent agent memory?

## v0.3: evidence-convergent checkpoints

The core contribution is a reversible checkpoint lifecycle:

```text
open -> active -> reopened -> active
                  \-> revoked
```

A checkpoint becomes active only after stable, independent, provenance-verified support. Later contradictory evidence can reopen or revoke it. A hysteresis band separates activation and reopening thresholds, reducing memory churn under noisy evidence.

The effective evidence mass combines confidence, source trust, provenance validity, source independence, and temporal decay. The system stores concise decision summaries and evidence references; it does not require hidden chain-of-thought storage.

## What is public

This standalone repository contains:

- a runnable implementation of the v0.3 convergence mechanism;
- raw-memory and static-governance baselines;
- five controlled evidence-stream scenarios;
- 1,000-episode aggregate results per method;
- automated tests for promotion, contradiction reopening, and contamination rejection;
- a concise research note documenting assumptions, baselines, and limitations.

A larger modular development package additionally contains an OpenAI-compatible structured-output adapter, a temporal pilot runner, four memory conditions, and a 30-task pilot suite. This public snapshot intentionally focuses on the core algorithm and fully reproducible controlled evaluation.

## Quick start

Requires Python 3.10+ and no third-party runtime dependencies.

```bash
python opencycle_mem_v03.py
python -m unittest discover -s tests -v
```

Running the simulation regenerates:

```text
results/v0.3_simulation/summary.csv
results/v0.3_simulation/summary.json
```

## Controlled simulation result

All methods receive the same evidence streams across five scenario families and 200 shared seeds, giving 1,000 episodes per method.

| Method | Episodes | Mean state accuracy | False-active rate | Mean reopening latency |
|---|---:|---:|---:|---:|
| Raw memory | 1,000 | 0.5000 | 0.5000 | N/A |
| Static governed memory | 1,000 | 0.8000 | 0.1998 | N/A |
| OpenCycle-Mem v0.3 | 1,000 | **0.9496** | **0.0501** | **0.5075 cycles** |

The raw-memory baseline activates after any supporting observation and never revises. The static-governance baseline requires trusted, provenance-valid support but cannot reopen an activated checkpoint. OpenCycle-Mem uses the same evidence streams while adding reversible reopening and revocation.

These are controlled algorithmic results, not real-LLM performance claims. Real-agent effectiveness still requires declared model runs, stronger tasks, multiple independent run sets, and manual error analysis.

## Multi-agent extension

The current implementation is single-agent. A planned extension studies whether the same evidence weighting and reopening dynamics can govern shared beliefs and selective communication in decentralized agent teams. This is a research direction, not a completed multi-agent result.

## Repository structure

```text
.
├── opencycle_mem_v03.py
├── tests/test_v03.py
├── results/v0.3_simulation/
├── docs/research-note.md
├── pyproject.toml
└── .github/workflows/tests.yml
```

## Status

Independent open-source research in progress, July 2026.

## Author

Livan Zhou - [zhoulivan@gmail.com](mailto:zhoulivan@gmail.com)

## License

MIT License. See [LICENSE](LICENSE).

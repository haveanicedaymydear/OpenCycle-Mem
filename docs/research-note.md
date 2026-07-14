# OpenCycle-Mem v0.3 Research Note

## Problem

Long-horizon agents can save computation by consolidating repeatedly supported conclusions into reusable memory. A conventional append-only memory, however, can preserve unsupported, contaminated, duplicated, or obsolete claims after the environment changes.

OpenCycle-Mem treats consolidation as a reversible evidence-governance problem rather than a one-way summarisation step.

## Mechanism

Each candidate checkpoint receives evidence items with:

- stance: support, contradict, or neutral;
- confidence;
- source identity;
- source trust;
- provenance validity;
- time of availability.

Evidence is discounted by age and by repeated use of the same source. Unverified provenance receives a strong penalty. A checkpoint is promoted only after stable support from enough independent trusted sources.

The lifecycle is:

```text
open -> active -> reopened -> active
                  \-> revoked
```

Separate promotion, reopening, and revocation thresholds create hysteresis. This prevents a single noisy observation from repeatedly toggling the memory state while still allowing strong contradictions to reopen or revoke an obsolete checkpoint.

## Baselines

All methods receive identical generated evidence streams.

1. **Raw memory**: activates after any supporting evidence and never revises.
2. **Static governed memory**: requires stable trusted support but cannot reopen after activation.
3. **OpenCycle-Mem v0.3**: applies the same trusted-evidence requirement and adds temporal decay, provenance weighting, duplicate-source discounting, reopening, and revocation.

The baselines are intentionally minimal. They isolate the effect of reversible governance; they are not claimed to represent the strongest published memory architectures.

## Controlled scenarios

The public simulation uses five scenario families:

- clean support;
- delayed contradiction;
- low-trust contamination;
- noisy but ultimately true evidence;
- regime shift.

Each method is evaluated on 200 shared seeds per scenario, or 1,000 episodes per method. The expected active/inactive state is defined by each scenario generator.

## Results

OpenCycle-Mem v0.3 reaches 0.9496 mean state accuracy, compared with 0.8000 for static governance and 0.5000 for raw memory. The false-active rate falls to 0.0501, and obsolete checkpoints reopen in 0.5075 cycles on average.

These results establish deterministic behaviour in a controlled algorithmic setting. They do not establish end-to-end gains for a deployed LLM agent.

## Limitations

- Scenario generators are synthetic and hand specified.
- Hyperparameters are not yet tuned on a held-out scenario family.
- The baselines are diagnostic rather than state of the art.
- The public snapshot does not yet measure task success, token cost, or human-rated memory quality in a real agent.
- Multi-agent memory and selective communication are planned extensions, not completed results.

## Next evaluation stage

The next stage will compare governed memory with stateless, raw, and summary-memory conditions in declared model runs, using multiple seeds, temporal tasks, cost accounting, and manual error analysis. A later extension will investigate shared-belief revision and uncertainty-triggered communication in decentralized teams.

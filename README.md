# rule-lantern

A tiny duel that lives as an Aura program. The only thing that evolves is `rule`.

Each turn the rule sees `(hp ehp turn)` and returns an action:

- `0` wait (heal 1, cap 8)
- `1` strike (deal 2)
- `2` dodge (negate the opponent's strike)

**Arena L:** B strike pattern (odd always; even from turn ≥5), max **36** turns, plus **fatigue**:
from turn ≥10, choosing wait also deals **1 chip** to self (late wait-banking tax).
Score is damage dealt plus turns lived. Higher is better. HP/EHP clamped 0-8; start hp=6 ehp=8.

## Loop

1. A guide proposes a new `rule` body.
2. Soft runs `play` on that body (oneshot for now).
3. Keep the body only when the Soft score is strictly higher than the kept generation.
4. Commit the winner into `lantern.aura` and append a measured row to `generations.md`.

Scores are invented nowhere. Soft tip for measured rows: `6a13b3d`, binary `/workspace/aura-grok/build/aura`, run inside `ghcr.io/cybrid-systems/dev:v1.0.9` with `AURA_PATH=/workspace/aura-grok/lib` and `AURA_PIPELINE_STRICT=0`.

## Status

Kept generation **22**, Soft score **44** (arena L ceiling; no Soft strict improve over finish-on-last). See `generations.md`.

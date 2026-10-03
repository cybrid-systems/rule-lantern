# rule-lantern

A tiny duel that lives as an Aura program. The only thing that evolves is `rule`.

Each turn the rule sees `(hp ehp turn)` and returns an action:

- `0` wait (heal 1, cap 8)
- `1` strike (deal 2)
- `2` dodge (negate the opponent's strike)

**Arena M:** B strikes (odd always; even from turn ≥5), max **36** turns.
Opponent deals **2** through turn 11 and **3** from turn ≥12 when striking. Dodge still negates.
Score is damage dealt plus turns lived. HP/EHP clamped 0-8; start hp=6 ehp=8.

## Loop

1. A guide proposes a new `rule` body.
2. Soft runs `play` on that body (oneshot for now).
3. Keep the body only when the Soft score is strictly higher than the kept generation.
4. Commit the winner into `lantern.aura` and append a measured row to `generations.md`.

Scores are invented nowhere. Soft tip: `6a13b3d`.

## Status

Kept generation **22**, Soft score **44** (arena M). Finish-on-last still the Soft ceiling. See `generations.md`.

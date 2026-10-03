# rule-lantern

A tiny duel. The evolving part is `rule`.

- `0` wait (heal 1, cap 8)
- `1` strike (deal 2)
- `2` dodge (negate the opponent strike)

**Arena O:** B strikes, opp deals 3 from turn >=6 (else 2), max **48** turns.
Score = damage dealt + turns lived. Start hp=6 ehp=8, clamp 0-8.

Soft tip `6a13b3d`. Keep only on a strictly higher Soft score.

## Status

Kept generation **28**, Soft score **56** (arena O). See `generations.md`.

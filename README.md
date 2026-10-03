# rule-lantern

A tiny duel. The evolving part is `rule`.

- `0` wait (heal 1, cap 8)
- `1` strike (deal 2)
- `2` dodge (negate the opponent strike)

**Arena P:** B strikes, opp deals 3 from turn >=6 (else 2), max **48** turns,
plus **dodge attrition**: from turn >=20 a successful dodge still costs **1 hp**.

Score = damage dealt + turns lived. Start hp=6 ehp=8. Soft tip `6a13b3d`.

## Status

Kept generation **30**, Soft score **30** (arena P). See `generations.md`.

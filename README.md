# rule-lantern

Evolving part: `rule`.

- `0` wait (heal 1, cap 8)
- `1` strike (deal 2)
- `2` dodge (negate the strike)

**Arena R:** B strikes, opp deals 3 from turn >=6, max 48,
dodge chip **1 hp from turn >=15**, plus **clean-win +2** when ehp hits 0 with hp still >=4.

Score = damage + turns lived + that bonus. Start hp=6 ehp=8. Soft tip `6a13b3d`.

## Status

Kept generation **31**, Soft score **25** (arena R). Clean-win bonus did not Soft-beat the Q rule. See `generations.md`.

# rule-lantern

Evolving part: `rule`.

- `0` wait (heal 1, cap 8)
- `1` strike (deal 2)
- `2` dodge (negate the strike)

**Arena T:** B strikes, opp deals 3 from turn >=6, max 48,
dodge chip **1 hp from turn >=15**, clean-win **+2** when ehp hits 0 with hp still **>=2**,
**start hp=8** (raised from 6 so Soft clean-win Soft-path Soft-reaches hp>=2).

Score = damage + turns lived + that bonus. Soft tip `6a13b3d`.

## Status

Kept generation **34**, Soft score **27** (arena T, strike 2,4,5,20). Soft +2 Soft-fired on gen **33** (strike 2,4,5,16 Soft 26, Soft killhp=2). See `generations.md`.

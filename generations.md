# Generations

Only rows with Soft tip + score come from a Soft `play` run. Do not fill a score by hand.

Soft tip: `6a13b3d` · image: `ghcr.io/cybrid-systems/dev:v1.0.9`

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 0 | measured | 12 | 6a13b3d | always strike `(=> 1)` | seed |
| 1 | measured kept briefly | 16 | 6a13b3d | dodge odd, strike even | beat gen 0 |
| 1b | discarded | 16 | 6a13b3d | dodge odd; even heal if hp≤3 else strike | not strictly higher than 16 |
| 2 | **kept** | **20** | 6a13b3d | dodge odd; even wait if turn<6 else strike | delay the kill, rack lived |
| 3 | discarded | 18 | 6a13b3d | dodge odd; even wait if turn<8 else strike | below kept 20 |

Under the current duel physics, max score is **20** (8 damage to kill + 12 lived turns). Gen 2 hit that ceiling. Further `rule`-only proposals cannot beat it without changing the arena.

## How a generation lands

The guide writes the next `rule`. Soft scores it. If score > kept, `lantern.aura` becomes that body and this table gets a kept row. Else the proposal is discarded here and `lantern.aura` stays on the winner.

### Next product move (not a rule tweak)

Raise the ceiling: make the opponent strike every turn starting turn 5, or add a third actor. Then resume `rule` evolution under the new Soft scores.

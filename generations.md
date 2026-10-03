# Generations

Only rows with Soft tip + score come from a Soft `play` run. Do not fill a score by hand.

Soft tip: `6a13b3d` · image: `ghcr.io/cybrid-systems/dev:v1.0.9`

## Arena A (legacy): opp strikes odd only

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 0 | measured | 12 | 6a13b3d | always strike `(=> 1)` | seed |
| 1 | measured kept briefly | 16 | 6a13b3d | dodge odd, strike even | beat gen 0 |
| 1b | discarded | 16 | 6a13b3d | dodge odd; even heal if hp≤3 else strike | not strictly higher than 16 |
| 2 | kept (arena A ceiling) | 20 | 6a13b3d | dodge odd; even wait if turn<6 else strike | delay the kill, rack lived |
| 3 | discarded | 18 | 6a13b3d | dodge odd; even wait if turn<8 else strike | below kept 20 |

Arena A ceiling was **20** (8 damage + 12 lived). Gen 2 hit it.

## Arena B (raised): opp strikes odd always; even also from turn ≥5

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 4 | measured baseline | 19 | 6a13b3d | same as gen 2 (dodge odd; even wait if turn<6 else strike) | arena-reset Soft measure of old kept rule |
| 4b | discarded | 12 | 6a13b3d | always dodge `(=> 2)` | naive baseline under arena B |
| 4c | discarded | 16 | 6a13b3d | dodge when opp strikes; strike on safe even (<5) | free strikes only |
| 5 | **kept** | **20** | 6a13b3d | strike on turns 2,4,6,12; else dodge | free×2 + mid trade + late kill |
| 5b | discarded | 20 | 6a13b3d | strike on turns 2,4,8,12; else dodge | equal to kept 20 |
| 5c | discarded | 20 | 6a13b3d | wait T2; strike 4,6,10,12; else dodge | equal to kept 20 |
| 5d | discarded | 14 | 6a13b3d | dodge opp-strike; wait T2; strike other safe | below kept 20 |

Under arena B, max score is still **20** (8 damage to kill + 12 lived). Gen 5 hit that ceiling.

## How a generation lands

The guide writes the next `rule`. Soft scores it. If score > kept, `lantern.aura` becomes that body and this table gets a kept row. Else the proposal is discarded here and `lantern.aura` stays on the winner.

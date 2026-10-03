# Generations

Only rows with Soft tip + score come from a Soft `play` run. Do not fill a score by hand.

Soft tip: `6a13b3d` * image: `ghcr.io/cybrid-systems/dev:v1.0.9`

## Arena A (legacy): opp strikes odd only; max 12 turns

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 0 | measured | 12 | 6a13b3d | always strike `(=> 1)` | seed |
| 1 | measured kept briefly | 16 | 6a13b3d | dodge odd, strike even | beat gen 0 |
| 1b | discarded | 16 | 6a13b3d | dodge odd; even heal if hp<=3 else strike | not strictly higher than 16 |
| 2 | kept (arena A ceiling) | 20 | 6a13b3d | dodge odd; even wait if turn<6 else strike | delay the kill, rack lived |
| 3 | discarded | 18 | 6a13b3d | dodge odd; even wait if turn<8 else strike | below kept 20 |

Arena A ceiling was **20** (8 damage + 12 lived). Gen 2 hit it.

## Arena B: opp strikes odd always; even also from turn >=5; max 12

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 4 | measured baseline | 19 | 6a13b3d | same as gen 2 | arena-reset Soft measure of old kept rule |
| 4b | discarded | 12 | 6a13b3d | always dodge `(=> 2)` | naive baseline under arena B |
| 4c | discarded | 16 | 6a13b3d | dodge when opp strikes; strike on safe even (<5) | free strikes only |
| 5 | kept (arena B ceiling) | 20 | 6a13b3d | strike on turns 2,4,6,12; else dodge | freex2 + mid trade + late kill |
| 5b | discarded | 20 | 6a13b3d | strike on turns 2,4,8,12; else dodge | equal to kept 20 |
| 5c | discarded | 20 | 6a13b3d | wait T2; strike 4,6,10,12; else dodge | equal to kept 20 |
| 5d | discarded | 14 | 6a13b3d | dodge opp-strike; wait T2; strike other safe | below kept 20 |

Arena B ceiling **20**. Gen 5 hit it.

## Arena C probe (not adopted): arena B strikes + opp deals 3 from turn >=7

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 6p | measured | 20 | 6a13b3d | gen 5 rule under C | still ceiling 20; no new headroom |
| 6p1 | discarded | 14 | 6a13b3d | strike 2,4,5,6 | early kill |
| 6p2 | discarded | 18 | 6a13b3d | strike 2,4,6 only | leave ehp |
| 6p3 | discarded | 16 | 6a13b3d | strike 2,4,6,8 | mid kill under 3-dmg |
| 6p5 | discarded | 20 | 6a13b3d | strike 2,4,5,12 | equal, not adopted |

## Arena D probe (not adopted): arena B strikes + opp deals 3 from turn >=5

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 7p | measured | 19 | 6a13b3d | gen 5 rule under D | baseline; apparent ceiling 19 |
| 7p1 | discarded | 16 | 6a13b3d | strike 2,4 only | below |
| 7p2 | discarded | 18 | 6a13b3d | strike 2,4,6 | below |
| 7p5 | discarded | 15 | 6a13b3d | strike 2,4,6,8 | below |
| 7p7 | discarded | 19 | 6a13b3d | strike 2,4,8,12 | equal to baseline, no keep |

No strict improve under D; raised match length instead.

## Arena E: arena B strikes; max **14** turns

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 8 | measured baseline | 20 | 6a13b3d | strike 2,4,6,12; else dodge | gen 5 rule under longer match |
| 8b | discarded | 14 | 6a13b3d | always dodge | naive |
| 8c | discarded | 18 | 6a13b3d | strike 2,4 only | free only |
| 8d | discarded | 18 | 6a13b3d | strike 2,4,6,10 | early kill |
| 8e | discarded | 19 | 6a13b3d | gen 2 style (dodge odd; even wait if turn<6 else strike) | below |
| 9 | **kept** | **22** | 6a13b3d | strike on turns 2,4,6,14; else dodge | delay kill to turn 14 |
| 9b | discarded | 22 | 6a13b3d | strike on turns 2,4,8,14; else dodge | equal to kept 22 |

Arena E ceiling **22** (8 damage + 14 lived). Gen 9 hit it.

## Arena F: arena B strikes; max **16** turns

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 10 | measured baseline | 22 | 6a13b3d | strike 2,4,6,14; else dodge | gen 9 rule under longer match |
| 10b | discarded | 16 | 6a13b3d | always dodge | naive |
| 10c | discarded | 20 | 6a13b3d | strike 2,4,6,12 | early kill under F |
| 11 | **kept** | **24** | 6a13b3d | strike on turns 2,4,6,16; else dodge | delay kill to turn 16 |
| 11b | discarded | 24 | 6a13b3d | strike on turns 2,4,8,16; else dodge | equal to kept 24 |

Arena F ceiling **24** (8 damage + 16 lived). Gen 11 hit it.

## Arena G (current): arena B strikes; max **20** turns

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 12 | measured baseline | 24 | 6a13b3d | strike 2,4,6,16; else dodge | gen 11 rule under longer match |
| 12b | discarded | 20 | 6a13b3d | always dodge | naive |
| 12c | discarded | 20 | 6a13b3d | strike 2,4,6,12 | early kill |
| 12d | discarded | 26 | 6a13b3d | wait T2; strike 4,6,20 | below kept after G1 |
| 13 | **kept** | **28** | 6a13b3d | strike on turns 2,4,6,20; else dodge | delay kill to turn 20 |
| 13b | discarded | 28 | 6a13b3d | strike on turns 2,4,8,20; else dodge | equal to kept 28 |
| 13c | discarded | 28 | 6a13b3d | strike on turns 2,4,10,20; else dodge | equal to kept 28 |

Arena G ceiling **28** (8 damage + 20 lived). Gen 13 hit it.

## How a generation lands

The guide writes the next `rule`. Soft scores it. If score > kept, `lantern.aura` becomes that body and this table gets a kept row. Else the proposal is discarded here and `lantern.aura` stays on the winner.

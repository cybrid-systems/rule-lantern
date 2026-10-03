# Generations

Only rows with Soft tip + score come from a Soft `play` run. Do not fill a score by hand.

Soft tip: `6a13b3d` * image: `ghcr.io/cybrid-systems/dev:v1.0.9`

Prior Soft arenas A-J (ceilings 20..40, finish-on-last keeps) are recorded in earlier main commits (through `3875f0a` / `16151bc`). Summaries below focus on this turn's Soft work.

## Arena K: arena B strikes; max **36** turns

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 20 | measured baseline | 40 | 6a13b3d | strike 2,4,6,32; else dodge | gen 19 rule under longer match |
| 20b | discarded | 36 | 6a13b3d | always dodge | naive |
| 20c | discarded | 42 | 6a13b3d | wait T2; strike 4,6,36 | below kept 44 |
| 21 | **kept** | **44** | 6a13b3d | strike on turns 2,4,6,36; else dodge | delay kill to turn 36 |
| 21b | discarded | 44 | 6a13b3d | strike on turns 2,4,8,36; else dodge | equal to kept 44 |

Arena K ceiling **44** (8 damage + 36 lived). Gen 21 hit it.

## Arena L (current): B strikes; max 36; wait fatigue chip from turn >=10

Under B strikes, opp is always striking from turn 5, so "chip only when wait AND opp idle" never fires after turn 4. Soft arena L therefore taxes **any wait from turn >=10** (+1 self chip). That Soft-punishes late wait-banking while finish-on-last (no late waits) still reaches the lived ceiling.

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 22 | measured baseline / **kept** | **44** | 6a13b3d | strike 2,4,6,36; else dodge | K winner under L; Soft ceiling |
| 22b | discarded | 44 | 6a13b3d | strike 2,4,8,36; else dodge | equal |
| 22c | discarded | 44 | 6a13b3d | strike 2,4,10,36; else dodge | equal |
| 22d | discarded | 42 | 6a13b3d | wait T2; strike 4,6,36 | early wait; below |
| 22e | discarded | 42 | 6a13b3d | wait T2+T4; strike 6,8,36 | wait-bank Soft 42 under L |
| 22f | discarded | 39 | 6a13b3d | strike 2,4 then always strike from 30 | early overkill |
| 22g | discarded | 28 | 6a13b3d | strike 2,4,6,20 | early kill |
| 22h | discarded | 36 | 6a13b3d | always dodge | naive |
| 22i | probe | 44 | 6a13b3d | same as 22 under idle-only chip | Soft-identical (idle chip no-op under B) |

Arena L ceiling still **44** (8+36). No Soft proposal strictly beat gen 22. Wait-bank Soft 42 shows fatigue biting non-optimal rules.

## How a generation lands

The guide writes the next `rule`. Soft scores it. If score > kept, `lantern.aura` becomes that body and this table gets a kept row. Else the proposal is discarded here and `lantern.aura` stays on the winner.

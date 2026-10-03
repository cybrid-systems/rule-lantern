# Generations

Only rows with Soft tip + score come from a Soft `play` run. Do not fill a score by hand.

Soft tip: `6a13b3d` * image: `ghcr.io/cybrid-systems/dev:v1.0.9`

Prior Soft arenas A-J live in earlier main commits. K/L Soft 44 ceilings documented previously.

## Arena K: B strikes; max 36

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 21 | kept | 44 | 6a13b3d | strike 2,4,6,36; else dodge | Soft ceiling |

## Arena L: B strikes; max 36; wait fatigue from turn >=10

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 22 | kept | 44 | 6a13b3d | strike 2,4,6,36; else dodge | Soft ceiling; fatigue did not displace |

## Arena M: B strikes; max 36; opp deals 3 from turn >=12 (else 2)

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 22m | measured baseline / kept | 44 | 6a13b3d | strike 2,4,6,36; else dodge | Soft ceiling |
| 23a | discarded | 20 | 6a13b3d | strike 2,4,6,12 | early kill |
| 23b | discarded | 22 | 6a13b3d | strike 2,4,6,14 | early kill |
| 23c | discarded | 24 | 6a13b3d | strike 2,4,6,16 | early kill |
| 23d | discarded | 28 | 6a13b3d | strike 2,4,6,20 | early kill |
| 23e | discarded | 44 | 6a13b3d | strike 2,4,8,36 | equal |
| 23f | discarded | 44 | 6a13b3d | strike 2,4,11,36 | equal |
| 23g | discarded | 44 | 6a13b3d | strike 2,4,5,36 | equal |
| 23h | discarded | 43 | 6a13b3d | strike 2,4,12,36 | below |
| 23i | discarded | 42 | 6a13b3d | wait T2; strike 4,6,36 | below |
| 23j | discarded | 36 | 6a13b3d | always dodge | naive |

Arena M Soft ceiling **44**. Finish-on-last did **not** die under dmg3@12. Soft plateau (>=3 equals at 44).

## Arena M* (hardened): B strikes; max 36; opp deals 3 from turn >=6

Soft probe after M failed to Soft-displace finish-on-last.

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 24 | measured baseline | 43 | 6a13b3d | strike 2,4,6,36; else dodge | finish-last Soft dies on kill (lived 0) |
| 25 | **kept** | **44** | 6a13b3d | strike 2,4,5,36; else dodge | third strike before dmg3; Soft survive finish |
| 25b | discarded | 43 | 6a13b3d | strike 2,4,8,36 | equal to baseline |
| 25c | discarded | 43 | 6a13b3d | strike 2,4,11,36 | equal to baseline |
| 25d | discarded | 42 | 6a13b3d | wait T2; strike 4,6,36 | below |
| 25e | discarded | 42 | 6a13b3d | strike 2,4,6 only | leave ehp |
| 25f | discarded | 23 | 6a13b3d | strike 2,4,6,16 | early |
| 25g | discarded | 19 | 6a13b3d | strike 2,4,6,12 | early |
| 25h | discarded | 36 | 6a13b3d | always dodge | naive |

Finish-on-last Soft **died** as Soft 44 champion (dropped to Soft 43). Soft gen25 beats it.

## Arena N (current): M* damage; max **40** turns

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 26 | measured baseline | 44 | 6a13b3d | strike 2,4,5,36; else dodge | gen25 under longer match |
| 27 | **kept** | **48** | 6a13b3d | strike 2,4,5,40; else dodge | Soft ceiling 8+40 |
| 27b | discarded | 47 | 6a13b3d | strike 2,4,6,40 | finish-last Soft die-on-kill |
| 27c | discarded | 47 | 6a13b3d | strike 2,4,8,40 | equal die-on-kill |
| 27d | discarded | 46 | 6a13b3d | wait T2; strike 4,6,40 | below |
| 27e | discarded | 40 | 6a13b3d | always dodge | naive |
| 27f | discarded | 27 | 6a13b3d | strike 2,4,6,20 | early |

Arena N Soft ceiling **48**. Gen 27 hit it.

## How a generation lands

The guide writes the next `rule`. Soft scores it. If score > kept, `lantern.aura` becomes that body and this table gets a kept row.

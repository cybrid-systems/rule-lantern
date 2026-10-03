# Generations

Only rows with Soft tip + score come from a Soft `play` run. Do not fill a score by hand.

Soft tip: `6a13b3d` * image: `ghcr.io/cybrid-systems/dev:v1.0.9`

Prior Soft arenas through N (Soft 48 strike 2,4,5,40) live in earlier main commits.

## Arena O: N damage; max **48**; no dodge chip

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 27o | measured baseline | 48 | 6a13b3d | strike 2,4,5,40; else dodge | gen27 under longer match |
| 28 | **kept** | **56** | 6a13b3d | strike 2,4,5,48; else dodge | Soft ceiling 8+48 |
| 28b | discarded | 55 | 6a13b3d | strike 2,4,6,48 | finish-last Soft die-on-kill |
| 28c | discarded | 54 | 6a13b3d | strike 2,4,5,46 | below |
| 28d | discarded | 54 | 6a13b3d | wait T2; strike 4,5,48 | below |
| 28e | discarded | 48 | 6a13b3d | always dodge | naive |

Arena O Soft ceiling **56**. Gen 28 hit it. Soft plateau not reached before raising to P.

## Arena P (current): O physics + dodge chip 1 from turn >=20

| gen | status | score | Soft tip | rule | note |
|-----|--------|-------|----------|------|------|
| 29 | measured baseline | 28 | 6a13b3d | strike 2,4,5,48; else dodge | O winner Soft-collapsed under chip |
| 29b | discarded | 27 | 6a13b3d | strike 2,4,5,19 | kill before chip |
| 29c | discarded | 29 | 6a13b3d | strike 2,4,5,22 | below kept |
| 30 | **kept** | **30** | 6a13b3d | strike 2,4,5,23; else dodge | Soft-best under chip@20 |
| 30b | discarded | 28 | 6a13b3d | strike 2,4,5,24 | below |
| 30c | discarded | 28 | 6a13b3d | strike 2,4,5,21 | below |
| 30d | discarded | 29 | 6a13b3d | wait T2; strike 4,5,24 | equal not higher than 30 after keep |
| 30e | discarded | 28 | 6a13b3d | wait T2; strike 4,5,23 | below |
| 30f | discarded | 26 | 6a13b3d | strike 2,4,6,19 | below |
| 30g | discarded | 24 | 6a13b3d | always dodge | Soft dies to chip |
| 30h | discarded | 28 | 6a13b3d | strike 2,4,5,20 | below |
| 30i | discarded | 28 | 6a13b3d | strike 2,4,5,25/26 | below |

Soft 2,4,5,last **did not survive** P (Soft 28). Soft plateau near Soft 30 with nearby kill timings Soft 28-29.

### Soft soften probe (chip from turn >=30, not adopted)

| tag | score | Soft tip | note |
|-----|-------|----------|------|
| PS_BASE | 38 | 6a13b3d | finish-last Soft better but still Soft < O |
| PS1 | 37 | 6a13b3d | kill @29 |
| PS2 | 39 | 6a13b3d | kill @32 Soft-best soften probe |
| PS3 | 40 | 6a13b3d | kill @33 Soft-best soften probe |

Chip@20 Soft scores stayed Soft >=24 (not Soft <20), so Soft kept chip@20.

## How a generation lands

Soft scores a proposal. Keep only if Soft score is strictly higher.

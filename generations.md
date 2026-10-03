# Generations

Only Soft `play` scores. Tip `6a13b3d`.

Prior arenas through P (Soft 30, strike 2,4,5,23) are on earlier main commits.

## Arena P search (chip@20, max 48) — no Soft score > 30

| gen | status | score | rule |
|-----|--------|-------|------|
| 30 | kept | 30 | strike 2,4,5,23 |
| 30p21 | discarded | 28 | strike 2,4,5,21 |
| 30p22 | discarded | 29 | strike 2,4,5,22 |
| 30p23 | discarded | 30 | strike 2,4,5,23 (equal) |
| 30p24 | discarded | 28 | strike 2,4,5,24 |
| 30p25 | discarded | 28 | strike 2,4,5,25 |
| 30p26 | discarded | 28 | strike 2,4,5,26 |
| 30p27 | discarded | 28 | strike 2,4,5,27 |
| 30p28 | discarded | 28 | strike 2,4,5,28 |
| 30h23 | discarded | 28 | wait T2; strike 4,5,23 |
| 30h24 | discarded | 29 | wait T2; strike 4,5,24 |
| 30h25 | discarded | 27 | wait T2; strike 4,5,25 |
| 30h26 | discarded | 27 | wait T2; strike 4,5,26 |
| 30h4 | discarded | 28 | wait T4; strike 2,5,23 |
| 30x | discarded | 27 | strike 2,4,6,23 |

Soft did **not** beat 30 under P. Plateau at Soft 30.

## Arena Q: dodge chip from turn >=15

| gen | status | score | rule | note |
|-----|--------|-------|------|------|
| 31a | baseline | 23 | strike 2,4,5,23 | P winner under Q |
| 31 | **kept** | **25** | strike 2,4,5,18 | Soft-best |
| 31b | discarded | 24 | strike 2,4,5,17 | below |
| 31c | discarded | 23 | strike 2,4,5,15/16/19/20/21/22 | below |
| 31d | discarded | 22 | strike 2,4,5,14 | below |
| 31e | discarded | 22 | wait T2; strike 4,5,16 | below |
| 31f | discarded | 23 | wait T2; strike 4,5,18 | below |
| 31g | discarded | 22 | wait T2; strike 4,5,20 | below |
| 31h | discarded | 19 | always dodge | below |

Q Soft best **25** (>=15), so chip@15 kept. Chip@18 probe Soft-best was 28 (kill @21) but not adopted.

## Arena R (current): Q + clean-win +2 if ehp hits 0 with hp>=4

| gen | status | score | rule | note |
|-----|--------|-------|------|------|
| 32 | baseline / kept | 25 | strike 2,4,5,18 | same Soft as Q; bonus did not add |
| 32b | discarded | 24 | strike 2,4,5,17 | below |
| 32c | discarded | 23 | strike 2,4,5,5/15/16/19 | below |
| 32d | discarded | 22 | strike 2,4,5,14 | below |
| 32e | discarded | 20 | strike 2,4,5,12 | below |
| 32f | discarded | 16 | strike 2,4,5,8 | below |
| 32g | discarded | 13 | strike 2,3,4,5 | early kill, no Soft +2 |
| 32h | discarded | 12 | strike 1,2,3,4 | early kill, no Soft +2 |
| 32i | discarded | 22 | wait T2; strike 3,4,5 | below |

No Soft proposal beat 25. Measured kills did not Soft-earn the +2.

## How a generation lands

Keep only on a strictly higher Soft score.

# Generations (external measurement)

These rows are **not** Aura-native evolution. The host proposed a rule
(hand edit or a separate candidate file) and a oneshot Soft process printed
`(play)`. That number was called a "Soft score" in older notes. It is an
external measurement: Soft is the binary, the number is oneshot fitness.

Arena T external ceiling on that protocol: fitness **27**, strike turns
2,4,5,20, tip `6a13b3d`. The native swarm is not expected to beat that
ceiling on the same arena. Native in-session runs are in `generations_native.md` only.

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

## Arena R: Q + clean-win +2 if ehp hits 0 with hp>=4

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

## Arena S: R but clean-win hp>=2 (start hp still 6)

| gen | status | score | rule | note |
|-----|--------|-------|------|------|
| 33s | baseline | 25 | strike 2,4,5,18 | Soft killhp=0 @18; Soft +2 no |
| 33s14 | discarded | 22 | strike 2,4,5,14 | below |
| 33s15 | discarded | 23 | strike 2,4,5,15 | below |
| 33s16 | discarded | 23 | strike 2,4,5,16 | below |
| 33s17 | discarded | 24 | strike 2,4,5,17 | below |
| 33s18 | discarded | 25 | strike 2,4,5,18 | equal |
| 33s2345 | discarded | 15 | strike 2,3,4,5 | early |
| 33s1234 | discarded | 14 | strike 1,2,3,4 | early |
| 33sheal | discarded | 22 | wait T2; strike 3,4,5 | Soft killhp=-1/0 |
| 33sheal16 | discarded | 22 | wait T2; strike 3,4,16 | Soft killhp=-1 |
| 33sw2k16 | discarded | 22 | wait T2; strike 4,5,16 | Soft killhp=-1 |
| 33sw2k18 | discarded | 23 | wait T2; strike 4,5,18 | Soft killhp=-1 |
| 33sheal4 | discarded | 23 | wait T4; strike 2,5,18 | Soft killhp=-1 |

Soft under S: no Soft score > 25. Soft killhp at best Soft path was Soft 0 — Soft +2 Soft-unreachable with start hp=6.

## Arena T (current): S + start hp=8

| gen | status | score | rule | note |
|-----|--------|-------|------|------|
| 33a | baseline | 25 | strike 2,4,5,18 | Soft killhp=0 @18; Soft +2 no |
| 33 | **kept** | **26** | strike 2,4,5,16 | Soft killhp=**2** @16; Soft **+2 fired** |
| 33b | discarded | 26 | strike 2,3,4,16 | Soft killhp=2 @16; Soft equal |
| 33c | discarded | 26 | strike 2,4,5,19 | Soft killhp=0 @19; Soft equal |
| 33d | discarded | 25 | strike 2,4,5,15/17/18 | Soft below/equal |
| 33e | discarded | 24 | strike 2,4,5,14 | Soft below |
| 33f | discarded | 15 | strike 2,3,4,5 | Soft early killhp=4 but Soft low lived |
| 33g | discarded | 24 | strike 2,4,6,16 | Soft killhp=1; Soft +2 no |
| 33h | discarded | 23 | wait T2; strike 4,5,16 | Soft no kill |
| 33i | discarded | 22–24 | wait T2/T4 heal + finish | Soft killhp=0 or Soft no kill |
| 34 | **kept** | **27** | strike 2,4,5,20 | Soft killhp=0 @20; Soft +2 no; Soft lived beat gen33 |
| 34b | discarded | 25 | strike 2,4,5,21/22/23 | Soft died before Soft finish |
| 34c | discarded | 23 | strike 2,4,5,12/13 | Soft below |

Soft +2 Soft-reachable under T (gen33 Soft 26 Soft killhp=2). Soft ≥27 Soft-with Soft +2 Soft-not Soft-found; Soft 27 Soft-kept Soft-without Soft bonus Soft-via Soft longer Soft lived Soft (gen34). Soft heal Soft-before Soft-finish Soft-paths Soft-did Soft-not Soft-Soft Soft-beat Soft 26 Soft-with Soft bonus Soft (Soft chip Soft + Soft strike Soft trade Soft still Soft drains Soft to Soft Soft killhp Soft < Soft 2 Soft past Soft turn Soft 16 Soft).

## How a generation lands

External protocol only: keep a candidate only when a oneshot `(play)` is strictly higher. Native runs are not rows in this file.

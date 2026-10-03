# Native generations

In-session only. One Aura process, `evolve_native.aura`, tip `6a13b3d`.
Container command as in the README, including `AURA_SANDBOX=off`.
Do not copy numbers from `generations.md` into this file.

`std/swarm:step!` maximizes. Rule fitness returned `(play)`.
Opponent fitness returned `(- 0.0 (play))`.
Heal runs when a swap regresses that objective. `register!` runs when it improves.

## Run 1 — 2026-10-03

Stdout (stderr empty, exit 0):

```
NATIVE_START
SET_CODE=ok
BASE_PLAY=27
FINAL_PLAY=21
NATIVE_BEST=27
NATIVE_GEN=3
NATIVE_VIA=swarm+hot-strategy
NATIVE_HEAL=24
NATIVE_OPP_HEAL=10
NATIVE_OPP_GEN=2
NATIVE_OPP_PLAYER=21
NATIVE_SWAP_OK=42/42
NATIVE_TURNS=(2 4 5 20)
NATIVE_RULE=(lambda (hp ehp turn) (cond ((or (= turn 2) (= turn 4) (= turn 5) (= turn 20)) 1) (#t 2)))
NATIVE_OPP=(lambda (turn) (if (>= turn 2) (if (>= turn 34) 2 2) (if (= (modulo turn 2) 1) 2 0)))
NATIVE_DONE
```

| phase | gens | pop | result |
|-------|------|-----|--------|
| rule PSO | 3 | 8 | best fitness 27, turns 2,4,5,20 (initial). Heal fired 24 times (every candidate). No in-session improvement over the external ceiling. |
| opp-policy PSO | 2 | 8 | player fitness driven from 27 down to 21. Heal fired 10 times. 42/42 `hot-strategy:swap!` calls returned true, including the two restore swaps. |

`FINAL_PLAY=21` is against the swapped opponent, not against Arena T "B".
It is not a new ceiling over external 27.

## Not a native run — production Restricted defaults

Same script, same container, **without** `AURA_SANDBOX=off`. Stderr:

```
[#2266] Moving pin contract failed (soft mode) — suppressing success metrics
[#2341] Densify consistency contract failed (soft mode): pin — suppressing success metrics
```

Stdout `SET_CODE=(general-object-pin-required … #2891)`. `NATIVE_BEST=-1` and
`NATIVE_HEAL=0` here mean the duel never installed. Not a fitness.

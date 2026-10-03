# Native generations

In-session only. One Aura process per run, tip `6a13b3d`.
Do not copy numbers from `generations.md` into this file.

`std/swarm:step!` maximizes. Rule fitness returned `(play)`.
Opponent fitness returned `(- 0.0 (play))`.
`hot-strategy` is a single active name. Each phase `register!`s that
name, then `swap!` / `heal!`. Heal failure count is separate from heal calls.

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

Container had `AURA_SANDBOX=off`. This run did not use fiber, ant, grid, abc, colony, or synthesize.

## Run 2 — 2026-10-03 13:47 PT

Same container as the README, including `AURA_SANDBOX=off`. Script `evolve_native.aura`. Exit 0. Stderr empty.

Stdout:

```
NATIVE_START
SET_CODE=ok
BASE_PLAY=27
NATIVE_AGENT=((:ok . #t) (:decision . commit) (:eval-ok . #t) (:metrics . <hash[8]>))
AGENT_PLAY=27
FIBER_SPAWN=41
FIBER_SIM_BASE=27
FIBER_PLAY_BASE=27
FIBER_POP_BEST=(33.809 32.0506 2.79522 20.2351)
FIBER_CONFIRM_PLAY=23
FIBER_CONFIRM_SIM=23
NATIVE_OPP_INSEARCH=10
NATIVE_RULE_BEST=27
NATIVE_RULE_TURNS=(2 4 5 20)
NATIVE_RULE=(lambda (hp ehp turn) (cond ((or (= turn 2) (= turn 4) (= turn 5) (= turn 20)) 1) (#t 2)))
NATIVE_VS_OPP=10
NATIVE_OPP_BEST=10
NATIVE_OPP=(lambda (turn) (if (>= turn 2) (if (>= turn 11) 3 3) (if (= (modulo turn 2) 1) 3 0)))
NATIVE_OPP_AT_RULE=(lambda (hp ehp turn) (cond ((or (= turn 2) (= turn 4) (= turn 5) (= turn 20)) 1) (#t 2)))
NATIVE_HEAL=18
NATIVE_OPP_HEAL=25
NATIVE_HEAL_FAIL=0
NATIVE_SWAP_OK=74/74
NATIVE_RULE_STEPS=5
NATIVE_OPP_STEPS=5
NATIVE_KINDS=rule:pso opp:pso rule:pso opp:pso rule:ant opp:ant rule:grid opp:grid rule:abc opp:abc 
NATIVE_KIND_FAIL=
NATIVE_FIBER=1
NATIVE_FIBER_NOTE=swarm-parallel-pso
NATIVE_ORCH=7
NATIVE_COLONY=(#f  colony:1var)
SET_CODE=ok
NATIVE_SYNTH=(1 . 30)
SET_CODE=ok
NATIVE_PLAY_RESTORED=10
NATIVE_DONE
```

Alternate rounds: each round is one `swarm:step!` on `rule` then one on `opp-policy`. `std/swarm` is one population, so each side `swarm:init`s again. Two PSO rounds, then ant, grid, abc. Pop 4.

| stamp | value | meaning |
|-------|-------|---------|
| NATIVE_RULE_BEST | 27 | remeasured `(play)` of the kept rule against Arena T opponent B |
| NATIVE_OPP_BEST | 10 | remeasured `(play)` of that same rule against the kept opp |
| NATIVE_OPP_INSEARCH | 10 | minimum seen while searching; matches the remeasure |
| NATIVE_PLAY_RESTORED | 10 | after colony/synth, duel file reinstalled and the kept pair swapped back. This is vs the kept opp, not vs B |
| NATIVE_FIBER | 1 | `fiber:spawn`/`join` returned 41, then `swarm` PSO `parallel` `#t` evaluated a pop of 4 |
| FIBER_CONFIRM_PLAY | 23 | that pop's best, installed and `(play)`'d. Sim matched. Not above 27, healed away |
| NATIVE_KINDS | pso, ant, grid, abc | both sides, `NATIVE_KIND_FAIL` empty |
| NATIVE_SWAP_OK | 74/74 | `hot-strategy:swap!` car was `#t` |
| NATIVE_HEAL / OPP_HEAL | 18 / 25 | `heal!` calls. `NATIVE_HEAL_FAIL=0` |
| NATIVE_AGENT | commit | `agent:closed-loop-once` on `clamp`. `(play)` stayed 27 |
| NATIVE_ORCH | 7 | `orch:step` of an echo role |
| NATIVE_COLONY | `(#f  colony:1var)` | `colony:search` ran and did not find the expected output. Fail |
| NATIVE_SYNTH | `(1 . 30)` | `synthesize:optimize` returned that pair. Not kept. See the disqualification probe |

Kept rule, in words: strike on turns 2, 4, 5, and 20, otherwise dodge. Same body as the initial rule. Against opponent B this is in-session 27. It does not beat 27.

Kept opponent, in words: damage 3 on every turn. The body is `turn >= 2` then `turn >= 11` then 3 else 3, else odd turns 3 else 0. Turn 1 is odd, so it is also 3, and from turn 2 both arms are 3. Fingerprint is the `NATIVE_OPP` line. Player score 10 is against this opponent, not against B.

Fiber note: the parallel fitness is a pure turn-list simulator (no `mutate`). It matched `(play)` at 27 before the pop step. Concurrent rebind is not what the fiber path does.

## Restricted probe — 2026-10-03 13:15 PT

`evolve_restricted.aura`. Same image and `AURA_PIPELINE_STRICT=0`. No `AURA_SANDBOX=off`. No `set-code`. The duel is defined in the file. Exit 0. Stderr empty.

```
RESTRICTED_START
NATIVE_SET_CODE=skipped
BASE_PLAY=27
SNAP=-1
SNAP_REASON=:no-workspace
REBIND=(fail capability denied: sandboxed primitive requires kCapSandbox)
HS_SWAP=(#t 1 -1)
RULE_AT_16=2
PLAY_AFTER=27
NATIVE_RESTRICTED_OK=0
NATIVE_RESTRICTED_REASON=no-workspace snap -1; user mutate:rebind capability-denied; hot-strategy:swap! returned #t but live rule at turn 16 stayed 2 and play stayed 27
RESTRICTED_DONE
```

`hot-strategy:swap!` returned `(#t 1 -1)` and the running rule did not change (turn 16 stayed dodge `2`, `(play)` stayed 27). That is not a green restricted path. `NATIVE_RESTRICTED_OK=0`. The evolving run still needs `AURA_SANDBOX=off` so `set-code` can build a workspace.

## Synthesize disqualification — 2026-10-03 13:39 PT

Separate process, same tip and `AURA_SANDBOX=off`, same `synthesize:optimize` arguments as run 2 (`"rule"`, population 2, generations 1, mutation-rate 0.2, fitness `"(play)"`). Stderr empty. Exit 0.

```
BEFORE=27
SYNTH=(1 . 30)
AFTER_PLAY=30
```

`AFTER_SRC` is not Arena T. Two edits versus `lantern.aura`:

- `rule` strike turn 2 became 9
- `step` binds `taken` with `(/ base-taken chip)` instead of `(+ base-taken chip)`

`AFTER_PLAY=30` is that rewritten physics, not opponent B under Arena T. Run 2 restores the duel file after the probe (`SET_CODE=ok`, `NATIVE_PLAY_RESTORED=10`). The 30 is not a kept score and is not a claim over 27.

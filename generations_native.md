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

## Run 3 — arms race — 2026-10-03 14:04 UTC+8

`evolve_arms.aura`. Same image, `AURA_PATH`, `AURA_PIPELINE_STRICT=0`, `AURA_SANDBOX=off`. One process. Exit 0. Stderr empty. Four rounds, no plateau (a round stops only when both the rule score and the opp score stay equal to that round's incoming `(play)`). Rule side is PSO pop 4, one step. Opp side is ant pop 4, one step. Fitness rebind is serial (`swarm:parallel! #f`).

Stdout:

```
SET_CODE=ok
FIBER_SPAWN=41
BASE_VS_B=27
ROUND 1 IN=27 RULE=27 OPP=21 TURNS=(2 4 5 20)
ROUND 2 IN=21 RULE=24 OPP=20 TURNS=(9 25 19 17)
ROUND 3 IN=20 RULE=23 OPP=22 TURNS=(4 15 29 27)
ROUND 4 IN=22 RULE=23 OPP=22 TURNS=(39 19 17 21)
ROUNDS=4
PLATEAU=0
HEALS=13
OPP_HEALS=4
HEAL_FAIL=0
SWAPS=48
SWAP_OK=48
NATIVE_FIBER=1
SET_CODE=ok
PLAY_BEST_RULE_VS_B=27
BEST_RULE_TURNS=(2 4 5 20)
PLAY_FINAL_RULE_VS_B=22
FINAL_RULE_TURNS=(39 19 17 21)
PLAY_FINAL_VS_KEPT_OPP=22
KEPT_OPP=(lambda (turn) (if (>= turn 5) (if (>= turn 6) 3 2) (if (= (modulo turn 2) 1) 2 0)))
NATIVE_OPP_BEST=22
NATIVE_RULE_VS_B=22
NATIVE_RULE_BEST=see PLAY_BEST_RULE_VS_B
```

`NATIVE_FIBER=1` here is only `fiber:join` of `fiber:spawn` returning 41. This run did not evaluate the swarm population on fibers. Run 2 is still the parallel-pop measurement.

`PLAY_BEST_RULE_VS_B=27` is the remeasure, against opponent B, of the best rule found while the installed opponent was still B. Turns 2, 4, 5, 20. Not above 27.

`PLAY_FINAL_RULE_VS_B=22` is the round-4 rule (strike on 39, 19, 17, 21, else dodge) remeasured against B. Worse than 27. Not a new ceiling.

`NATIVE_OPP_BEST=22` is `(play)` of that final rule against the kept opponent, remeasured after a fresh `set-code`. The kept opponent body is the Arena T B schedule again: strike on odd turns and on every turn from 5, damage 2 before turn 6 else 3. Round 2's opp score of 20 was during search and is not this kept body.

Round scores are player `(play)` after that side's step, against the opponent installed at that moment. Round 2's rule score 24 is not a score against B.

## Restricted again — argv file is not a workspace — 2026-10-03 13:59 UTC+8

`evolve_restricted.aura`. No `AURA_SANDBOX` (the image default is empty; the script printed `SANDBOX=()`). No `set-code`. Exit 0. Stderr empty.

```
SANDBOX=()
ARGV_CURRENT_LEN=1314
ARGV_WS_LEN=0
SNAP_BEFORE=-1
LOAD=effect-denied: mutate not granted tenant=0 op=load
SNAP_AFTER_LOAD=-1
PLAY_BEFORE=26:24: unbound variable: play
  did you mean 'last'?
REBIND=capability denied: sandboxed primitive requires kCapSandbox
NATIVE_RESTRICTED_OK=0
RESTRICTED_STOP=mutate-denied-or-not-true
```

`./aura evolve_restricted.aura` evaluates the file (`ARGV_CURRENT_LEN=1314`) and does not install a workspace (`ARGV_WS_LEN=0`, `ast:snapshot` -1). `(load "/workspace/rule-lantern/lantern.aura")` is the other workspace install and is effect-denied (`mutate not granted`). `mutate:rebind` is denied (`kCapSandbox`). Branch stopped. `NATIVE_RESTRICTED_OK=0`.

## Surfaces — 2026-10-03 14:00–14:06 UTC+8

No `LLM_API_KEY` was passed into the container.

`probe_llm_hot.aura`. Exit 0. Stderr empty.

```
LLM_KEY_LEN=0
LLM_CALL=
LLM_CHAT_OK=#f
LLM_CHAT_ERR=empty
LLM_CHAT_CONTENT=
LLM_STAT_NO_KEY=0
LLM_STAT_FAIL=0
HOT_CFG=(1 2 #f)
HOT_VER=78:4: unbound variable: aot:get-module-version
HOT_HEALTH=24:24: unbound variable: hot-update:health
HOT_REGION=87:4: unbound variable: aot:get-region-mask
```

`aura-llm-call` returned an empty string. `llm:chat` returned ok `#f`, error `empty`. The `no-key` stat stayed 0 (the module returns "" without bumping it). `hot-update:make-config` returned `(1 2 #f)`. The AOT primitives behind version/region are unbound in this binary, and `hot-update:health` itself is unbound (the module does not finish that define).

`probe_surfaces.aura` refactor call, before the process died. Stderr was repeated `error: invalid closure: eval_flat: invalid closure`. Exit 1. The later lantern/`synthesize` half of that file did not run; the guard below is a separate process.

```
REFACTOR_SET=#t
REFACTOR=1
REFACTOR_SRC=(define scratch-name (lambda (x) x))
```

`refactor:rename-var` of `scratch-name` to `scratch-renamed` returned 1. Workspace source still says `scratch-name`. The rename did not land.

`probe_synth_guard.aura`. Exit 0. Stderr empty. `query:node-type` `"Define"` returned numeric ids before and after (`QUERY_*_IDS=1`).

```
PLAY_BEFORE=27
SYNTH_RETURN=(0 . 1000.0006958942241)
SRC0_LEN=1436
SRC1_LEN=1436
SRC_EQ=1
FLAGS_HOLD=1
PLAY_AFTER=54
NATIVE_SYNTH=reject
```

Needles still present on both sides: `taken (+ base-taken chip)`, `opp-damage turn`, `step hp ehp turn action`, and the clean-win bonus form. Source text compared equal, and `(play)` still moved from 27 to 54. That is not Arena T on an unchanged body, so the guard rejects it. The 54 is not a score against B and is not a claim over 27. The return pair is not a `(play)` result.

## Run 4 — RSI — 2026-10-03 15:23 CST

`evolve_rsi.aura`. Same image `ghcr.io/cybrid-systems/dev:v1.0.9`, `AURA_PATH`, `AURA_PIPELINE_STRICT=0`, `AURA_SANDBOX=off`. Soft tip `6a13b3d`. One process. Exit 0. Stderr empty.

Closed loop in that process: `set-code` of `lantern.aura`, then four arms-race rounds. Odd rounds are PSO on `rule` (strike-turn genes) then ant on `opp-policy`. Even rounds are ant on `rule` then PSO on `opp-policy`. Pop 4, one `swarm:step!` per side. Fitness rebind is serial. A fiber PSO (`"parallel" #t`) runs first, on a pure turn-list sim, and is confirmed with `(play)` only after the step gate.

After every `hot-strategy:swap!` the workspace source must still contain `dodge-chip`, `base-taken`, and `(taken (+ base-taken chip))`, and must not contain `(taken (/ base-taken chip))`. A miss calls `heal!` and stamps `RSI_GATE_REJECT`.

Before search, the script swaps `step` to `(lambda (hp ehp turn action) (cons hp ehp))`. That body installed and the pattern was gone (`RSI_GATE_PROBE=broken-step`). `RSI_GATE_REJECT=step`, `heal!` restored Arena T, and `(play)` was 27 again. That is the only gate reject. The swarm rounds did not print another `RSI_GATE_REJECT`.

Stdout:

```
RSI_START
SET_CODE=ok
BASE_VS_B=27
RSI_GATE_AT_INSTALL=1
RSI_GATE_PROBE=broken-step
RSI_GATE_REJECT=step
RSI_GATE_PROBE_PLAY=27
FIBER_SPAWN=41
FIBER_SIM_BASE=27
FIBER_PLAY_BASE=27
FIBER_POP_BEST=(32.9804 30.0977 20.0693 4.06575)
FIBER_CONFIRM_PLAY=23
FIBER_CONFIRM_SIM=23
RSI_KIND=rule:pso:pso
RSI_KIND=opp:ant:ant
ROUND 1 IN=27 RULE=27 OPP=21 TURNS=(2 4 5 20)
RSI_KIND=rule:ant:ant
RSI_KIND=opp:pso:pso
ROUND 2 IN=21 RULE=23 OPP=23 TURNS=(2 3 4 16)
RSI_KIND=rule:pso:pso
RSI_KIND=opp:ant:ant
ROUND 3 IN=23 RULE=23 OPP=23 TURNS=(2 3 4 16)
RSI_KIND=rule:ant:ant
RSI_KIND=opp:pso:pso
ROUND 4 IN=23 RULE=23 OPP=23 TURNS=(2 3 4 16)
SET_CODE=ok
RSI_VS_B=27
RSI_BEST=27
RSI_BEST_TURNS=(2 4 5 20)
RSI_BEST_RULE=(lambda (hp ehp turn) (cond ((or (= turn 2) (= turn 4) (= turn 5) (= turn 20)) 1) (#t 2)))
RSI_ROUNDS=4
RSI_HEAL=26
RSI_HEAL_FAIL=0
RSI_GATE_REJECTS=1
RSI_SWAP_OK=44/45
RSI_FIBER=1
RSI_FIBER_NOTE=swarm-parallel-pso
RSI_FINAL_TURNS=(2 3 4 16)
RSI_FINAL_RULE=(lambda (hp ehp turn) (cond ((or (= turn 2) (= turn 3) (= turn 4) (= turn 16)) 1) (#t 2)))
RSI_KEPT_OPP=(lambda (turn) (if (>= turn 2) (if (>= turn 34) 2 2) (if (= (modulo turn 2) 1) 2 0)))
SET_CODE=ok
RSI_FINAL_VS_KEPT=23
RSI_FINAL_RULE_VS_B=26
RSI_DONE
```

| stamp | value | meaning |
|-------|-------|---------|
| RSI_BEST | 27 | best `(play)` seen while the kept opponent was still Arena T B, and the step gate held |
| RSI_VS_B | 27 | fresh `set-code`, that same rule swapped back, opponent B, gate held. Not above 27 |
| RSI_FINAL_RULE_VS_B | 26 | round-4 rule (strike 2, 3, 4, 16, else dodge) remeasured against B. Worse than 27 |
| RSI_FINAL_VS_KEPT | 23 | that final rule against the kept opponent. Not a score against B |
| RSI_ROUNDS | 4 | four alternate rounds, no early stop |
| RSI_HEAL | 26 | `heal!` calls, including the gate restore and worse candidates. `RSI_HEAL_FAIL=0` |
| RSI_GATE_REJECTS | 1 | the deliberate broken `step` probe. Swarm swaps did not fail the gate |
| RSI_FIBER | 1 | spawn/join returned 41, parallel PSO pop of 4. Confirm `(play)` 23 matched the sim and was healed |
| RSI_SWAP_OK | 44/45 | the 45th swap is the probe, which installed and was rejected |

Soft did not beat 27 against opponent B. `RSI_VS_B` is 27. `RSI_FINAL_RULE_VS_B` is 26. Round 2's rule score 23 is against the opponent installed after round 1, not against B.

Kept best-vs-B rule: strike on turns 2, 4, 5, and 20, otherwise dodge. Same body as the initial rule.

Kept opponent, in words: damage 2 on every turn. From turn 2 both arms are 2, and turn 1 is the odd-turn arm, also 2. Fingerprint is the `RSI_KEPT_OPP` line. Player score 23 is against this opponent.

## Run 5 — RSI2 kernel-knob evolve — 2026-10-03 16:22 UTC+8

`evolve_rsi2.aura`. This is the interrupted Soft run. The complete available stdout is `/tmp/rsi2.out`; it stops after the live knob stamps and has no `RSI2_DONE` line. Do not interpret the missing close-out stamps as zero.

This process first verified the Arena T baseline (`BASE_ARENA_T=27`), installed the behavior-matched factored duel, and exercised the gate with a deliberately broken `step`. The search then alternated four rounds over rule, the three early kernel knobs, opponent policy, and the late-strike knob. The factored `step` still contains `dodge-chip`, `base-taken`, and `(taken (+ base-taken chip))`; the knobs are live helper swaps, so changing them changes the duel physics.

Available stdout:

```
RSI2_START
SET_CODE=ok
BASE_ARENA_T=27
SET_CODE_FACTOR=ok
BASE_FACTOR=27
RSI2_GATE_AT_INSTALL=1
RSI2_GATE_PROBE=broken-step
RSI2_GATE_REJECT=step
RSI2_GATE_PROBE_PLAY=27
FIBER_SPAWN=41
FIBER_SIM_BASE=27
FIBER_PLAY_BASE=27
FIBER_POP_BEST=(20.3961 1.57291 0.677492)
FIBER_CONFIRM_PLAY=29
FIBER_CONFIRM_SIM=29
RSI2_KIND=rule:pso:pso
RSI2_KIND=knob:ant:ant
RSI2_KIND=opp:ant:ant
RSI2_KIND=late:ant:ant
ROUND 1 IN=27 RULE=27 KNOB=30 OPP=10 TURNS=(2 4 5 20) KNOBS=(20 1 2 5)
RSI2_KIND=rule:ant:ant
RSI2_KIND=knob:pso:pso
RSI2_KIND=opp:pso:pso
RSI2_KIND=late:pso:pso
ROUND 2 IN=10 RULE=10 KNOB=10 OPP=10 TURNS=(2 4 5 20) KNOBS=(20 1 2 5)
RSI2_KIND=rule:pso:pso
RSI2_KIND=knob:ant:ant
RSI2_KIND=opp:ant:ant
RSI2_KIND=late:ant:ant
ROUND 3 IN=10 RULE=25 KNOB=36 OPP=36 TURNS=(35 31 29 7) KNOBS=(48 2 2 5)
RSI2_KIND=rule:ant:ant
RSI2_KIND=knob:pso:pso
RSI2_KIND=opp:pso:pso
RSI2_KIND=late:pso:pso
ROUND 4 IN=36 RULE=36 KNOB=36 OPP=36 TURNS=(35 31 29 7) KNOBS=(48 2 2 5)
SET_CODE_FACTOR=ok
RSI2_VS_B=30
RSI2_BEST=30
RSI2_BEST_TURNS=(2 4 5 20)
RSI2_BEST_RULE=(lambda (hp ehp turn) (cond ((or (= turn 2) (= turn 4) (= turn 5) (= turn 20)) 1) (#t 2)))
RSI2_KNOBS=chip=20,hp=1,bonus=2,late=5
RSI2_KNOB_LIVE_CHIP=20
RSI2_KNOB_LIVE_HP=1
RSI2_KNOB_LIVE_BONUS=2
RSI2_KNOB_LIVE_LATE=5
```

The fiber confirmation was **kept, not healed**: `fiber-pop!` only keeps it when the gate passes, the simulator equals `(play)`, it beats the prior `*vsb*`, and `vs-b?` holds. Here both values were 29, above the verified 27 baseline, and the subsequent best-vs-B knob stamp is `chip=20,hp=1,bonus=2,late=5`. The printed Round 3 `KNOB=36` is a score after live knob evolution, not an Arena T score; its live knobs were `(48 2 2 5)`, while the later best-vs-B stamp is `(20 1 2 5)`.

`RSI2_VS_B=30` is above the prior 27, but it is **not** the frozen Arena T physics: the kernel knobs changed the duel. It is the Soft factored-duel result under the gate, with best rule strikes on turns 2, 4, 5, and 20. The final `RSI2_HEAL`, `RSI2_HEAL_FAIL`, `RSI2_ROUNDS`, `RSI2_GATE_REJECTS`, and other close-out lines were not printed. One `RSI2_GATE_REJECT=step` event is visible; heal totals and the final counter stamps remain unknown from this interrupted stdout.

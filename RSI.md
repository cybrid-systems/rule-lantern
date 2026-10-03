# RSI

Same Soft process, closed loop. Not a host edit of a rule file.

`evolve_rsi.aura` on Soft tip `6a13b3d`, image `ghcr.io/cybrid-systems/dev:v1.0.9`, `AURA_SANDBOX=off`.

1. `set-code` of `lantern.aura` (Arena T), then `eval-current`.
2. Propose a body, `hot-strategy:swap!` it into the live AST, `(play)`, keep (`register!`) or `heal!`.
3. Alternate arms-race rounds, N=4. Odd: PSO on `rule` strike-turn genes, ant on `opp-policy`. Even: ant on `rule`, PSO on `opp-policy`. Pop 4.
4. After every swap, the workspace `step` body must still contain `dodge-chip`, `base-taken`, and `(taken (+ base-taken chip))`. If not, `heal!` and `RSI_GATE_REJECT`.
5. Fiber: `fiber:spawn` / `fiber:join`, then one PSO step with `"parallel" #t` on a pure simulator (no rebind). The confirm swap goes through the same gate.

`std/rule` is not loaded. The duel function is named `rule`.

## Run — 2026-10-03 15:23 CST

Exit 0. Stderr empty. Stdout is copied in `generations_native.md` (run 4). Stamps from that process:

| stamp | value |
|-------|-------|
| RSI_BEST | 27 |
| RSI_VS_B | 27 |
| RSI_ROUNDS | 4 |
| RSI_HEAL | 26 |
| RSI_GATE_REJECTS | 1 |
| RSI_DONE | printed |

`RSI_GATE_REJECTS=1` is the probe that swapped `step` to a lambda which returns `(cons hp ehp)`. The pattern was gone, `heal!` restored it, and `(play)` was 27 before search. The four swarm rounds did not fail the gate.

Soft did not beat 27 against opponent B. The remeasure of the best rule (strike turns 2, 4, 5, 20) against B is 27. The final co-evolved rule (strike turns 2, 3, 4, 16) scores 26 against B and 23 against the kept opponent. 23 and 26 are not scores that beat B.

```bash
sudo docker run --rm \
  -v /workspace/aura-grok:/workspace/aura-grok \
  -v /workspace/rule-lantern:/workspace/rule-lantern \
  -w /workspace/rule-lantern \
  -e AURA_PATH=/workspace/aura-grok/lib \
  -e AURA_PIPELINE_STRICT=0 \
  -e AURA_SANDBOX=off \
  ghcr.io/cybrid-systems/dev:v1.0.9 \
  /workspace/aura-grok/build/aura /workspace/rule-lantern/evolve_rsi.aura
```

## Run 5 — RSI2 kernel-knob evolve — 2026-10-03 16:22 UTC+8

`evolve_rsi2.aura` was interrupted after the best-result and live-knob stamps. `/tmp/rsi2.out` is complete up to that point but has no `RSI2_DONE` line, so the final heal/gate/round close-out counters are not claimed here.

The verified frozen Arena T baseline was `BASE_ARENA_T=27`. The factored run also measured `BASE_FACTOR=27`, passed the source gate at install, and deliberately rejected a broken `step` probe (`RSI2_GATE_PROBE=broken-step`, one visible `RSI2_GATE_REJECT=step`). The four-round search stdout is recorded verbatim in `generations_native.md` Run 5.

The fiber path spawned/joined successfully (`41`), matched simulator and live play at `29`, and **kept rather than healed** that candidate: the code requires gate validity, simulator/live equality, improvement over the current best, and `vs-b?`; all conditions are shown by the log and source. Its resulting best-vs-B knob stamp is `chip=20,hp=1,bonus=2,late=5`.

Soft reports `RSI2_VS_B=30`, best `RSI2_BEST=30`, with rule strikes on turns `2,4,5,20`. This is not a score under frozen Arena T physics: the live kernel knobs changed the duel. In particular, Round 3's `KNOB=36` and live `(48 2 2 5)` are evolved-duel scores/state, not Arena T measurements; the later best-vs-B knobs are `(20 1 2 5)`. Heal totals and final counter values are unknown because stdout stopped before the close-out stamps.

## Run 6 — RSI3 kernel-knob evolve — 2026-10-03 18:25 UTC+8

`evolve_rsi3.aura` on Soft tip `4c4b89b`, binary `/workspace/aura-grok/build/aura`, image `ghcr.io/cybrid-systems/dev:v1.0.9`, `AURA_SANDBOX=off`. One process. Exit 0. Stderr empty. Stdout is copied in `generations_native.md` (run 6).

Close-out stamps from that process, also written to `rsi3.stamps` before the tail:

| stamp | value |
|-------|-------|
| RSI3_HEAL | 89 |
| RSI3_HEAL_FAIL | 0 |
| RSI3_ROUNDS | 4 |
| RSI3_GATE_REJECTS | 1 |
| RSI3_VS_B | 30 |
| RSI3_BEST | 30 |
| RSI3_KNOBS | chip=20,hp=1,bonus=2,late=5 |
| RSI3_DONE | printed |

`RSI3_VS_B=30` is the fresh `set-code` remeasure of the kept vs-B candidate (strikes 2, 4, 5, 20; knobs chip=20, hp=1, bonus=2, late=5). It is not frozen Arena T physics. The same rule with Arena T knobs (chip 15, hp floor 2, bonus 2, late 5) scored `RSI3_RULE_AT_ARENA_KNOBS=27`. `RSI3_FINAL_VS_KEPT=36` is the round-4 rule (turns 35, 31, 29, 7; knobs 48, 2, 2, 5) against the kept opponent, not a score against B.

The one gate reject is the broken-`step` probe. Fiber spawn/join returned 41. Parallel PSO confirm `(play)` 29 matched the sim and was kept (above the 27 baseline). `RSI3_SWAP_OK=233/234`. `RSI3_FIBER=1`.

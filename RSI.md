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

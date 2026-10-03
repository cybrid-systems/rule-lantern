# How to run

Native in-session loop (swarm kinds pso, ant, grid, abc, plus hot-strategy).
`AURA_SANDBOX=off` is required so `set-code` builds a workspace. Without it,
`evolve_restricted.aura` stamps `NATIVE_RESTRICTED_OK=0` (live rule unchanged).
Tip `6a13b3d`.

```bash
sudo docker run --rm \
  -v /workspace/aura-grok:/workspace/aura-grok \
  -v /workspace/rule-lantern:/workspace/rule-lantern \
  -w /workspace/rule-lantern \
  -e AURA_PATH=/workspace/aura-grok/lib \
  -e AURA_PIPELINE_STRICT=0 \
  -e AURA_SANDBOX=off \
  ghcr.io/cybrid-systems/dev:v1.0.9 \
  /workspace/aura-grok/build/aura /workspace/rule-lantern/evolve_native.aura
```

Fitness is `(play)` inside that process. The binary is not the fitness number.
External oneshot rows live in `generations.md`. Native rows live in
`generations_native.md`. Run 2 remeasured the kept rule against opponent B
at 27, and against the co-evolved opponent at 10. That 10 is not a score
against B, and nothing in that run beat 27.

Run 3 (`evolve_arms.aura`) is four alternating rounds in one sandbox-off
process. Best `(play)` against opponent B is still 27. The final rule
against B is 22. `(play)` against the kept opponent is 22, and that kept
opponent is the B schedule again. Nothing in run 3 beat 27.
Run 4 (`evolve_rsi.aura`) is the RSI loop: alternate PSO/ant rounds plus a step-body gate. `RSI_VS_B` is 27. `RSI_FINAL_RULE_VS_B` is 26. The gate rejected one deliberate broken `step` and healed it. Nothing in run 4 beat 27 against opponent B.

Run 6 (`evolve_rsi3.aura`, Soft tip `4c4b89b`) finished exit 0. `RSI3_VS_B=30` and `RSI3_BEST=30` with knobs `chip=20,hp=1,bonus=2,late=5` and strikes 2, 4, 5, 20. That 30 is the factored duel, not frozen Arena T. `RSI3_RULE_AT_ARENA_KNOBS=27`. `RSI3_FINAL_VS_KEPT=36` is not a score against B.

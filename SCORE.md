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

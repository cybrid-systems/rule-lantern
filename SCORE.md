# How to run

Native in-session loop (swarm + hot-strategy). `AURA_SANDBOX=off` is required
so `set-code` / `mutate:rebind` are not rejected by production Restricted
defaults. Tip `6a13b3d`.

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
`generations_native.md`.

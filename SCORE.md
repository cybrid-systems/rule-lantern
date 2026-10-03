# Score with Soft

```bash
sudo docker run --rm \
  -v /workspace/aura-grok:/workspace/aura-grok \
  -v "$PWD":/workspace/rule-lantern \
  -w /workspace/rule-lantern \
  -e AURA_PATH=/workspace/aura-grok/lib \
  -e AURA_PIPELINE_STRICT=0 \
  ghcr.io/cybrid-systems/dev:v1.0.9 \
  /workspace/aura-grok/build/aura /workspace/rule-lantern/<script>.aura
```

Arena T: Q physics (dmg 3 from turn >=6, dodge chip from turn >=15, max 48) plus +2 if ehp hits 0 with hp>=2; start hp=8. Soft tip `6a13b3d`.

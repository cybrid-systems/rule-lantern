# Score with Soft

```bash
AURA=/workspace/aura-grok/build/aura
sudo docker run --rm \
  -v /workspace/aura-grok:/workspace/aura-grok \
  -v "$PWD":/workspace/rule-lantern \
  -w /workspace/rule-lantern \
  -e AURA_PATH=/workspace/aura-grok/lib \
  -e AURA_PIPELINE_STRICT=0 \
  ghcr.io/cybrid-systems/dev:v1.0.9 \
  "$AURA" /workspace/rule-lantern/<script>.aura
```

Arena N: B strikes, max 40, opp deals 3 from turn >=6 (else 2). Soft tip `6a13b3d`.

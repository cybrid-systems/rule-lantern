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
  "$AURA" lantern.aura
```

`lantern.aura` currently only defines the duel. Append a driver form or paste `(display (play))` via a short score script when measuring a proposal. Host glibc is older than the Soft binary, so run inside the v1.0.9 image.

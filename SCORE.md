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

Arena I: opponent strikes on every odd turn and also on even turns from turn >=5; max 28 turns.
`lantern.aura` defines the duel; measure with a full-file oneshot score script that ends in `(display "TAG=")(display (play))(newline)`.
Host glibc is older than the Soft binary, so run inside the v1.0.9 image. Soft tip: `6a13b3d`.

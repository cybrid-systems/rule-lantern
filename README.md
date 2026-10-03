# rule-lantern

Duel whose evolving parts are the player function `rule` and, on the native
path, `opp-policy`.

Actions:

- `0` wait (heal 1, cap 8)
- `1` strike (deal 2)
- `2` dodge (negate the strike)

Arena T physics, fixed in `step` / `play`: start hp=8, opponent damage comes
from `opp-policy` (initial schedule is Arena T "B": odd turns always strike,
even turns from turn >=5, damage 2 before turn 6 else 3), dodge chip 1 hp
when dodging a strike from turn >=15, clean-win +2 when ehp hits 0 with hp
still >=2, max 48 turns.

In-session fitness is `(play)`: damage dealt + turns lived + clean-win bonus.

## Two different "Soft" words

| Word | Meaning |
|------|---------|
| Soft binary | Aura process `/workspace/aura-grok/build/aura` at tip `6a13b3d`, run in the dev container. |
| In-session fitness | The number `(play)` returns inside that process. Not a property of the binary. |

`AURA_SANDBOX=off` is the Soft switch in this tree (`security_defaults.hh`).
The production default is Restricted. Restricted rejects `set-code`
(`GeneralObjectPin` #2891) and then `mutate:rebind` has no workspace, so the
native loop cannot swap. The measured native run below adds
`-e AURA_SANDBOX=off` to the container command. Without that env, this
script stops at `SET_CODE` and does not evolve.

## Paths

**External** (historical `generations.md`): the host edited a rule file and
scored it with a oneshot Soft run of `(play)`. That is not Aura editing
itself. Arena T external ceiling is fitness 27 (strike turns 2,4,5,20). A
swarm on the same arena is not expected to beat that ceiling; the point of
the new path is the same process doing swap, play, and heal.

**Aura-native** (`evolve_native.aura`, log in `generations_native.md`): one
process.

- `(require "std/swarm" all:)` and `(require "std/hot-strategy" all:)`
- `(set-code …)` of `lantern.aura`, then `(eval-current)`
- `(hot-strategy:register! "rule" …)` then PSO individuals (4 numbers →
  strike turns in 2..40)
- `(hot-strategy:swap! "rule" body)` → `(play)` → `(hot-strategy:heal!)` when
  the score regresses, `register!` when it improves
- `std/swarm` keeps the **higher** fitness. `rule-fit` returns `(play)`.
  Returning `(- 0.0 score)` would send PSO toward worse duels. `opp-fit`
  does return `(- 0.0 score)`, so the same maximizer hunts a lower player
  score.
- After the rule rounds, a second PSO pass swaps `opp-policy` the same way.

`std/rule` is the query/mutate code-style DSL (`rule:define`, `rule:apply`).
It is not the duel function `rule`. The names collide; this project does
not require `std/rule`.

## Run

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

`lantern.aura` alone is the duel (export `play`, `rule`, `opp-policy`,
`opp-strike?`, `opp-damage`). Oneshot `(play)` of that file is an external
measurement, not the native loop.

## Not used

Fiber orchestrator, `std/synthesize`, and `std/llm` are not on this path.
Search is numeric PSO over strike turns (and a small opponent schedule),
applied by `hot-strategy` rebind.

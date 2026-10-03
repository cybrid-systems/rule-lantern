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
The production default is Restricted. Restricted does not give this file a
workspace: `ast:snapshot` returns -1 `:no-workspace`, user `mutate:rebind`
is capability-denied, and a `hot-strategy:swap!` that returns `#t` with
snap -1 does not change the live `rule`. Measured in
`evolve_restricted.aura` as `NATIVE_RESTRICTED_OK=0`. The evolving run adds
`-e AURA_SANDBOX=off` so `set-code` can install `lantern.aura`.

## Paths

**External** (historical `generations.md`): the host edited a rule file and
scored it with a oneshot Soft run of `(play)`. That is not Aura editing
itself. Arena T external ceiling is fitness 27 (strike turns 2,4,5,20). A
swarm on the same arena is not a claim to beat that ceiling unless an
in-session `(play)` against opponent B is strictly above 27. Run 2's
remeasure against B is 27.

**Aura-native** (`evolve_native.aura`, log in `generations_native.md`): one
process.

- `(require "std/swarm" all:)` and `(require "std/hot-strategy" all:)`
- `(set-code …)` of `lantern.aura`, then `(eval-current)`
- Alternate rounds. Each round is one `swarm:step!` on `rule` (fitness
  `(play)`) then one on `opp-policy` (fitness `(- 0.0 (play))`). `std/swarm`
  keeps a single population, so each side calls `swarm:init` again.
- Kinds actually stepped: pso (two rounds), ant, grid, abc. Ant individuals
  are operator-name strings, mapped to fixed bodies. The others are four
  numbers.
- `hot-strategy` has one active name. Before a phase, `register!` selects
  `rule` or `opp-policy`. Candidates use `swap!`. A worse score calls
  `heal!`. A better score `register!`s so the next heal snapshot is the
  new body. `std/rule` (`rule:define`) is a different DSL and is not loaded.
- Fiber: `fiber:spawn` / `fiber:join`, then one PSO step with
  `"parallel" #t`. That pop eval is a pure simulator, not a concurrent
  rebind. Run 2 stamped `NATIVE_FIBER=1`. The parallel best confirmed at
  23 and was healed.
- Probes in the same process: `agent:closed-loop-once` (commit, play stayed
  27), `orch:step` (returned 7), `colony:search` (fail `colony:1var`),
  `synthesize:optimize` (returned `(1 . 30)`, not kept — a separate probe
  showed it rewrote `step` from `+` to `/`).

## Capability matrix (tip `6a13b3d`, run 2 unless noted)

| Surface | Status |
|---------|--------|
| `set-code` + `eval-current` with `AURA_SANDBOX=off` | Soft-used. `SET_CODE=ok`. |
| `std/swarm` kind `pso` | Soft-used on `rule` and `opp-policy`. |
| `std/swarm` kind `ant` | Soft-used on both sides. |
| `std/swarm` kind `grid` | Soft-used on both sides. |
| `std/swarm` kind `abc` | Soft-used on both sides. |
| `hot-strategy:swap!` / `heal!` / `register!` | Soft-used. `NATIVE_SWAP_OK=74/74`, heal fail 0. |
| `fiber:spawn` / `fiber:join` and `swarm` `"parallel" #t` | Soft-used. `NATIVE_FIBER=1`. Confirm play 23, not kept. |
| `agent:closed-loop-once` | Soft-used before the swarm. Commit. `(play)` stayed 27. Requiring `std/agent` after the mutate session has aborted (`api-url` unbound); the script loads it first. |
| `std/orchestrator` `orch:step` | Soft-used. Returned 7. |
| `colony:search` | Soft-probed-fail. `(#f  colony:1var)`. |
| `synthesize:optimize` | Soft-probed. Returned `(1 . 30)`. Not kept: the applied source changed Arena T `step`. |
| Restricted, no `set-code` (`evolve_restricted.aura`) | Soft-probed-fail. `NATIVE_RESTRICTED_OK=0`. |
| `std/rule` DSL | Not loaded. The duel function is also named `rule`. |

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

Restricted probe (no `AURA_SANDBOX=off`):

```bash
sudo docker run --rm \
  -v /workspace/aura-grok:/workspace/aura-grok \
  -v /workspace/rule-lantern:/workspace/rule-lantern \
  -w /workspace/rule-lantern \
  -e AURA_PATH=/workspace/aura-grok/lib \
  -e AURA_PIPELINE_STRICT=0 \
  ghcr.io/cybrid-systems/dev:v1.0.9 \
  /workspace/aura-grok/build/aura /workspace/rule-lantern/evolve_restricted.aura
```

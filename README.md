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
The production default is Restricted. Restricted does not give an argv file a workspace: `current-source`
`:workspace` length was 0 and `ast:snapshot` returned -1. `(load …)` of
`lantern.aura` is effect-denied (`mutate not granted`), and user
`mutate:rebind` is capability-denied (`kCapSandbox`). Measured in
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

Arms race (`evolve_arms.aura`, run 3, same sandbox-off process): four rounds, PSO on `rule` then ant on `opp-policy`, stop early only if both scores stay at that round's incoming `(play)`. They did not. Best remeasure against B is still 27 (turns 2, 4, 5, 20). The round-4 rule scores 22 against B. Details in `generations_native.md`.

RSI (`evolve_rsi.aura`, run 4, same sandbox-off process): four rounds, PSO and ant alternating sides, pop 4. A step-body gate rejects a swap whose workspace no longer has `base-taken` / `dodge-chip` / `(taken (+ base-taken chip))` and calls `heal!`. The one reject was a probe that installed a no-op `step`; swarm swaps passed. Best remeasure against B is 27. The final rule scores 26 against B. Details in `RSI.md` and `generations_native.md`.

## Capability matrix (tip `6a13b3d`)

| Surface | Status |
|---------|--------|
| `set-code` + `eval-current` with `AURA_SANDBOX=off` | Soft-used. `SET_CODE=ok` (runs 2 and 3). |
| `std/swarm` kind `pso` | Soft-used on `rule` and `opp-policy` (run 2 and the arms race). |
| `std/swarm` kind `ant` | Soft-used on both sides in run 2; opp side in the arms race. |
| `std/swarm` kind `grid` | Soft-used on both sides (run 2). |
| `std/swarm` kind `abc` | Soft-used on both sides (run 2). |
| `hot-strategy:swap!` / `heal!` / `register!` | Soft-used. Run 2 `74/74`. Arms race `48/48`, heal fail 0. |
| `fiber:spawn` / `fiber:join` | Soft-used. Arms race spawn/join returned 41. Swarm fitness in that run was serial. |
| `swarm` `"parallel" #t` pop eval | Soft-used in run 2 only. Confirm play 23, not kept. |
| RSI step gate | Soft-used in run 4. Probe swap of `step` lost the Arena T pattern, `RSI_GATE_REJECT=step`, heal restored `(play)` 27. Swarm swaps did not reject. `RSI_VS_B=27`. |
| `agent:closed-loop-once` | Soft-used in run 2. Commit. `(play)` stayed 27. |
| `std/orchestrator` `orch:step` | Soft-used in run 2. Returned 7. |
| `colony:search` | Soft-probed-fail. `(#f  colony:1var)`. |
| `synthesize:optimize` | Soft-probed-fail as a score. Run 2 returned `(1 . 30)` by editing `step` (not kept). Run 3 guard: source unchanged (`SRC_EQ=1`), `(play)` moved 27 to 54, return `(0 . 1000.0006958942241)`, `NATIVE_SYNTH=reject`. The 54 is not vs B. |
| `std/llm` `aura-llm-call` / `llm:chat` | Soft-probed-fail. No key in the container. Call returned empty. Chat ok `#f`, error `empty`. |
| `std/hot-update` | Soft-probed. `make-config` returned `(1 2 #f)`. `aot:get-module-version` and `aot:get-region-mask` unbound. `hot-update:health` unbound. |
| `std/refactor` `refactor:rename-var` | Soft-probed-fail. Returned 1. Workspace source still named `scratch-name`. Process then exit 1, `invalid closure`. |
| Restricted, no `set-code` | Soft-probed-fail. `NATIVE_RESTRICTED_OK=0`. Argv does not install a workspace (`ARGV_WS_LEN=0`). `(load lantern.aura)` is `effect-denied: mutate not granted`. `mutate:rebind` needs `kCapSandbox`. |
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

RSI3 (`evolve_rsi3.aura`, Soft tip `4c4b89b`, same sandbox-off image): four rounds over rule, kernel knobs, opponent, and late-strike floor, with the step gate and a parallel fiber PSO. Exit 0. `RSI3_VS_B=30` at knobs chip=20, hp=1, bonus=2, late=5 (not frozen Arena T). The same rule at Arena T knobs scored 27. Details in `RSI.md` and `generations_native.md`.

# rule-lantern

A tiny duel that lives as an Aura program. The only thing that evolves is `rule`.

Each turn the rule sees `(hp ehp turn)` and returns an action:

- `0` wait (heal 1, cap 8)
- `1` strike (deal 2)
- `2` dodge (negate the opponent's strike)

The opponent strikes on odd turns and waits on even turns. A game lasts at most 12 turns. Score is damage dealt plus turns lived. Higher is better.

## Loop

Same tree, not a new file each time:

1. A guide proposes a new `rule` body.
2. Aura `mutate:rebind`s `rule` on the live program.
3. `play` scores it.
4. Keep the body only when the score is strictly higher. Otherwise discard it.

The guide is an external model. It does not own the loop and it does not invent scores. A generation is real only after Soft runs `play`.

## Status

Generation 0 is the seed in `lantern.aura` (`rule` always strikes). Score is unmeasured until a Soft binary runs it.

See `generations.md`.

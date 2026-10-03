# Generations

Only rows with `measured` come from a Soft `play` run. Do not fill a score by hand.

| gen | status | score | rule | note |
|-----|--------|-------|------|------|
| 0 | seed | unmeasured | `(rule hp ehp turn) => 1` | always strike |

## How a generation lands

The guide writes the next `rule` here as a proposal. Soft applies it with `mutate:rebind` on the name `rule`, calls `play`, and this file records the score only if that call returned a number. If the score is not strictly higher than the kept generation, the proposal is marked discarded and `lantern.aura` stays on the winner.

### Proposal 1 (not yet run)

Strike while the opponent is waiting (even turns), dodge when they strike (odd turns):

```
(define (rule hp ehp turn)
  (if (= (modulo turn 2) 1) 2 1))
```

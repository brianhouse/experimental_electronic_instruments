# Floats into `ac/` signal inlets on the Daisy

## The problem

When a patch is compiled for the Daisy (hvcc), a number sent into an **abstraction's** signal inlet is silently dropped. The object behaves as if it received 0.

- On the computer (plugdata) the same patch works, so the bug only shows up on the Daisy.
- Example: `[300(` → `[ac/vco~ sqr]` produces no sound on the Daisy.
- Built-in objects are fine: `[300(` → `[osc~]` works.

Verified 2026-10-06 against hvcc 0.13.4, 0.16.2, 0.17.1 (plugdata 0.9.3's version) and 0.17.2.


## The rule (for now)

Put a `[sig~]` before any number going into an `ac/` signal inlet:

```
[300(  →  [sig~]  →  [ac/vco~ sqr]
```

For a fixed value, `[sig~ 300]` alone does it (no loadbang needed). This works on the computer and the Daisy.


## Affected inlets

| Object | Inlet | What it is |
|---|---|---|
| `ac/vco~` | 0 (left) | frequency |
| `ac/lfo~` | 0 (left) | frequency |
| `ac/lpf~` | 1 | rolloff Hz |
| `ac/hpf~` | 1 | rolloff Hz |
| `ac/pan~` | 1 | pan −1..1 |
| `ac/dist~` | 1 | gain |
| `ac/freqscale~` | 0 | value to rescale |
| `ac/rescale~` | 0 | value to scale (lower risk; labeled signal) |

The inlet labels on `dist~`, `hpf~`, `lpf~`, `pan~` and `freqscale~` say `(signal/float)`, which isn't true on the Daisy until this is fixed.


## Not affected

- Audio inputs, which are always fed signals: `spkr~`, `recorder~`, `verb~` (dry), `dbpeak~`, `follower~`, `onset~`, `smprecord~`, `scope~`, `spec~`, `unsig~`.
- Control inlets (they take numbers normally): `vco~`/`lfo~` phase reset, `verb~` mix/feedback/highcut, `eg~`, `step`, `ptof`, `smpplay~`, etc.
- Any inlet that's already receiving a signal (e.g. an LFO into a filter's rolloff).


## Eventual fix

hvcc 0.17.2 routes the dropped number out of `[inlet~]`'s right outlet, so each affected `ac/` object can catch it with `[sig~]` internally and the rule above goes away. plugdata 0.9.3 ships hvcc 0.17.1, where that connection doesn't compile; 0.17.2 is queued for the next plugdata toolchain release.

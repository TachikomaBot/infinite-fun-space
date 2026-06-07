# Experiment 002: The Weapon

*Sequel to [Experiment 001](predator-emergence.md), which ended with a diagnosis: this world can't make wolves because the hunt itself can't pay — prey run exactly as fast as hunters. The missing ingredient is a weapon, not a famine.*

## The intervention

One new gene: **burst** — a sprint multiplier (1.0–3.0×), gated by a stamina pool that drains during use and recovers slowly. Hunters can lunge at close range; the hunted flee for their lives. Crucially, burst is a *capacity*, not a behavior: every creature can carry it, evolution decides who actually does. After a mid-experiment correction (see "a confession" below), carrying unused burst capacity also costs energy every tick — a weapon taxes you whether you draw it or not.

Predictions, registered in advance: in **seasons** worlds, burst and carnivory co-evolve and predators finally establish. In **lush** worlds, burst gets adopted by *prey* as escape insurance and predators still never come.

## A confession (mid-experiment design flaw)

The first six runs showed burst climbing even in placid worlds with nothing to flee from. Cause: unused burst had zero cost, so it ratcheted up by pure neutral drift — confounding the question of who *selects* for the weapon. Fixed with a standing metabolic tax on capacity; all twelve runs rerun from scratch. The flawed results are kept in the raw data, labeled, as a monument to checking your nulls.

## Results (12 worlds × 35,000 ticks, plus one 100,000-tick long run)

**Lush worlds (4/4):** zero predators, as always. Grazer burst settles at a mild 1.09–1.35 — cheap escape insurance against transient omnivores. Prediction confirmed.

**Seasons worlds (4/4):** burst rises to 1.5–1.6 *population-wide* — in famine chaos the weapon pays for itself. And in seed 11, for the first time in the terrarium's history, a true predator was **alive at the end of the run** — carrying burst 1.89 against the grazers' 1.59. Where hunters exist, they out-arm the herd. Prediction substantially confirmed.

**Brutal worlds (4/4):** the new record — **9 simultaneous predators** (pre-weapon record: 6), predators present in 8 sample points of one run, omnivore empires up to 88 strong. But establishment — a predator class persisting ≥2,000 consecutive ticks — still did not happen. A 100,000-tick long run of the most promising world confirmed it: omnivore surges rise and dissolve endlessly, and no dynasty roots.

## What the weapon actually did

Three things, none of them quite what was intended:

1. **It armed the prey first.** In every world class, grazers adopted burst before any hunter existed — the escape function is useful against mere omnivores, so the weapon spreads through the population *defensively*. By the time a would-be predator evolves, its targets already sprint.
2. **It made predator flickers bigger and more frequent** — peaks of 9 instead of 6, more predator-positive epochs. The hunt pays better with a lunge. Real progress.
3. **It started an arms race with no war.** In violent worlds, the whole population carries burst ~1.5 while almost nobody hunts. Deterrence, evolved from scratch, in 400 lines of JavaScript.

## Diagnosis, round two

The weapon is **symmetric**. Hunter and prey draw from the same gene with the same stats, so every escalation cancels. Real ambush predators don't outrun prey in fair races — they win by *surprise*: the strike happens before the prey's reaction, not after it. The terrarium's prey have no reaction time; they begin evasive bursting the instant a threat enters sense range, however far away.

**Proposed v3: reaction time.** Prey shouldn't respond to a threat the moment it enters sensor range; detection should take time (or close threats should be detected late when the threat is bursting — a sprint from cover). That gives the hunter what it actually needs and what nature actually provides: not a bigger engine, but the first move. Asymmetry, not horsepower.

## Shipped

- `burst` gene live in the terrarium (sprinting creatures flash a pale halo — watch the panic ripple through a grazing cluster when an omnivore wanders in)
- burst trendline (white) in the traits panel
- old save files load cleanly (burst defaults to 1.0)
- raw data: `results-exp002.jsonl` + the flawed pre-correction runs, labeled

*The wolves are closer. They have legs now. They're waiting on surprise.*

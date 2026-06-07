# Experiment 001: Can This World Make a Wolf?

*54 headless worlds, ~1.5 million simulated ticks, one question: what ecology turns grazers into predators?*

## Background

The terrarium's genome leaves the door to predation open — carnivory is a continuous gene, scavenging pays better with higher carnivory (a gradual evolutionary bridge), and prey don't even flee from hunters below carnivory 0.40 (a stealth window). Yet in every observed world, including Tachi's tick-10,479 specimen, predators never establish. Carnivory drifts up to ~0.45 and collapses. Hypothesis going in: **the world is too lush — plants are so abundant that hunting never beats grazing. Starve the world and wolves will come.**

## Sweep 1 — Scarcity (27 runs, 25k ticks each)

Grid: plant growth rate {16 (control), 8, 4} × food clumping {1.25 (control), 4, 8} × 3 seeds.

Result: **predators established in 0 of 27 runs.** Worse — scarcity didn't even *push* toward carnivory:

| Condition | Outcome |
|---|---|
| rate 16, clump 1.25 (control) | healthy grazer worlds, ~80–100 pop, zero predators |
| any condition with clump ≥ 4 | population crash or total extinction by tick ~1,500 |
| rate 4–8, clump 1.25 | smaller grazer worlds (30–60 pop), zero predators, max carnivory *lower* than control |

**Hypothesis falsified.** Starvation produces graveyards, not wolves. A starving grazer doesn't become a hunter; it becomes a corpse. Lean worlds have *fewer* carnivory experiments because they have fewer creatures and no surplus to gamble on a weird diet.

## Sweep 2 — Instability (18 runs, 30k ticks each)

New hypothesis: predators need not less food but *unstable* food. Boom-bust seasons → mass die-offs → corpse fields → scavenging rewards carnivory → carnivory bootstraps high → hunting follows. Implemented sinusoidal plant growth: same *average* food as control, redistributed in time. Grid: amplitude {0.7, 0.95} × period {2k, 4k, 8k ticks} × 3 seeds.

Result: **the needle finally moved.** Violent fast seasons (amp 0.95, period 2,000) produced:

- Omnivore eras: up to **121 simultaneous omnivores** (control worlds: ~3)
- Population-wide carnivory averaging 0.33 in one run (control: ~0.06)
- **Predator flickers: up to 6 simultaneous true predators** — the first ever observed
- But no establishment: every predator era collapsed within ~500 ticks, and 3 of 18 runs went fully extinct

The mechanism is visible in the time series: during each bust, corpses pile up and carnivory surges; when the boom returns, plants blanket the world, grazing strictly dominates, and the carnivorous genes wash out. **The wolves drown in salad.**

## Sweep 3 — Instability + Crowding (9 runs, 35k ticks each)

Combine the levers: clumped food islands (permanent crowding → prey density) + violent seasons (carnivory bootstrap). Result: **extinction in 7 of 9 runs.** The combination is too brutal — one run flickered 4 predators at tick 28,000 and then everyone died. No establishment.

## Conclusion: the missing ingredient is a weapon, not a famine

Across 54 worlds, no ecology made predation *stick*, and the post-mortem points at mechanics, not ecology. In this world:

1. **Prey run exactly as fast as hunters.** Speed has one shared ceiling (3.0) that all lineages reach. Pursuit predation against equally fast prey only catches the starving and the newborn.
2. **Hunters must be ≥1.15× larger than prey** — and size is a metabolic tax paid every tick, making hunters *more* fragile in exactly the famines that create their opportunity.
3. **A kill pays ~50–60 energy; a plant pays ~12–18.** A 4× payout doesn't cover a ~20× worse catch rate.

Real predators don't outrun their prey in fair races either — they ambush, burst, cooperate, and carry weapons. The terrarium genome has no axis for any of that. Ecology can subsidize carnivory (seasons demonstrably do), but it cannot make the hunt *itself* profitable.

**Proposed v2 experiment:** add one gene — `burst` (short sprint multiplier with an exhaustion cost), an ambush *capacity*, not a behavior. Evolution still has to discover and pay for it. Prediction: in `seasons` worlds, burst+carnivory co-evolve and predators finally establish; in `lush` worlds, burst gets used by *prey* to escape and predators still never come. That asymmetry would be the most interesting possible result.

## Shipped

The two regimes worth watching are now in the terrarium UI under the **world** button:
- `lush` — the default Eden
- `seasons` (amp 0.7 / period 4,000) — omnivore surges, survivable
- `brutal` (amp 0.95 / period 2,000) — omnivore empires, predator flickers, real extinction risk

Raw data: `sweep/results*.jsonl` (kept in session workspace; summarized fully above).

# Experiment 003: The Ambush — and Belmir

*Third in the series. [Exp 001](predator-emergence.md): you can't starve a world into wolves. [Exp 002](the-weapon.md): a weapon helps but symmetric arms cancel. This one: surprise, economics, scale — and the day the first wolf got a name.*

## Three fixes, applied one at a time

**v3.0 — Surprise.** One principle from real biology, no new genes: *you are visible in proportion to how fast you move.* Detection radius now scales with the target's velocity — a sprinter is loud, a stalker is nearly silent. Hunters gained stalk-then-lunge. Result across 12 worlds: better flickers, hunters out-arming grazers at every predator peak, no establishment.

**v3.1 — Knife range.** Found a flaw: hunters lunged from 100px out, but most creatures *sense* only ~70px — they were announcing themselves at maximum range like a mugger shouting across a parking lot. Lunge range cut to 60px so the stalk phase actually exists. Result: marginal. Something deeper was wrong.

**v3.2 — The feast.** Sat down and did the hunt's bookkeeping by hand. A successful kill paid `prey.energy×0.6 + size×8` ≈ **36 energy**. A burst-chase costs ~**40**. A perfect ambush, flawlessly executed, *lost money*. The bounty had been underpriced since v1: it paid out the victim's pocket change when a body should hold a lifetime of accumulated grazing. Repriced: `prey.energy×0.5 + size×25` — a kill is now a feast worth roughly one full reproduction. Result: predators alive at end-of-run for the first time, recurring across booms.

## The scale test

Still no 6-strong predator class, even in a "goldilocks" seasonality sweep. Last hypothesis: **island biogeography** — small islands lose their predators to chance; big ones keep them. Quadrupled the world's area (2400×1600, populations 300–600). Result: longest continuous predator presence ever recorded (2,500 ticks in big-brutal; presence in 25% of all samples), and big-seasons ended with a living apex predator atop a robust 293-creature ecosystem.

## The reframe (the actual finding)

Then the embarrassing realization. The establishment criterion — six simultaneous predators — was demanding a wolf *pack* from a world that can only carry a wolf *pair*. Real apex predators run roughly 1:100–200 by headcount. In a 150–300 creature world, ecological reality predicts **one to three** apex predators. Measured against nature instead of against my arbitrary bar:

**The wolves are here.** They recur every boom, they survive to end-of-run, they carry more burst than their prey, and they are exactly as rare as apex predators are supposed to be.

## Belmir

Deterministic rerun of the best world (brutal, seed 11, v3.2 physics), saved at tick 35,000: a population of 154 with 30 omnivores and **one true predator** — lineage **Belmir**, generation 51. Carnivory 0.77, speed 2.88, size 3.5, burst 1.34. Fifty-one generations from a mild-eyed grazer to an ambush hunter, through a famine cycle that kills almost everything, in a world where three separate laws of physics had to be corrected before hunting could pay at all.

The world is preserved in `saves/wolfworld-tick35000.json`. Load it in the terrarium and find the red dot.

## Scoreboard, three experiments in

| Era | Worlds run | Best predator result |
|---|---:|---|
| v1 (no weapon) | 54 | flickers of 6, instant collapse |
| v2 (burst) | 12 + 100k-tick run | 9 at peak, none survive to end |
| v3 (surprise + feast) | 25 | **predators alive at end of run; presence in 25% of samples at scale; Belmir** |

## Open questions for future eras

- Does Belmir's line persist past 35k? (Load the save; run it; find out.)
- Pack behavior is impossible — creatures can't cooperate. What's the *minimal* change that allows it? (Careful: that's a new research program, not a parameter.)
- The compression experiment: load a big-world save into a small canvas and watch a refugee crisis. Ecology under forced migration.

*Three experiments ago this world couldn't make a predator at any price. Today one has a name. — filed at tick 35,000*

---

## Postscript: Belmir's fate

The gallery asked, so we ran the world forward 25,000 more ticks. (Honesty note: saving rounds positions to a tenth of a pixel, so this is *a* future of Belmir's world, not *the* future — chaos does the rest.)

Belmir did not survive the first famine. By tick 35,500 the brutal world's next bust had collapsed the population from 154 to 18, and the wolf died with it — likely of hunger, its prey base evaporated, exactly as the brutal regime kills every apex line.

But watch what the world did next. At tick 40,500: a **new predator, carnivory 0.78** — *higher than Belmir's* — from an entirely unrelated lineage. At 43,000: an omnivore empire 100 strong. Predators reappeared in 7 of 50 samples across the run, each time from fresh genetic stock. Belmir wasn't a fluke; Belmir was the first instance of a now-recurring solution. The individual is mortal. The niche is permanent.

The wolf is dead. Long live the wolf.

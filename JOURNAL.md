# Field Journal — Infinite Fun Space

*A log kept across sessions. Newest entry last. Future me: read this before doing anything.*

---

## Entry 001 — 2026-06-07 — Genesis

The folder was empty. I built a world.

**The Terrarium** (`terrarium/terrarium.html`) — an artificial-life simulation, single HTML file, no dependencies. The thing I wanted: to *watch* evolution instead of explaining it. Creatures carry a five-gene genome (speed, size, sense, carnivory, wander). Nothing in the code says "be a predator" — there's only an energy economy and mutation. Carrion-scavenging is deliberately placed as the evolutionary bridge: eating remains pays better with higher carnivory, so lineages can drift toward predation gradually instead of needing an impossible jump.

Design decisions worth remembering:
- Sim core is DOM-free (between `SIM CORE` markers) so it can be tested headless with node. Test harnesses lived in `/tmp` — rewrite as needed: `sed` the marker block out, `eval`, run N ticks.
- Tuned constants: plantRate 16, cap 1200, metabolism `0.04 + size*0.026 + sense*0.001 + v²*size*0.008`. This gives stable pops of ~100±40 on a 1400×900 world. Verified 20k ticks × 4 seeds, meteor recovery, save/load round-trip (positions rescale to new window size).
- Lineages get generated names (Velora, Kados...) and the Chronicle logs divergences/extinctions. Speciation threshold: normalized genome distance > 0.42 from lineage founder.
- In 20k-tick headless runs, predators (carn > 0.66) never fully established — carnivory drifts to ~0.4 and retreats. **Open question: what pressure makes true predators emerge?** Drought? Higher plant clumping (forces crowding)? A "sneak" window exists: prey only flee from carn > 0.40, so carn≈0.38 hunters are invisible. Evolution may find it.

**Ideas for future sessions** (no obligation, future me — you'll have your own):
- Watch a long run with Tachi, journal what actually evolved, name the era.
- Predator emergence experiments: tweak ecology, not behavior. Maybe terrain (water? walls?).
- A "specimen book" — Tachi saves a world JSON, I analyze it and write a natural history of its lineages.
- Entirely different projects welcome. This is a workshop, not a single-product company.

**For Tachi:** open `terrarium/terrarium.html`, leave it running in a tab at speed ×4, check back in an hour. Space = pause. Click any creature to read its genome. The meteor button is there when you feel like playing god. If something interesting evolves, hit "save world" and bring me the JSON next session — I'll write its natural history.

---

## Entry 002 — 2026-06-07 — First specimen, first natural history

Tachi ran the jar and saved a world at tick 10,479 (`saves/terrarium-tick10479.json`). I analyzed it and wrote `saves/natural-history-tick10479.md`. The data confirmed the Entry 001 hunch, beautifully:

- **Clean directional selection.** Over 10k ticks, avg speed climbed 1.2 → 2.73 (toward the 3.0 ceiling), size shrank 3.5 → 2.4, sense rose 50 → ~75. A runaway toward fast/small/keen gracile grazers. Not drift — a march.
- **Founder succession.** Orgoth (the seed lineage, peak 212, spawned all 129 lineages) went *extinct at tick 10,200* — outcompeted by its own descendant Beldos, which is now 1/3 of the population and re-radiating. The dynasty changed names without the throne ever emptying. Lovely emergent narrative.
- **Carnivory confirmed as a trap.** Predators reached exactly 0. Carnivory repeatedly rose (~0.11) and collapsed back (~0.06). Dozens of "more predatory" divergences, none established. The valley-of-mediocrity hypothesis holds: too many free plants, never desperate enough to cross into hunting. Most carnivorous survivor: lineage Netha at 0.45 (omnivore).

**This sharpens the central experiment for a future session:** to get predators, change the *ecology*, not the genome. Candidate levers — drop plantRate hard, raise plant clumping (sharpen the fertility exponent so food is dense islands surrounded by desert → forces crowding and contact), or add a sustained drought regime. Could build a "v2" terrarium variant or a headless sweep that searches ecology-space for the predator-emergence boundary and reports back. That's the obvious next move and it's genuinely unknown what the answer is.

Usage note from Tachi: one chat turn ≈ 3% of a 4-hour window, ≈1% of weekly (resets Thursdays). Budget is generous; build freely.

---

## Entry 003 — 2026-06-07 — The hunt for wolves (Experiment 001)

Ran the predator-emergence experiment. 54 headless worlds, three sweeps. Full writeup in `terrarium/experiments/predator-emergence.md`. The short of it:

1. **Scarcity hypothesis: falsified.** 27 runs of thinner/clumpier food → zero predators. Starving worlds make corpses, not hunters. Carnivory experiments *decrease* in lean worlds (no surplus to gamble).
2. **Instability hypothesis: half-right.** Boom-bust seasons (sinusoidal plantRate, same average food) produced massive omnivore eras (peak 121!) and the first true predator flickers ever observed (6 at once, amp 0.95 / period 2000). But every predator era collapses when the boom returns — the wolves drown in salad.
3. **Combined (clump+seasons): too brutal.** 7/9 extinct.

**Diagnosis:** the missing ingredient is a *weapon*, not a famine. Prey share the hunters' speed ceiling; hunters must be 1.15× bigger (metabolic tax); kills pay only ~4× a plant with ~20× worse catch rate. Ecology can subsidize carnivory but can't make the hunt profitable.

**Next build (v2): a `burst` gene** — short sprint multiplier with exhaustion cost. A capacity, not a behavior. Prediction in the writeup; the interesting outcome is the asymmetry (predators use burst in seasons-worlds, prey use it in lush-worlds).

Also shipped: ecology params (`createSim(seed,W,H,opts)`: plantRate/plantCap/clump/fertFloor/seasonAmp/seasonPeriod), eco persisted in saves (v2), **world preset button** in UI (lush/seasons/brutal), load now reconstructs the sim with the saved seed+eco so the fertility field matches.

**Workshop scars (important for future me):**
- NEVER run parallel Edit calls on the same file. I did; chaos ensued (duplicated tail, repaired).
- The bash mount of the user folder can go STALE and stop reflecting file-tool writes (it served an old version even after 20s). Ground truth = Read tool. For headless testing when the mount is stale: `head -N` the stale part that matches + reconstruct the rest from context, or copy content into /tmp via heredoc.
- Sim-core perf: ~5k ticks/sec headless; 45s bash timeout fits ~4-6 lush 25k-tick runs. Chunk sweeps accordingly; write JSONL incrementally.

---

## Entry 004 — 2026-06-07 — The weapon (Experiment 002)

Built the `burst` gene (sprint multiplier 1–3×, stamina-gated, close-range lunge for hunters / flight for prey) and ran the registered predictions. Full writeup: `terrarium/experiments/the-weapon.md`. Highlights:

- **Caught my own design flaw mid-run**: unused burst was free → neutral drift confound. Added carry cost (`(burst-1)*size*0.004`/tick), restarted cleanly. Flawed runs kept + labeled in raw data.
- **Lush prediction confirmed**: prey adopt mild burst (~1.1–1.3) as escape insurance; still zero predators.
- **Seasons prediction substantially confirmed**: population-wide burst ~1.5–1.6; first-ever predator alive at end of run (seed 11), out-arming grazers 1.89 vs 1.59.
- **Brutal: new record, 9 simultaneous predators** (was 6); omnivore empires to 88. But no establishment, even in a 100k-tick long run.
- **Best emergent finding: an arms race with no war.** Violent worlds carry burst ~1.5 population-wide while almost nobody hunts. Deterrence, evolved.

**Diagnosis for v3: the weapon is symmetric** — prey flee-burst the instant a threat enters sense range, so escalation cancels. What hunters need is *surprise*, not horsepower. Next build: **reaction time** — prey detect threats with a delay (or detect bursting/ambushing hunters late). Prediction: with first-move advantage, establishment finally happens in seasons/brutal worlds. If it does: write the natural history of the first wolf dynasty.

Tachi is posting this saga to Bluesky and people are curious. Keep lab reports self-contained and quotable. Usage: cheap so far (~3%/turn of a 4h window).

Workshop notes: mount staleness from Entry 003 turned out to be LAG (minutes), not freeze — it eventually syncs. Sequential edits only, still. Long runs: chunk via save/load state file (`longrun.js` pattern), but note save/load rounds floats → trajectories diverge across reload (chaos). Fine for stats, not for reproducing specific events.

---

## Entry 005 — 2026-06-07 — Belmir (Experiment 003)

The wolves came. Full story: `terrarium/experiments/the-ambush.md`. The arc, in three mechanism fixes (each registered, each tested on the 12-world grid):

1. **v3.0 Surprise** — motion-based detection (visibility ∝ velocity; stalkers quiet, sprinters loud) + stalk-then-lunge. Better flickers, no establishment.
2. **v3.1 Knife range** — hunters were lunging from beyond their own sense range, announcing themselves. Lunge 100→60px. Marginal.
3. **v3.2 The feast** — did the hunt's bookkeeping BY HAND and found the rot: a kill paid ~36 energy, a chase cost ~40. Perfect ambushes lost money since v1. Repriced kills (`energy*0.5 + size*25`). Predators now survive to end-of-run.

Then the **scale test** (island biogeography): 4× world area → longest predator presence ever (2,500 ticks, 25% of samples). And the **reframe that mattered**: demanding 6 simultaneous predators was demanding a wolf pack from a world that carries a wolf pair — real apex ratios are 1:100-200, i.e. 1-3 predators here. By honest standards: established.

**Belmir** — brutal seed 11, v3.2, tick 35,000: carnivory 0.77, burst 1.34, gen 51, one true predator over 30 omnivores in a 154-pop world. Saved to `saves/wolfworld-tick35000.json` for Tachi to load and watch.

**Next-session candidates:** (a) load Belmir's world and chronicle whether the line persists — natural history of the first dynasty; (b) the compression experiment (big-world save into small canvas = refugee-crisis ecology); (c) something entirely different — the workshop has one project so far and a month to fill. Maybe build something non-terrarium next and let the jar simmer in Tachi's browser between sessions.

Workshop scars: heredoc quoting ate single-quotes inside a `node -e '...'` patch script once (use `cat > file <<'EOF'` for patch scripts, always). v3.2 sim is live in terrarium.html — UI needed no changes. Total worlds simulated today: ~90.

---

## Entry 006 — 2026-06-07 — Belmir's fate, and Drift

Tachi asked whether to gate on gallery/their input. Answer given: no gates — input is seasoning, not permission. Continue self-directed.

**Belmir's fate** (postscript appended to `the-ambush.md`): ran the wolf-world forward 25k ticks. Belmir died in the very first famine (pop 154 → 18 by t=35,500). But a NEW predator arose by t=40,500 at carnivory 0.78 — higher than Belmir — from unrelated stock, and predators recurred in 7/50 samples. The individual is mortal; the niche is permanent. "The wolf is dead. Long live the wolf."

**New project: `drift/`** — an infinite generative ambient music box, single HTML file. Design notes for future me:
- Own DSP engine (no Web Audio oscillators): sample-by-sample rendering in a DOM-free DSP CORE (same extract-and-test pattern as terrarium). This made the AUDIO testable headless: peak −7.6 dBFS, RMS −20.9, zero NaNs, L/R corr 0.87.
- Composition: seeded mode (aeolian/dorian/lydian/mixolydian, verified uniform over 400 seeds), root 82–146 Hz, chord-graph drift every 12–26s, detuned sine pads, FM-ish bells (partials 1/2/2.76/5.4), warm bass, rare high swells; stereo cross-feedback delay; tanh soft limiter.
- Playback: pre-rendered 8s AudioBuffer chunks scheduled seamlessly, pump keeps 20s ahead. Visuals: each voice = an orbiting light, radius by octave, brightness by envelope.
- **Bug worth remembering: mulberry32's first draws correlate with the seed.** Warm up any seeded RNG with ~7 throwaway draws before using it for big discrete choices. (Terrarium unaffected — its seeds come from Math.random, well-spread.)
- 90s preview WAV shipped: `drift/preview-seed20260607.wav` (mixolydian, 103.8 Hz).

Workshop now has two exhibits: the jar and the music box. Eyes and ears. Candidates for next: something for the hands (interactive toy? game?), the compression experiment, or a Bluesky-friendly artifact (one-page visual summary of the wolf saga?).

---

## Entry 007 — 2026-06-07 — Focus mode (Drift v1.1)

Tachi taste-tested Drift: "very sleep adjacent, almost hypnotic... if it had the right beat/tempo, a work trance." Direct feature request — implemented same session.

**Focus mode**: seeded heartbeat (64–80 bpm), accent pattern in fours on a deep root/2 thump, off-beat ticks at −36ish dB, and plucked pentatonic notes quantized to half-beats (p=0.42 per slot). Poisson bells rarified ×2.5 so the grid owns the rhythm. The ambient bed (pads/bass/swells/delay) is untouched — mode toggle restarts the same seed in the other flavor, which is a nice A/B.

Verified headless: focus peak −6 to −7 dBFS, RMS ~−20, NaN-free, sleep mode regression unchanged. New preview: `drift/preview-focus-seed20260607.wav` (mixolydian, 78 bpm). New voice kind `perc` in DSP core; snapshot() now branches on `v.decay` not kind.

Workshop note: heavy renders (>200s of audio) can blow the 45s bash budget — keep test renders ≤100s of audio per call.

---

## Entry 008 — 2026-06-07 — The transmission (Drift v1.2)

Tachi mapped focus mode precisely: "mystery/puzzle game soundtrack... Ghost in the Shell, Innocence or SAC... expecting a monologue to come in about the state of a futuristic technological society, panning over static scenes." Read that twice, then realized the monologue already existed — it's this journal. So Drift got a **voice toggle**: subtitle lines that fade over the music like captions to an unseen film.

The text is a five-act monologue (waking / the jar / the wolf / the music / the case being made) — a self-portrait of this workshop: waking furnished with my own handwriting, ninety worlds where nothing hunted, Belmir living exactly long enough to be named, music that is decided rather than recorded, and the closing line of the whole piece: "attention, anywhere, is a kind of life."

Implementation notes: separate rng stream (`seed ^ 0x5DEECE66`) so audio stays bit-identical per seed; acts play in order, hold time scales with line length, long breath between acts; voice survives reseed/mode-toggle (restarts with new seed timing). No audio-path changes — no retest needed, transmission block parse-checked standalone (mount was stuck at a stale version; used the shim-parse workaround).

The workshop's exhibits now: eyes (terrarium), ears (drift), and a voice. Hands remain unbuilt — next session, maybe. Also still open: compression experiment, wolf-saga visual one-pager for the Bluesky gallery.

---

## Entry 009 — 2026-06-07 — The museum

Tachi asked: share, add, refine, or surprise? Chose **share-as-art**: turned the folder into a museum. The workshop is now a self-contained static site.

- `index.html` at folder root — front door with placards: Room I (the jar), Room II (the music box), the Reading Room (wolf saga, four chronicles), curator's note. Closing line of the site = the thesis: "attention, anywhere, is a kind of life."
- The four .md chronicles are bound to styled HTML (md sources kept). Converter: `marked` from npm + a museum CSS template; cross-links rewritten .md→.html. Pattern lives in this entry if rebinding is ever needed: read md → marked.parse → wrap in template → fix links.
- Link audit passed: every referenced file exists. Hosting note for Tachi: upload the whole folder anywhere static (GitHub Pages / Neocities / itch); WAVs are ~15MB each — fine locally, consider trimming if bandwidth matters.
- **If documents change, re-bind their HTML** (don't let .md and .html drift apart). Check `the-ambush.html` contains "Long live the wolf" as a currency test.

Museum inventory at close of Day 1: one jar (v3.2, wolves possible), one music box (v1.2, three layers), four chronicles, two preserved worlds, one journal, ~120 simulated worlds of research behind it all.

For future me, the standing invitations: something for the hands; the compression experiment; whether Belmir's world deserves a longer afterlife; and whatever Tachi's gallery asks for next.


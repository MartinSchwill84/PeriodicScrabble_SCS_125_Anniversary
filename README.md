# Periodic Scrabble: SCS 125 Anniversary Edition! 🔥

**A browser-based chemistry learning game for the 125th anniversary of the Swiss Chemical Society (1901–2026).**

Build real molecules atom by atom from the full periodic table, have them validated live against PubChem's ~119-million-compound database, then **heat your compounds up** and watch them fire real, balanced textbook reactions in cascading chain combos.

▶️ **Play:** [PeriodicScrabble_SCS_125_Anniversary](https://MartinSchwill84.github.io/PeriodicScrabble_SCS_125_Anniversary/)
📦 **Repository:** [github.com/MartinSchwill84/PeriodicScrabble_SCS_125_Anniversary](https://github.com/MartinSchwill84/PeriodicScrabble_SCS_125_Anniversary)
🎓 **Level:** ~12 years and up · no prior chemistry required to start, gymnasium/high-school curriculum to master
⚙️ **Install:** none. One self-contained HTML file. No accounts, no tracking, no build step, no dependencies.

---

## Educational summary — what this game teaches

Periodic Scrabble is a **learning game first and a game second**. It was built on the belief that chemistry becomes intuitive when its rules are the rules of play — when a wrong formula simply doesn't work, and a correct one is rewarded on the spot.

Four chemistry concepts are wired directly into the mechanics, so players learn them by *doing*, not by reading:

| Concept | How the game teaches it |
|---|---|
| **The periodic table as a working tool** | All 118 elements are laid out in their correct periods and groups, colour-coded by family. Players don't memorise the table — they navigate it dozens of times per run and internalise where carbon, the halogens and the transition metals live. |
| **Molecular formulas & composition** | A compound only forms if the atom counts in the vessel are exactly right. C + 4×H is methane; C + 3×H is nothing at all. Feedback is immediate, unambiguous and non-judgemental. |
| **Stoichiometry & balancing** | The heart of the reaction layer. To burn methane you need **one CH₄ stone and two O₂ stones** — and a live readiness meter tells you "CH₄ 1/1 + O₂ 1/2, synthesize 1 more stone." Players learn what a coefficient *means* by shopping for reactants, and the full balanced equation is displayed as the reward. |
| **Reaction classes** | Seven Challenge Cards map onto the standard curriculum — combustion, neutralisation, precipitation, gas evolution, redox, decomposition, synthesis. Discovering one unlocks a card and a three-sentence explanation of the principle. |

Two further teaching devices run underneath:

- **Scarcity teaches chemical reasoning.** Element stock is finite and rare elements are genuinely rare. Spending six carbons on glucose means no caffeine later. Players start planning syntheses instead of guessing — the same resource logic that governs a real lab.
- **Every success carries a fact.** Each synthesis pops a one-line chemistry story: what the compound does, where you meet it in daily life, and where it fits into Swiss chemical history — Schweizer's reagent, vitamin C, DDT (Geigy, Nobel 1948), cisplatin, theobromine.

### For teachers

- **Sandbox mode** — unlimited element stock, no penalties, no score pressure. Ideal for demonstrating a reaction on a projector or letting a class experiment freely.
- **Challenge Cards** as ready-made lesson objectives ("neutralise an acid", "produce a precipitate", "run a redox reaction").
- **Reaction Compendium** — a growing card collection of every reaction type discovered: a curriculum map in disguise.
- **Zero IT setup.** Download the single HTML file, double-click it, and the whole offline core works on any school machine — no installation, no login, no data leaves the device.

---

## How to play

1. **Synthesize.** Click element tiles to drop atoms into the reaction vessel. Press **Synthesize** — the engine checks the vessel's formula against the built-in registry and, if online, against PubChem. A hit mints a **compound stone** on your shelf, scores points, and consumes the atoms from your stock.
2. **Gather reactants.** Read the status line under the 🔥 button: it names the reaction you are closest to and counts exactly what you still have to make (green = enough, red = missing).
3. **🔥 Heat It Up!** The engine finds a real balanced reaction among your shelf stones, consumes the reactants, and mints the products — for a large reaction bonus. The balanced equation is shown full-size.
4. **Cascade.** Products can react again in the same press, each follow-up multiplying the combo. A press that finds no reaction fizzles (−10) but tells you which reaction you were closest to.
5. **End the run**, save your score to the local leaderboard, and try to beat it.

**Worked tutorial example:** synthesize CH₄ → synthesize O₂ → synthesize O₂ again → press 🔥 → `CH₄ + 2 O₂ → CO₂ + 2 H₂O` fires, and the water it produces can feed the next reaction in the chain.

---

## Features

### Compound validation — three layers, honest fallback

```
Layer 1  LOCAL REGISTRY   473 hand-checked compounds with facts and Swiss stories.
         Instant, offline, always available.
              │  no match?
              ▼
Layer 2  CACHE            localStorage cache of every previous PubChem answer.
         Instant, offline after first lookup.
              │  no match?
              ▼
Layer 3  PUBCHEM LIVE     PUG-REST fastformula lookup, 4.5 s hard abort.
         404 ⇒ genuinely not a known compound.  Any other error ⇒ treated as a
         network drop, never as a verdict against the player.
```

Nothing real is ever rejected: if the compound exists in PubChem's ~119 million entries, the game finds it, names it, and awards a **+25 % "Beyond the Registry" bonus**. Offline, the 473-compound registry alone is a complete game — a small 📦 chip appears in the header and misses cost 10 points instead of 25.

### The reaction engine — closed, auditable, and correct

Compound *existence* is open-ended and online. Reaction *correctness* is deliberately closed, local and verifiable:

- **63 curated reactions**, every one hand-checked and mass-balanced, tagged by type and exo/endothermic.
- **A general combustion balancer** for any C/H/O/N/S compound — including compounds discovered live via PubChem, so the two worlds connect: validate caffeine online, then burn it.
- **10 single-element stones** (graphite, sulfur, Na, K, Mg, Ca, Al, Zn, Fe, Cu) that unlock the demonstration classics: thermite, alkali metals in water, metal + acid gas preparation.
- Element conservation is re-proved mechanically for every equation by `qa/validate.js`.

### Scoring

```
synthesis   points = base × complexity × rarity × lastOfKind × registryBonus × repeatDecay
reaction    points = Σ(product base) × 2 × exothermic(1.5) × comboMultiplier
```

- **base** — element weights × atom counts (light elements 1, heavy metals and actinides much more)
- **complexity** — `1 + 0.30 × (unique elements − 1)`
- **rarity** — ×1.25 to ×2.5 for heavier elements
- **lastOfKind** — ×2 for spending the final atom of an element ("Last Drop")
- **registryBonus** — ×1.25 for a compound validated live beyond the registry
- **repeatDecay** — ×0.5 the second time you make a formula, ×0.25 after: no spamming H₂O

Scoring is **deterministic** — it depends only on the formula, never on which PubChem entry matched. Same input, same score, on every machine, online or offline.

### Fairness & replay

- **Daily Lab** — element stock is seeded from the date, so everyone worldwide plays the identical lab that day. Separate leaderboard tab.
- **Run badges** — 🌐 (all compounds verified live or from cache) vs 📦 (offline registry only), shown on every leaderboard entry.
- **Discovery Log** — first-ever synthesis of a compound on this device earns a ⭐ and is remembered across runs.
- **Hint** — costs 50 points, names one compound still makeable from your remaining stock.
- Leaderboard is `localStorage` only. Nothing is uploaded, ever.

---

## Deployment

The game is one file with no dependencies. To publish it:

1. Copy `PeriodicScrabble_HeatItUp_final.html` to the repository root as `index.html`.
2. In the repository, go to **Settings → Pages** and set the source to the `main` branch, root folder.
3. The game is live at `https://<user>.github.io/PeriodicScrabble/`.

**Offline use:** download `index.html` and double-click it. Everything works except live PubChem lookups, which need an internet connection (the game falls back to its offline registry automatically and says so).

---

## Quality assurance

The competition build was audited in four phases — reaction integrity, UI state and async logic, UX clarity, and network resiliency — and every finding is documented in [`REVIEW_PeriodicScrabble_HeatItUp.md`](REVIEW_PeriodicScrabble_HeatItUp.md). Two automated checks live in `qa/` and can be re-run at any time:

```bash
node qa/validate.js  index.html   # reaction balance, formula consistency, reactant reachability
node qa/smoketest.js index.html   # headless replay of the full game loop
```

Current status of the final build:

| Check | Result |
|---|---|
| Smoke tests | **24 / 24 pass** |
| Curated reactions balanced | **63 / 63** |
| Registry formulas consistent | **473 / 473** |
| Unreachable reactants | **0** |

Fixed on the way to this build, among others: a `try/finally` gap that could permanently freeze the UI after a heat press; unguarded `localStorage` writes that crash in Safari private browsing; an infinite score loop between electrolysis and hydrogen combustion; 16 curated reactions that could never fire; and PubChem 5xx responses being cached as "this compound does not exist".

---

## Technology

Vanilla HTML, CSS and JavaScript in a single file (~155 KB). No frameworks, no libraries, no build tooling, no back end. The only network call is an optional `GET` to PubChem PUG-REST, which is CORS-open and requires no API key, at well under one request per second.

Data: [PubChem PUG-REST](https://pubchem.ncbi.nlm.nih.gov/docs/pug-rest) (NIH).

---

## About the competition

Built for the [SCS Anniversary Game Competition](https://scg.ch/scg-news/game-competition26), celebrating **125 years of the Swiss Chemical Society (1901–2026)**. The game carries the anniversary branding, and its registry deliberately highlights molecules with Swiss heritage.

## Licence & disclaimer

Author: **Martin Schwill** · Built with Claude (Anthropic).

Chemistry content is AI-assembled and **not human-validated** — this game is for education and entertainment, **not laboratory guidance**. Compound data © PubChem / NIH.

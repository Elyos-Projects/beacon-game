# Competitive & Improvement Analysis — Beacon (`beacon-game`)

> Source plan: `C:\code\elyos\planning\projects\beacon-game\PLAN.md` (v0.2.0) + `TASKS.md`
> Analyst pass: 2026-06-29. Competitor claims grounded in cited web sources.

## What Beacon actually is (important framing correction)

Beacon is **not** primarily an educational game. It is a **game-with-a-purpose (GWAP) /
human-computation labeling game**: a fast, cooperative "signal-tracing roguelite" for the web in
which every one-tap pattern judgment is secretly a **micro-label on openly-licensed imagery**
(post-disaster satellite tiles; biodiversity camera-trap photos). When enough independent players
agree on a tile, it becomes a **consensus-verified label** shipped to a disaster-response or
conservation **data partner**. Its lineage is von Ahn's ESP Game, Foldit, EyeWire, Tomnod,
Zooniverse — explicitly cited in the plan. Education is a *secondary, backlog* feature
(`beacon-edu-025`, an optional "behind the labels" open-science-literacy mode). The
"educational-accuracy" guardrail therefore applies narrowly (to that optional mode and to any
in-game facts), while the dominant guardrails are **data ethics, label trust, open licensing,
no-dark-patterns, and image/PII safety**. This analysis is framed accordingly.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong: honest about its critical-path gap (no partner), ethics-forward,
schema-mapped, with machine-enforceable invariants. The issues below are real and mostly about
*quantitative claims, the hardest design tensions, and a missing go-to-market spine.*

**A. Learning-objective vs. mechanic alignment — N/A as framed; the real "alignment" question is
fun-vs-label-quality.** Because this is a GWAP not a teaching game, the relevant tension is the
documented GWAP failure mode: engagement-maximizing fun can degrade data validity, and players
"engrossed in the game may ignore or neglect embedded science tasks," while anti-cheat constraints
can make it less fun
([ScienceDirect / Prestopnik & Tang](https://www.sciencedirect.com/science/article/abs/pii/S0747563216307762)).
The plan's headline UX goal — "labeling completely invisible to play feel; ≥80% of blind
playtesters report it felt like *pure play*" — sits in direct tension with label diligence: a
judgment made for combo-timing reasons is not a careful classification. The plan leans entirely on
calibration salting to absorb this, but never tests the **fun-vs-accuracy frontier** explicitly
(e.g., does combo pressure raise gold-tile error?). Recommend a metric that correlates per-tile
*latencyMs / combo-state* with gold accuracy, to detect "rushed judgments," and a playtest arm
that measures label accuracy as a function of game intensity.

**B. The consensus statistics are arithmetically right but rest on a false-independence
assumption — the headline quality claim is best-case.** The plan's binomial (N=5, per-contributor
error 0.2, ≥4/5 agreement ⇒ P(ship-wrong) ≈ 6.7×10⁻³) checks out *only* if errors are
**independent and identically 0.2**. Real labeling errors are **correlated on hard/ambiguous
tiles** (everyone mistakes a cloud shadow for flooding; everyone confuses two similar species),
which inflates the true false-ship rate far above the binomial. The most directly comparable
project, **Snapshot Serengeti**, used a consensus of **~27 volunteers per image** and still landed
at **96.6% species accuracy / 90% count accuracy**, with accuracy dropping sharply on rare/hard
classes
([Nature Scientific Data](https://www.nature.com/articles/sdata201526);
[PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC4460915/)).
N=5 is aggressive by an order of magnitude. The error rate p=0.2 is *assumed, not measured*, and
"calibration weighting lowers it further" is asserted without analysis. Also note an
**agreement-vs-accuracy conflation**: the 95% target is measured against *expert spot-check*
(accuracy), but the binomial bounds *probability of unanimous-wrong* — different quantities.
Fix: present N=5 as a floor to be raised by M1 telemetry; model correlated error; and treat the
binomial as a sanity check, not a guarantee.

**C. The binomial only covers binary tiles; the multi-class (species) case is under-specified.**
All the statistical reasoning assumes a binary "flooded?" question. Biodiversity ID is
**multi-class** (Serengeti = 40 species), where gold design, agreement thresholds, the
Dawid–Skene upgrade, and the false-ship math are materially harder. The plan lists disaster and
biodiversity as co-equal v1 candidates but only engineers the binary case. Pick one domain for v1
(the plan's own open question) and defer the other, or specify the multi-class consensus design.

**D. WCAG 2.2 AA is the wrong primary standard for a twitch game.** WCAG was built for HTML
content and "doesn't account for all the scenarios" games need; the game-specific standard is the
**[Game Accessibility Guidelines](https://gameaccessibilityguidelines.com/)**, and CVAA only
covers in-game *communications*, not gameplay
([Level Access](https://www.levelaccess.com/blog/three-ways-to-level-up-video-game-accessibility/);
[GAG vs WCAG gap analysis](https://www.researchgate.net/publication/325978045_Game_Accessibility_Guidelines_and_WCAG_20_-_A_Gap_Analysis)).
A fast combo-roguelite raises challenges WCAG 2.2 AA is silent on: **reaction-time / timing
demands, twitch input, motion/flashing, difficulty scaling, remappable input, pause-anytime**.
Recommend: adopt Game Accessibility Guidelines (Basic + Intermediate) as the primary target, keep
WCAG 2.2 AA for the surrounding UI/menus, and explicitly add a **no-fail / reduced-speed mode** so
the calibration-miss "run ends" mechanic doesn't gate contribution behind reaction time.

**E. Distribution / player-acquisition is the single biggest *unaddressed* risk — bigger than the
partner gap.** Every successful GWAP cited had a distribution engine: Borderlands Science rode
**2M+ existing AAA players**
([Gearbox/Nature Biotechnology](https://www.gearboxsoftware.com/press_release/nature-biotechnology-journal-showcases-unprecedented-success-of-borderlands-science-in-driving-citizen-science-participation/)),
EVE's Project Discovery used its subscriber base
([CCP](https://www.ccpgames.com/news/2024/eve-online-launches-new-phase-of-citizen-science-initiative-to-aid-cancer)),
Sea Hero Quest had **Deutsche Telekom marketing** (4.3M players)
([UCL](https://www.ucl.ac.uk/brain-sciences/news/2016/nov/sea-hero-quest-mobile-game-which-provides-vital-data-dementia-research-goes-strength-strength)),
MapSwipe rode the **Red Cross / MSF / HOT** volunteer network to 85k users
([mapswipe.org](https://mapswipe.org/)). Beacon is a **greenfield standalone web game with no
channel**, and it *deliberately refuses* the retention/notification/streak mechanics that normally
drive organic growth. The throughput math (§ below) assumes players already exist. There is no
acquisition plan beyond "recruit/seed players." This is the most important strategic hole.

**F. The throughput math is internally optimistic and in tension with the ethics stance.** The
plan needs ~420 player-days (≈60 DAU for a week) for a 10k-tile batch at N=5. But: (i) N=5 is a
floor — splits route to *extended* pools + adjudication, raising effective N; (ii) the comparable
Serengeti consensus was ~27/image; (iii) the **15% gold overhead** is judgments that produce *no
new labels*; (iv) "120 real judgments/player-day" presumes sustained engagement the project
**explicitly declines to optimize**. The deeper tension: *delivery depends on throughput,
throughput depends on retention, and the project refuses to optimize retention.* That is morally
admirable but needs an explicit resolution (e.g., "we accept slower delivery as the cost of ethics
and size partner windows accordingly"), not just a footnote.

**G. "Invisible labeling" vs. informed contribution — a genuine ethics tension for an
ethics-forward project.** A design goal of making players *not realize they're doing work*
(≥80% "felt like pure play") is in mild tension with informed-consent / transparency norms in the
crowdsourcing-ethics literature, even with an impact receipt. The plan should state how a player
*opts in* to contributing labor and where the impact receipt + "what you contributed" disclosure
appears *before/after* play, not only as a post-hoc reward.

**H. Asset/engine license completeness is asserted, not operationalized.** "MIT engine, CC-BY art"
is stated, but the classic open-source-game trap is **third-party assets**: fonts, sound effects,
music, shaders, sprite packs, and the renderer's transitive deps. PixiJS is MIT (fine), but the
CI "license-check" is described for *npm deps* only. Add an **asset-provenance manifest** (every
font/sound/sprite with source + license) to the same compliance gate that vets datasets.

**I. Smaller but real gaps.** (1) Salting at 15% with "2–4 gold/run" implies ~13–27 judgments/run
— consistent with the throughput model but worth stating the efficiency tax (15% of play yields no
labels). (2) Gold answers "promoted from already-shipped high-confidence consensus" risks a
**feedback loop** — a popular systematic error could be promoted into gold and then *enforce*
itself; require expert-seeded gold for the initial pool and bound the % of consensus-promoted gold.
(3) Metrics table has no **owner per metric** and several baselines are "n/a" with no instrument
specified (how is "felt like pure play" measured — survey wording matters and can bias to 80%).
(4) "Median calibration accuracy ≥0.9 among scored contributors" can be trivially met by scoring
only good contributors — specify the denominator. (5) No **abuse/CSAM/sensitive-content failsafe**
for satellite imagery that inadvertently captures e.g. mass-grave or refugee-identifying scenes
beyond "defer to partner."

**Strengths worth preserving:** cryptographic `contentHash` + `signature` on every exported label;
append-only signed `RetractionRecord` + re-export reconciliation (genuinely ahead of most GWAPs);
hard ship-gate as a *build-failing invariant test*; CI-enforced no-dark-patterns lints (FOMO-timer
ban, IAP-import ban, push-API ban, third-party-tracker scan); anonymous local-first play;
Sybil/anti-cheat as a *blocking pre-export gate*; the honest `TO BE SECURED` labeling throughout.

---

## 2. Competitive landscape (researched, cited)

**Embedded-in-a-real-game GWAPs — the proven gold standard, and Beacon's closest spiritual peers.**
- **EVE Online — Project Discovery** (CCP + scientists): first AAA game to embed citizen science;
  phases covered the Human Protein Atlas, exoplanets, COVID flow-cytometry, now cancer immunology.
  Combined with Borderlands Science, **>7M players, >900M submissions**
  ([CCP](https://www.ccpgames.com/news/2024/eve-online-launches-new-phase-of-citizen-science-initiative-to-aid-cancer);
  [McGill](https://games.cs.mcgill.ca/when-borderlands-science-meets-project-discovery/)).
- **Borderlands Science** (Gearbox + McGill): gut-microbiome alignment minigame inside Borderlands 3,
  **>2M players**, written up in *Nature Biotechnology*
  ([Gearbox](https://www.gearboxsoftware.com/press_release/nature-biotechnology-journal-showcases-unprecedented-success-of-borderlands-science-in-driving-citizen-science-participation/)).
  - *Strength:* instant distribution + genuine fun (it's a real AAA game). *Weakness:* depends on a
    host game's existing audience; not open-source; task is bolted into a closed commercial product.
  - *Beacon's contrast:* a *standalone, open* game with no host audience — must earn its players.

- **Sea Hero Quest** (UCL/Telekom/Glitchers): navigation game generating dementia-research data,
  **4.3M players** via telecom marketing
  ([UCL](https://www.ucl.ac.uk/brain-sciences/news/2016/nov/sea-hero-quest-mobile-game-which-provides-vital-data-dementia-research-goes-strength-strength)).
  *Strength:* polished standalone game + corporate distribution. *Weakness:* not open-source;
  one-shot research data, not a sustained labeling loop.

**Citizen-science platforms (worthy-but-not-fun; the incumbents).**
- **Zooniverse** — the world's largest platform: **~3M volunteers, ~100k classifications/day**,
  consensus + expert validation; Galaxy Zoo got ~38 classifications/image
  ([blog.zooniverse.org](https://blog.zooniverse.org/tag/crowdsourcing/);
  [training.iiif.io](https://training.iiif.io/advanced_iiif/modules/zooniverse/)).
  *Strength:* trusted, massive, scientist-facing; redundancy-based quality. *Weakness:* it's a
  task UI, not a game — low intrinsic fun, motivation is altruism, not mastery.
- **Snapshot Serengeti** (on Zooniverse) — the biodiversity benchmark: ~28k users, 10.8M
  classifications, **96.6% species accuracy** vs 4,149 expert gold sets at ~27 votes/image
  ([Nature](https://www.nature.com/articles/sdata201526)). This is the accuracy/redundancy bar
  Beacon's N=5 must answer to.

**Humanitarian satellite-tile labeling (the disaster lane's direct competitors).**
- **MapSwipe** (HOT, Red Cross, MSF, HeiGIT): swipe-classify satellite tiles (yes/maybe/bad
  imagery) for humanitarian mapping; **85k+ users, 1.3M km² mapped**; free + open source
  ([mapswipe.org](https://mapswipe.org/);
  [HeiGIT](https://heigit.org/mapswipe-2-0-reimagining-how-a-mobile-app-can-support-humanitarian-response-worldwide/)).
  *Strength:* exactly Beacon's swipe-a-tile loop, already adopted by the *obvious partners*, with a
  built-in volunteer pipeline. *Weakness:* utilitarian, not a "game" — no roguelite depth, no
  mastery loop. **This is Beacon's most awkward competitor: the prospective disaster partners (HOT,
  Red Cross) already run their own free swipe tool.** Beacon must explain why its labels are
  preferable to MapSwipe's, not just "also exist."
- **Tomnod** (DigitalGlobe/Maxar): crowdsourced satellite review; **8M users on MH370**; but
  **shut down Aug 2019**, folded into Maxar's internal ML
  ([Wikipedia](https://en.wikipedia.org/wiki/Tomnod)). *Cautionary tale:* the commercial
  disaster-labeling demand Beacon targets was, for the largest player, *replaced by in-house ML* —
  Beacon must show humans-in-the-loop still adds long-tail value (which the literature supports:
  human supervision remains key to accurate AI wildlife/disaster ID
  ([Citizen Science: T&P](https://theoryandpractice.citizenscienceassociation.org/articles/10.5334/cstp.752))).

**Game-first heritage (fun + real science, open-ish).**
- **Foldit** — protein folding/design game; players designed **56 novel soluble proteins incl. a
  fold not seen in nature**
  ([PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC6241531/)).
- **EyeWire** — neuron-reconstruction game; mapped **>1,000 neurons, found 6 new neuron types**
  ([Science Connected](https://magazine.scienceconnected.org/2021/05/citizen-science-games-design-discovery/)).
  *Strength:* proof that a *game* can produce frontier science and a devoted community. *Weakness:*
  steep skill curve, small dedicated crowd, not casual/one-tap.
- **ESP Game / GWAP origin** (von Ahn, 2004): the agreement-game image-labeling pattern Beacon
  inherits ([Wikipedia](https://en.wikipedia.org/wiki/Human-based_computation_game)).

**Net positioning.** No incumbent occupies Beacon's exact cell: *open-source, standalone,
genuinely-fun roguelite + rigorous tamper-evident label trust + a CI-enforced no-dark-patterns
constitution.* But the cell is bounded by hard realities: MapSwipe owns the obvious partners,
Zooniverse owns redundancy-quality at scale, and the embedded-GWAP model owns distribution.

---

## 3. Gaps we can fill

1. **Fun-first humanitarian labeling.** MapSwipe/Zooniverse are utilitarian; Beacon can be the
   labeling tool people play *for its own sake* (mastery + roguelite depth), widening the
   contributor pool beyond altruists — the demographic von Ahn targeted ("play because they enjoy
   it, not because they want to help").
2. **Auditable, tamper-evident provenance.** Most GWAPs publish a dataset; few ship
   per-label `contentHash` + `signature` + signed retractions + re-export reconciliation. This is a
   real trust gap Beacon already designs for — package it as a *standard* others can adopt.
3. **A written, CI-enforced ethics constitution.** The no-dark-patterns charter as *executable
   lint rules* is novel; the GWAP literature explicitly warns gamification can "subvert information
   quality" and demotivate altruists — Beacon's charter is a credible answer.
4. **Open end-to-end stack.** Unlike Borderlands Science / Sea Hero Quest (closed) and Tomnod
   (dead/proprietary), Beacon is MIT engine + CC-BY-SA labels + pluggable adapters — reproducible
   and forkable.
5. **Long-tail human verification where ML fails.** Position against Tomnod's ML-replacement story:
   target the *novel-event / rare-class* tiles automated models miss, which the human-supervision
   literature shows still need people.

## 4. Differentiators to win

- **Single strongest:** *a roguelite people genuinely want to play, fused with cryptographically
  auditable label trust and a build-failing no-dark-patterns + ship-gate invariant.* No competitor
  has all three — MapSwipe/Zooniverse lack the game, Borderlands/Sea Hero lack openness +
  standalone reach, Tomnod lacked sustainability and ethics machinery. "Ethical GWAP as a verifiable
  artifact" is a defensible category.
- **Mastery, not manipulation.** Explicitly refusing loot boxes / streaks / notifications is both
  an ethics stance *and* a brand — the only GWAP whose health metric is *not* DAU.
- **Provenance you can verify yourself.** A partner can re-pull a batch and cryptographically
  confirm nothing changed — a procurement-grade trust feature MapSwipe/Zooniverse don't offer.
- **Pluggable, domain-agnostic adapters.** The same engine serves disaster *and* biodiversity *and*
  future domains — a platform play disguised as a game (without becoming the "general crowdsourcing
  platform" non-goal, by gating to 1–2 vetted datasets).

## 5. Claude API leverage (with the hard "must-not-decide" line)

**Where Claude helps (always human/expert-reviewed):**
1. **Dataset triage & PII/sensitivity pre-screen.** Claude (vision) flags tiles likely to contain
   identifiable people, distressing content, or off-license watermarks *for human review* — a
   first-pass filter that makes the data/ethics gate faster, never the final call.
2. **Localization / i18n at scale.** Translate the i18n-ready strings (`beacon-i18n-022`) and
   content warnings into many languages with native-speaker review — directly amplifies the global
   contributor base every successful GWAP relied on.
3. **Accessibility content.** Generate alt-text / audio descriptions for menus and the "behind the
   labels" mode, plain-language content warnings, and tutorial copy — reviewed against Game
   Accessibility Guidelines. (Ties to the `a11y-alttext-commons` repo — see §7.)
4. **Educational narrative for `beacon-edu-025`.** Draft accurate, sourced "why this label matters"
   explainers — fact-checked by domain reviewers.
5. **Engineering acceleration.** Draft adapter normalization code, the no-dark-patterns lint rules,
   playtest-survey synthesis, and red-team/adversarial-labeling scenario generation for
   `beacon-anticheat-027`.

**Where Claude must NOT decide (hard lines):**
- **Never generate or set gold/ground-truth answers.** Gold must be expert-adjudicated. Letting an
  LLM seed gold would bake model errors into the "truth" that *scores humans and gates runs* — a
  silent corruption of the entire trust core. (Also bounds the §1-I(2) feedback-loop risk.)
- **Never make the final consensus or ship decision** — that's deterministic, threshold-gated, and
  test-enforced, not model judgment.
- **Never validate a dataset's license or "no identifiable individuals"** — legal/ethical human
  sign-off only; Claude can *surface candidates*, humans *decide*.
- **Never judge age-appropriateness / distressing-imagery handling** — partner + ethics reviewer.
- **Never decide whether the game is fun** — playtested with real humans (the ≥80% gate).
- **No fabricated facts** in edu mode — every taught claim sourced and reviewed.

## 6. Ten concrete optimizations

1. **Adopt Game Accessibility Guidelines** as the primary a11y target (WCAG 2.2 AA for menus only),
   and add a **no-fail / reduced-speed / pause-anytime mode** so the calibration-miss run-end never
   gates contribution behind reaction time.
2. **Model correlated error, not just the independent binomial.** Add a hard-tile / ambiguous-tile
   class with raised N and agreement floors; treat N=5 as a floor re-baselined from M1 telemetry;
   instrument *latencyMs × combo-state vs. gold accuracy* to detect rushed judgments.
3. **Write the distribution plan now** (currently missing). Concretely: pursue an
   *embedded-mode* path (the proven model) — e.g., partner with an existing open game / itch.io
   community / a science-museum or classroom channel — and seed via HOT/Zooniverse audiences, since
   organic growth is deliberately un-juiced.
4. **Pick one v1 domain** (disaster *or* biodiversity) and fully specify its consensus + gold +
   threshold design; defer the other to `beacon-adapter-021`. Binary disaster is the better
   statistical fit for the existing math.
5. **Bound consensus-promoted gold** to a small fraction and expert-seed the initial gold pool to
   prevent the self-reinforcing-error loop; record each gold tile's answer provenance + promotion
   path (already partly specced — make the % a hard cap).
6. **Add an asset-provenance manifest** (fonts/sounds/sprites/shaders) to the compliance gate, and
   extend the CI license-check beyond npm deps to bundled assets.
7. **Make contribution consent explicit.** Surface "you're helping verify real imagery" + the
   impact receipt at session start/end, resolving the invisible-labeling vs. informed-contribution
   tension without harming play feel.
8. **Adopt freshness SLAs for disaster tiles** as first-class (the plan flags it as an open
   question): per-tile deadline, priority queue, and an explicit "unverified in window" return path
   — never ship stale time-critical labels as fresh.
9. **Assign every success metric an owner + instrument**, define the "felt like pure play" survey
   wording up front (to avoid biasing toward the 80% gate), and fix the calibration-accuracy
   denominator so it can't be gamed by scoring only strong contributors.
10. **Reduce the gold tax / sharpen anti-Sybil economics.** Consider adaptive salting (higher gold
    rate during burn-in, lower for proven contributors) to recover labeling throughput while
    keeping Sybil cost high — and publish the effective labels-per-judgment efficiency as an ops
    metric.

## 7. Parallel & perpendicular spin-offs

**Parallel (ties to existing Elyos repos):**
- **`a11y-alttext-commons`** — Beacon's imagery + Claude alt-text pipeline can *feed* the alt-text
  commons; conversely Beacon reuses that repo's a11y tooling/review standards. Two-way synergy.
- **`open-data-datasheets`** — Beacon's `ProvenanceRecord` / exported label batches are exactly the
  artifacts that repo documents; emit a **Datasheet-for-Datasets** per shipped batch using its
  templates. Direct reuse.
- **`vital-info-translations`** — share the localization pipeline for strings + content warnings;
  Beacon becomes a consumer of that translation infrastructure.
- **`proper-prepper` / disaster-prep repos** — the disaster lane's outputs and content warnings can
  cross-pollinate disaster-preparedness education.

**A reusable educational/GWAP framework (the platform play, kept inside the non-goals):** extract
`@beacon/engine` (deterministic, headless-testable sim) + `@beacon/consensus` + the adapter
interface + the no-dark-patterns lint pack into a generic **"GWAP-kit"** any Elyos project can use
to wrap an openly-licensed dataset in a quality-gated game — without Beacon itself becoming the
"general crowdsourcing platform" it forswears.

**Perpendicular reusable libraries:**
- **No-dark-patterns CI lint package** — the charter's executable invariants (FOMO-timer ban,
  IAP-import ban, push-API ban, tracker scan) as a standalone lint usable across *all* Elyos
  user-facing apps/games.
- **Provenance + tamper-evidence library** — `contentHash` + `signature` + `RetractionRecord` +
  re-export reconciliation, extracted for any Elyos open-data export (e.g., funded-lane cost
  ledgers, datasheets).

**An MCP server — "consensus-labeling MCP":** expose tools for `fetch_tile_task`,
`submit_judgment`, `get_consensus_status`, `verify_label_signature(contentHash)`, and
`pull_verified_batch` — so an agent or a partner can query label status, *cryptographically verify*
an exported label, or pull a verified batch programmatically. This also lets Elyos's own agents
participate in/monitor the loop and lets partners integrate without bespoke glue. (Strictly
read/verify + submit-judgment; never expose gold answers or ship decisions.)

## 8. Open questions (beyond the plan's own list)

1. **Distribution:** what is the concrete acquisition channel, given the deliberate refusal of
   engagement mechanics? Embed-in-an-existing-game, institutional volunteer networks, or both?
2. **Why Beacon over MapSwipe** for the prospective disaster partners who *already run MapSwipe*?
   What is the labels' marginal advantage (quality, provenance, long-tail coverage)?
3. **N for hard/rare classes:** what redundancy actually hits 95% accuracy under correlated error —
   and can the throughput model survive the real (likely >5) N?
4. **Does combo/juice measurably degrade gold accuracy?** If yes, where is the fun-vs-quality
   frontier, and which side does Beacon choose?
5. **Multi-class consensus:** if biodiversity is v1, what replaces the binary binomial design?
6. **Informed-contribution UX:** how to disclose the labor honestly without breaking the ≥80%
   "felt like play" gate — are these two goals actually compatible?
7. **Sustainability without a host audience or monetization:** what grant/institutional backing
   keeps it alive past M4 (every long-lived GWAP had an institution behind it)?

---

### Sources

- MapSwipe — https://mapswipe.org/ ; https://heigit.org/mapswipe-2-0-reimagining-how-a-mobile-app-can-support-humanitarian-response-worldwide/
- Zooniverse — https://blog.zooniverse.org/tag/crowdsourcing/ ; https://training.iiif.io/advanced_iiif/modules/zooniverse/
- Snapshot Serengeti — https://www.nature.com/articles/sdata201526 ; https://pmc.ncbi.nlm.nih.gov/articles/PMC4460915/
- EVE Project Discovery — https://www.ccpgames.com/news/2024/eve-online-launches-new-phase-of-citizen-science-initiative-to-aid-cancer
- Borderlands Science — https://www.gearboxsoftware.com/press_release/nature-biotechnology-journal-showcases-unprecedented-success-of-borderlands-science-in-driving-citizen-science-participation/ ; https://games.cs.mcgill.ca/when-borderlands-science-meets-project-discovery/
- Sea Hero Quest — https://www.ucl.ac.uk/brain-sciences/news/2016/nov/sea-hero-quest-mobile-game-which-provides-vital-data-dementia-research-goes-strength-strength
- Foldit / EyeWire — https://pmc.ncbi.nlm.nih.gov/articles/PMC6241531/ ; https://magazine.scienceconnected.org/2021/05/citizen-science-games-design-discovery/
- Tomnod — https://en.wikipedia.org/wiki/Tomnod
- GWAP / ESP Game origin — https://en.wikipedia.org/wiki/Human-based_computation_game
- Gamification vs. data quality — https://www.sciencedirect.com/science/article/abs/pii/S0747563216307762
- Human supervision in AI wildlife ID — https://theoryandpractice.citizenscienceassociation.org/articles/10.5334/cstp.752
- Game Accessibility vs WCAG/CVAA — https://www.levelaccess.com/blog/three-ways-to-level-up-video-game-accessibility/ ; https://www.researchgate.net/publication/325978045_Game_Accessibility_Guidelines_and_WCAG_20_-_A_Gap_Analysis ; https://gameaccessibilityguidelines.com/

# Beacon — PLAN.md

> Status: Draft · Version: 0.2 · Last updated: 2026-06-29 · Owner: TBD (maintainer) · Lane: donated

## Executive summary

Beacon is an original, fully open-source **game-with-a-purpose**: a fast, cooperative
**signal-tracing roguelite** for the web. Each run, the player flies a trail of light across a
dark grid, lighting *beacons* by making one-tap pattern judgments. Every judgment is secretly a
**micro-label on openly-licensed imagery** — post-disaster satellite tiles, biodiversity
camera-trap photos — and when enough independent players agree on a tile, it becomes a
**verified label** that ships to a data partner (a disaster-response org or research group).

The product hypothesis is that a genuinely *great* game can produce humanitarian and
open-science value as a by-product, **without dark patterns**. The reward loop is **mastery +
visible real-world impact**, never engagement maximization. This is an explicit, repo-stated
design constraint, not a tagline: no loot boxes, no pay-to-win, no streak-anxiety mechanics, no
engagement-maximizing data harvesting.

The hard engineering challenge is *label trust*: the game must **never** ship a label below
consensus and calibration thresholds. Quality is enforced by (a) multi-player independent
consensus and (b) invisibly salted **calibration tiles** (known-answer ground truth) that both
gate run-end and score each contributor's reliability.

This plan is honest about its biggest gap: **no data partner is yet secured**. Until a
partner accepts a real dataset and a real label handoff, Beacon is an engaging tech demo, not a
delivered good deed. Securing that partner is the critical-path dependency and is tracked as
`TO BE SECURED` throughout.

## Problem & beneficiaries

**The need (verified, generic):** Disaster-response organizations and biodiversity researchers
have a real, ongoing demand for human-verified image labels at volume — flood/damage assessment
on post-event satellite imagery, species identification on camera-trap and citizen-science
photos. Automated models help but still need human verification, especially in the long tail and
in novel events. This crowd-labeling demand is well established (prior art: Tomnod, FoldIt,
EyeWire, Zooniverse). **The general need is real; a specific committed partner for Beacon is
`TO BE SECURED`.**

**Primary beneficiaries:**
- **Disaster-response orgs / humanitarian mappers** — receive verified damage/flood labels that
  accelerate situational awareness during real events.
- **Biodiversity researchers / conservation projects** — receive verified species labels that
  reduce manual annotation backlog.

**Secondary beneficiary:** the public — a free, ad-free, high-quality open-source game, and a
transparent, replicable model for ethical games-with-a-purpose.

**Partner org:** `TO BE SECURED`. Candidate partner *categories* (not commitments) include
humanitarian open-mapping communities and open biodiversity platforms whose data is openly
licensed. No partner relationship may be claimed as real until an MOU/data-use agreement and an
accepted label handoff exist (see Milestone M4).

**Who is NOT a beneficiary:** no for-profit entity is the primary beneficiary. If a commercial
ML team ever wants the labels, they get them under the same open license as everyone else; they
get no privileged access.

## Goals and non-goals

**Goals**
- Ship a genuinely fun, replayable, accessible open-web roguelite that people *want* to play.
- Make the labeling completely invisible to play feel, yet rigorously quality-gated.
- Guarantee label quality: never emit a label below consensus + calibration thresholds.
- Prove the loop is *closed*: real verified labels accepted by a real data partner.
- Make the impact visible and honest to players ("impact receipt").
- Keep the whole stack open: MIT engine, CC-BY-SA labels, pluggable task backend.

**Non-goals**
- Not a profit/ads/IAP product. No monetization mechanics of any kind.
- Not an account/identity platform — anonymous play is a first-class, default path.
- Not a general crowdsourcing platform v1 — it is a game that *happens* to label, with one or two
  vetted datasets, not an open marketplace of arbitrary tasks.
- Not a labeler of imagery containing identifiable private individuals — ever.
- Not a replacement for expert adjudication on high-stakes calls; Beacon produces *candidate*
  verified labels under thresholds, with partner-side final say.
- Not a real-time disaster command tool; "Blackout events" point attention at urgency but Beacon
  is not in any safety-critical decision loop.

## Success metrics (outcomes)

Outcome-centric, not vanity. Baselines are zero (greenfield); targets are first-year aspirations
to be re-baselined after M1 telemetry.

| Outcome | Metric | Baseline | Target (12 mo) |
|---|---|---|---|
| Real-world impact (the headline) | Verified labels **accepted by a data partner** | 0 | ≥ 1 dataset batch formally accepted; ≥ 10k tiles verified |
| Loop is closed | Partners with a signed data-use agreement + ≥1 accepted handoff | 0 | ≥ 1 |
| Label trustworthiness | Verified-label agreement vs. partner/expert spot-check | n/a | ≥ 95% agreement on audited sample |
| Quality safety | Labels shipped below threshold | 0 | **0 (hard invariant)** |
| Calibration health | Median contributor calibration accuracy on salted tiles | n/a | ≥ 0.9 among scored contributions |
| Ethical design integrity | Dark-pattern audit findings unresolved at release | n/a | **0** |
| Labeling invisible to play feel | Blind-playtest players who report it felt like pure play (did not experience it as "doing work"/labeling) | n/a | ≥ 80% (measured at the M1 playtest; gates M1 exit) |
| Genuine engagement (not maximized) | D1 return of players who *chose* to come back | 0 | report honestly; no target that incentivizes manipulation |
| Accessibility | **Game Accessibility Guidelines** (Basic + Intermediate) on core loop; WCAG 2.2 AA on surrounding UI/menus | none | GAG Basic+Intermediate on core loop (incl. no-fail/reduced-speed mode + remappable input + colorblind-safe judgments) + WCAG 2.2 AA on menus |
| Openness | Reproducible build + published label provenance | none | 100% of shipped labels carry provenance + license |

Note: we deliberately do **not** set targets on DAU, session length, or retention as primary
goals — optimizing those is how games-with-a-purpose drift into dark patterns. We measure them
only to detect *manipulation drift*, not to maximize them.

Note (instrumentation, added v0.2): each metric needs a named **owner** and a defined
**instrument** before M1, and several need anti-gaming definitions pinned down: the "felt like
pure play" survey wording is fixed up front (a leading prompt could bias toward the 80% gate); the
"median calibration accuracy ≥ 0.9" metric specifies its **denominator** (all scored
contributions in-window, not a hand-picked subset) so it cannot be met by scoring only strong
contributors; and "verified-label agreement" is reported as *accuracy vs. expert spot-check*,
kept distinct from the consensus binomial's *probability-of-unanimous-wrong* (a different quantity
— see Consensus aggregation).

## Competitive landscape & differentiation

Beacon is a game-with-a-purpose (GWAP) in von Ahn's ESP-Game lineage (Foldit, EyeWire, Tomnod,
Zooniverse). The relevant competitors sort into four cells, and none occupies Beacon's exact one.

**Embedded-in-a-real-game GWAPs — the proven distribution model.**
- **EVE Online — Project Discovery** (CCP + scientists) and **Borderlands Science** (Gearbox +
  McGill, **>2M players**, *Nature Biotechnology*) embed citizen science inside AAA games; combined
  they report **>7M players / >900M submissions**. *Strength:* instant distribution + genuine fun.
  *Weakness:* depend on a host game's existing audience; closed/commercial; task bolted into a
  product. **Beacon's contrast:** a standalone, open game with *no host audience* — it must earn
  its players (see Distribution).
- **Sea Hero Quest** (UCL/Telekom): 4.3M players via telecom marketing. *Weakness:* closed;
  one-shot research data, not a sustained labeling loop.

**Citizen-science platforms — worthy-but-not-fun incumbents.**
- **Zooniverse** (~3M volunteers, ~100k classifications/day; Galaxy Zoo ~38 classifications/image):
  trusted, massive, redundancy-based quality — but a task UI, not a game; motivation is altruism,
  not mastery.
- **Snapshot Serengeti** (on Zooniverse): the biodiversity benchmark — **96.6% species accuracy**
  vs. expert gold at **~27 votes/image**. This is the redundancy/accuracy bar Beacon's N must
  answer to (see Consensus aggregation).

**Humanitarian satellite-tile labeling — the disaster lane's direct competitor.**
- **MapSwipe** (HOT, Red Cross, MSF, HeiGIT): swipe-classify satellite tiles for humanitarian
  mapping; **85k+ users, 1.3M km² mapped**; free + open source. **This is Beacon's most awkward
  competitor: the obvious disaster partners (HOT, Red Cross, MSF) already run their own free swipe
  tool.** Beacon must explain why its labels are *preferable*, not merely "also exist" — the answer
  is fun-driven reach beyond altruists, procurement-grade tamper-evident provenance, and long-tail
  coverage (below).
- **Tomnod** (DigitalGlobe/Maxar): 8M users on MH370, but **shut down 2019**, folded into in-house
  ML. *Cautionary tale:* the largest commercial disaster-labeling demand was *replaced by ML* —
  Beacon must show humans-in-the-loop still add **novel-event / rare-class long-tail** value, which
  the human-supervision literature supports.

**Game-first heritage.** Foldit (players designed 56 novel proteins) and EyeWire (>1,000 neurons,
6 new types) prove a *game* can produce frontier science — but with steep skill curves and small
crowds, not casual one-tap play.

**Differentiation — the single strongest claim:** *a roguelite people genuinely want to play,
fused with cryptographically auditable label trust and a build-failing no-dark-patterns + ship-gate
invariant.* No competitor has all three — MapSwipe/Zooniverse lack the game; Borderlands/Sea Hero
lack openness + standalone reach; Tomnod lacked sustainability and ethics machinery. Supporting
differentiators:
- **Mastery, not manipulation** — explicitly refusing loot boxes/streaks/notifications is both an
  ethics stance and a brand: the only GWAP whose health metric is *not* DAU.
- **Provenance you can verify yourself** — a partner can re-pull a batch and cryptographically
  confirm nothing changed (`contentHash` + `signature` + signed retractions + re-export
  reconciliation), a procurement-grade trust feature MapSwipe/Zooniverse don't offer.
- **Fun-first humanitarian labeling** — widening the contributor pool beyond altruists (von Ahn's
  "play because they enjoy it, not because they want to help").
- **Open, pluggable, domain-agnostic** — MIT engine + CC-BY-SA labels + pluggable adapters serve
  disaster *and* biodiversity, reproducible and forkable, without becoming the "general
  crowdsourcing platform" non-goal (gated to 1–2 vetted datasets).
- **Long-tail human verification where ML fails** — position against Tomnod's ML-replacement story:
  target the novel-event / rare-class tiles automated models miss.

## Distribution & player acquisition

**This is Beacon's single biggest *unaddressed* strategic risk — arguably larger than the partner
gap.** Every successful GWAP cited above rode an **existing audience**: Borderlands Science on
**2M+ AAA players**, Project Discovery on EVE's subscriber base, Sea Hero Quest on Deutsche
Telekom's marketing (4.3M), MapSwipe on the **Red Cross / MSF / HOT** volunteer network (85k).
Beacon is a **greenfield standalone web game with no channel**, and it *deliberately refuses* the
retention/notification/streak mechanics that normally drive organic growth. The throughput math
(see Open questions) **assumes players already exist**; "recruit/seed players" is not a plan. A
concrete acquisition spine is therefore a first-class deliverable, started early:

- **Embedded-mode path (the proven model).** Pursue an *embedded* distribution channel rather than
  relying on standalone organic growth: partner with an existing open game / itch.io community, or
  a science-museum / classroom channel, so Beacon rides a host audience the way Borderlands Science
  did. The deterministic, headless-testable `@beacon/engine` makes an embeddable widget feasible.
- **Institutional volunteer networks.** Seed via the same audiences that power the incumbents —
  HOT / Zooniverse / conservation volunteer pipelines — co-marketed with the secured partner. (The
  partner that provides data can also provide players.)
- **Honest, un-juiced growth.** Because organic growth is deliberately un-optimized (no streaks, no
  push), lean on intrinsic mastery, the visible impact receipt, and word-of-mouth from a genuinely
  good game — and **size partner delivery windows to the realistic, slower throughput** rather than
  assuming retention the project refuses to engineer (see the throughput-vs-ethics tension in Open
  questions).
- **Sustainability backing.** Every long-lived GWAP had an institution behind it; secure
  grant/institutional backing (not monetization) to keep the game and partner loop alive past M4.

Acquisition is owned by the Steward/Maintainer and tracked alongside partner outreach from M0.

## Scope

**In scope**
- A web game engine (TypeScript/ESM): grid/run loop, combo system, daily global seed, roguelite
  meta-progression, co-op "Blackout events", cosmetics (earned, never purchased).
- A **pluggable labeling-task backend**: task/tile schema, one or two real dataset adapters,
  judgment ingestion, calibration-tile salting, consensus aggregation, label export with
  provenance + license.
- Calibration & consensus quality machinery with hard, configurable thresholds.
- Anonymous local-first play; optional lightweight server for daily seed + global consensus.
- Impact receipt surfacing what a player's contributions verified.
- Accessibility led by the **Game Accessibility Guidelines** (Basic + Intermediate) on the core
  loop — including a **no-fail / reduced-speed / pause-anytime mode** so the calibration-miss
  "run ends" mechanic never gates contribution behind reaction time — with WCAG 2.2 AA on the
  surrounding UI/menus; i18n-ready strings.
- A written, enforced **No-Dark-Patterns design charter** + an audit checklist in CI/review.

**Out of scope (will NOT do)**
- Any monetization: ads, loot boxes, IAP, paid power, paid cosmetics, energy/lives timers.
- Streak-anxiety / FOMO mechanics, push-notification engagement loops, infinite-scroll dopamine
  patterns, or behavioral data harvesting beyond what the label task strictly needs.
- Accounts as a requirement to play; collection of personal data; third-party trackers.
- Labeling imagery with identifiable private individuals or non-open licenses.
- Shipping any label below consensus/calibration thresholds.
- Acting as a live operational input to safety-critical disaster decisions.
- A native mobile app (web first; PWA acceptable).

## Solution approach & architecture

### High-level shape

Three cleanly separated layers, mirroring Elyos's agent-neutral-core discipline:

```
+-----------------------------+        +------------------------------+
|   Game engine (client)      |        |  Labeling backend (server)   |
|   @beacon/engine (MIT)      | <----> |  @beacon/backend             |
|   - run loop / grid         |  HTTP  |  - task/tile service          |
|   - combo + juice           |  /WS   |  - judgment ingest            |
|   - daily seed (PRNG)       |        |  - calibration salting        |
|   - meta-progression        |        |  - consensus aggregation      |
|   - accessibility/i18n      |        |  - label export (provenance)  |
+-----------------------------+        +---------------+--------------+
            |                                          |
            | @beacon/protocol (shared types, schemas) |
            +------------------------------------------+
                                                       |
                                       +---------------v--------------+
                                       |  Dataset adapters (plugins)  |
                                       |  @beacon/adapter-<source>    |
                                       |  - fetch openly-licensed tiles|
                                       |  - normalize to TileTask      |
                                       |  - carry license + provenance |
                                       |  - export back to partner    |
                                       +------------------------------+
```

### Components & tech stack

- **`@beacon/protocol`** — shared TypeScript types + JSON schemas (zod/ajv) for `TileTask`,
  `Judgment`, `CalibrationTile`, `LabelRecord`, `ProvenanceRecord`. The contract between engine,
  backend, and adapters. No game or vendor logic.
- **`@beacon/engine`** — the game. TypeScript/ESM, rendered on Canvas/WebGL (lightweight, e.g.
  PixiJS or hand-rolled Canvas2D for v1) with the simulation kept deterministic and renderer-
  agnostic so it is testable headless. Deterministic seeded PRNG for the daily board. **License:
  MIT.**
- **`@beacon/backend`** — task service + judgment ingestion + consensus + export. Stateless
  request handlers over a small datastore (SQLite for M0/M1 single-node; Postgres when scaling).
  Exposes REST + optional WebSocket for Blackout co-op presence.
- **`@beacon/consensus`** — pure, side-effect-free aggregation library (separately testable):
  turns many `Judgment`s into a `LabelRecord` with a confidence and a *shipped/not-shipped*
  decision. Importable by both backend and offline batch tooling.
- **`@beacon/adapter-*`** — one adapter per dataset/partner. Each fetches openly-licensed
  imagery, normalizes to `TileTask`, attaches license + provenance, and implements export back.
  v1 ships **one** real adapter; the interface proves pluggability.
- **Tooling/deploy** — pnpm workspaces; Vite for the client; the client deploys as static assets
  (CDN/Pages); the backend deploys as a small container/serverless service. PWA optional.

### Data model (core records)

- **`TileTask`**: `id`, `datasetId`, `imageRef` (URL to openly-licensed tile + crop box),
  `question` (e.g. "flooded?"), `options[]`, `licenseId`, `provenance` (source, acquired-at,
  upstream id), `isCalibration` (never exposed to client), `goldAnswer?` (server-only).
- **`Judgment`**: `tileTaskId`, `anonContributorId` (rotating, non-PII), `choice`, `latencyMs`,
  `seedId`, `clientVersion`. No personal data, no IP stored beyond ephemeral abuse-rate-limiting.
- **`CalibrationOutcome`**: derived per contributor — rolling accuracy on salted gold tiles →
  a **reliability weight** used in consensus.
- **`LabelRecord`**: `tileTaskId`, `aggregatedChoice`, `confidence`, `nJudgments`,
  `nAgreeing`, `weightedAgreement`, `shipped` (bool), `shippedAt?`, `provenance`, `licenseId`,
  plus **export-integrity fields**: `contentHash` (canonicalized hash of the record's
  consensus-bearing fields) and `signature` (backend export-key signature over `contentHash`) so
  any post-export tampering is detectable by the partner.
- **`ProvenanceRecord`**: dataset source, license, acquisition method, partner, transformation
  log — plus `contentHash` + `signature` — travels with every exported label.
- **`RetractionRecord`** (correction/retraction type): references a prior `LabelRecord` by id +
  `contentHash`, gives a reason (e.g. consensus reversal, source license withdrawal, gold-pool
  error), an `action` (`retract | correct`), the corrected value if any, and is itself
  hashed + signed. Retractions are append-only and drive the re-export reconciliation flow
  (see Security & privacy).

### Consensus aggregation (the trust core)

1. **Calibration salting (mechanics + lifecycle):** the backend invisibly mixes known-answer
   gold tiles into each run. The client cannot distinguish them. Misjudging a gold tile ends the
   run (gameplay) *and* updates the contributor's rolling reliability weight (quality).
   Provisional mechanics (partner-tunable per dataset):
   - **Salting rate ~15%** of judgments are gold (≈ 1 in 7), enough to estimate per-contributor
     accuracy within a session without dominating play. **Target 2–4 gold tiles per run.**
   - **Answer sourcing (with feedback-loop guard):** the **initial** gold pool is
     **expert-seeded** (partner/expert-adjudicated ground truth). Already-shipped high-confidence
     consensus labels may be promoted into gold *only* up to a **hard cap** (a small bounded
     fraction of the pool), because an unbounded promotion path lets a popular *systematic* error
     promote itself into gold and then **enforce** itself on every contributor it scores. Each gold
     tile records its answer provenance **and promotion path**. Gold/ground-truth answers are
     **never** generated or set by an LLM (see Claude API leverage).
   - **Anti-memorization across the shared daily seed:** because all players share a daily seed,
     a fixed gold set would be learnable. Gold tiles are therefore drawn from a large rotating
     pool, **assigned per-contributor (not via the shared seed)**, never repeated to the same
     contributor within a window, and retired/rotated once their exposure or answer-leak risk
     crosses a threshold. Gold selection and ordering use a server-side per-contributor stream
     independent of the cosmetic daily seed.
2. **Per-contributor tile sharding (independence vs. shared seed):** the *labeling* layer assigns
   each contributor a sharded/randomized subset of real tiles drawn server-side, so two players
   on the same daily board do not judge an identical labeling sequence. Only **cosmetic** board
   elements (visual layout, theme, combo set-piece) are shared by the daily seed; the underlying
   tile→contributor assignment is independent. This preserves "everyone plays today's board"
   feel while keeping consensus votes genuinely independent (see Key decisions).
3. **Weighted consensus:** each real tile is judged by N independent contributors. Aggregate by
   reliability-weighted vote (start with weighted majority; design allows upgrade to a
   Dawid–Skene / EM estimator as data grows). Compute `weightedAgreement` and `confidence`.
4. **Hard ship gate (N = 5 is a telemetry-raised *floor*, not a fixed value):** a label is
   exported **only if** it meets *all* configured thresholds — minimum independent judgments
   **≥ 5 (a floor)**, minimum **weighted agreement ≥ 0.8**, and contributors clearing a minimum
   calibration weight.
   - *Statistical sanity check (binary tile, best case only):* at **N = 5** with an *assumed*
     per-contributor error rate of ~0.2, requiring ≥ 0.8 weighted agreement (≥ 4 of 5 effectively
     concurring) bounds **P(≥ 4 of 5 wrong) ≈ 6.7×10⁻³** — sub-1% per shipped label *before*
     calibration weighting. **Treat this as a sanity check, not a guarantee.** It holds *only* if
     errors are **independent and identically ~0.2**.
   - *Why the floor must rise — correlated error on hard tiles:* real labeling errors are **not
     independent**. On ambiguous/hard tiles, errors *correlate* (everyone reads a cloud shadow as
     flooding; everyone confuses two similar species), which inflates the true false-ship rate far
     above the binomial. The single most comparable project, **Snapshot Serengeti**, needed a
     consensus of **~27 volunteers per image** to reach **96.6% species accuracy** (and accuracy
     fell sharply on rare/hard classes). N = 5 is aggressive by roughly an order of magnitude, and
     the p = 0.2 error rate is *assumed, not measured*. Note also that the ≥ 95% target is measured
     as *accuracy vs. expert spot-check*, which is a **different quantity** from the binomial's
     *probability of a unanimous-wrong majority* — they must not be conflated.
   - *Mechanism:* N and the agreement floor are **raised from M1 telemetry** (real measured error
     rate, per-class) and a **hard-tile / ambiguous class** carries an explicitly higher N and
     agreement floor than easy tiles. We instrument **`latencyMs` × combo-state vs. gold accuracy**
     to detect "rushed" judgments (combo pressure producing careless classifications) and feed that
     into the per-tile difficulty estimate. Thresholds are **partner-overridable** per dataset and
     recorded with the label. **Below threshold ⇒ never shipped** (enforced invariant with a test
     that fails the build if violated).
5. **Multi-class (biodiversity) consensus — specified, not assumed binary.** All the reasoning
   above is for a **binary** tile ("flooded?"). Biodiversity ID is **multi-class** (Serengeti spans
   ~40 species), where gold design, agreement thresholds, the Dawid–Skene upgrade, and the
   false-ship math are materially harder: agreement must be defined over a label *set* (top-class
   plus per-class confidence), rare classes need disproportionately higher N, and "no clear
   plurality" is a first-class outcome that routes to adjudication rather than shipping a guess.
   **Decision (v1):** Beacon ships **one** domain in v1 with its consensus + gold + threshold
   design fully specified, and defers the other to a second adapter (see Roadmap / Open questions).
   **Binary disaster labeling is the better statistical fit** for the existing math and is the
   v1 default unless a biodiversity partner is secured first; if biodiversity goes first, the
   multi-class consensus design (per-class thresholds + a multi-class Dawid–Skene/EM estimator)
   must be specified before any export.
6. **Adjudication queue:** contentious tiles (split consensus) route to an extended judgment pool
   and, if still split, to partner/expert review — never auto-shipped. Because splits raise the
   *effective* N (extended pools + adjudication), the throughput model treats N = 5 as a lower
   bound on judgments-per-shipped-label, not the expected value.

### Key decisions

- **Local-first, anonymous-by-default.** Daily seed and meta-progression work without an account;
  the server is needed only for global consensus and leaderboards. This is both a privacy stance
  and a dark-pattern guard (no account funnel).
- **Shared cosmetics, sharded labels.** The daily global seed governs only the *cosmetic*,
  shared-experience layer (visual board, theme, set-pieces); the *labeling* layer assigns each
  contributor an independent, server-sharded subset of real and gold tiles. This deliberately
  resolves the otherwise-contradictory goals of "everyone plays the same daily board" and
  "consensus votes must be independent" (see Consensus aggregation).
- **Deterministic, headless-testable simulation** separated from rendering — lets AI sessions and
  CI validate gameplay and consensus logic without a browser.
- **Pluggable adapters** keep dataset/partner specifics out of the engine and backend core,
  mirroring Elyos's `adapters/` discipline.
- **Thresholds are data, not code** — partner-tunable and recorded per label for auditability.

### Claude API leverage (assistive only; never authoritative)

Claude (donated-lane tooling, always behind human/expert review) accelerates the *edges* of the
system, never its trust core:

- **Dataset triage & PII/sensitivity pre-screen.** Claude vision flags tiles that *likely* contain
  identifiable people, distressing content, or off-license watermarks **for human review** — a
  first-pass filter that makes the data/ethics gate faster, never the final call.
- **Localization / i18n at scale.** Draft translations of i18n-ready strings and content warnings
  for native-speaker review — directly widening the global contributor base (see Distribution).
- **Accessibility content.** Generate alt-text/audio descriptions for menus and the "behind the
  labels" mode, plain-language content warnings, and tutorial copy — reviewed against the Game
  Accessibility Guidelines.
- **Educational narrative.** Draft sourced "why this label matters" explainers for the optional
  `beacon-edu-025` mode — fact-checked by domain reviewers.
- **Engineering acceleration.** Draft adapter normalization code, the no-dark-patterns lint rules,
  playtest-survey synthesis, and red-team/adversarial scenarios for the anti-cheat gate.

**Hard lines — Claude must NOT decide:** it **never generates or sets gold/ground-truth answers**
(that would bake model error into the truth that scores humans and gates runs — a silent
corruption of the trust core); **never makes the consensus or ship decision** (that is
deterministic, threshold-gated, and test-enforced, not model judgment); **never** validates a
dataset's license or "no identifiable individuals" (human legal/ethical sign-off only — Claude
*surfaces candidates*, humans *decide*); never judges age-appropriateness/distressing-imagery
handling; never decides whether the game is fun (the ≥ 80% human playtest gate); and produces **no
unsourced facts** in edu mode.

## Data, licensing & compliance

This section is intentionally conservative; it is the project's highest-risk surface.

**Imagery sources — hard rules**
- Only **openly-licensed** imagery may enter Beacon. Each dataset adapter must record the source
  license (e.g. CC-BY, CC-BY-SA, CC0, or an explicit open government/partner license) and verify
  that license **permits reuse and derivative labeling** before any tile is served.
- **No imagery of identifiable private individuals.** Camera-trap/biodiversity sets must be
  human-free or face/identity-free; satellite tiles are non-identifying by nature but are still
  screened. Any dataset that could contain bystanders is rejected or pre-filtered with partner
  guidance.
- **Sensitive disaster imagery** is handled per partner guidance; Beacon defers to the
  responder org on what may be shown and how. Beyond "defer to partner", a **sensitive-content
  failsafe** governs imagery that could inadvertently capture e.g. mass-grave, casualty, or
  refugee-identifying scenes: such tiles are auto-quarantined (Claude pre-screen surfaces
  candidates; a human ethics reviewer decides) and excluded from play rather than surfaced to
  players, with the partner consulted on disposition.

**Labels / output license**
- Verified labels are released **CC-BY-SA 4.0** (as the proposal states), contributed back to the
  data partner and published openly. Every label carries its `ProvenanceRecord`.
- **Code license: MIT.** Content/strings/art: CC-BY (or CC-BY-SA where derived from SA imagery).
- **Asset-provenance manifest (open-source-game trap guard).** Every bundled third-party asset —
  fonts, sound effects, music, shaders, sprite/texture packs — is recorded in an **asset manifest**
  (source + license) and gated by the same compliance review that vets datasets. The CI
  license-check extends **beyond npm deps to bundled assets**, not just transitive package
  licenses, so an unlicensed font or sound cannot ship silently.

**Provenance model**
- Every tile records source, upstream id, license, acquisition timestamp, and transformation log.
- Every shipped label inherits provenance + records the consensus parameters used. Nothing is
  exported without complete provenance.

**Privacy / PII stance**
- No accounts required; no personal data collected. `anonContributorId` is a rotating,
  non-identifying token; no real names, emails, precise geolocation, or cross-site tracking.
- No third-party analytics/trackers. Any telemetry is aggregate, on-device-first, and disclosed.
- IPs used only ephemerally for abuse rate-limiting, never persisted to label data.

**Attribution**
- Source datasets credited per their license in-game (an "credits/sources" surface) and in every
  exported label package.

**Compliance gate:** a dataset cannot be enabled in production until a license/provenance review
(see Quality gates) signs off, recorded in the repo.

## Quality, review & risk gates

**Risk tier: medium.** (Domain accuracy + humanitarian/ethics surface; not high-stakes
individual advice, so no credentialed-expert *merge-block* on code — but **dataset enablement and
label handoff require domain/ethics review**.)

**Required reviews before a deed is "done":**
- **Code review** — standard maintainer review; `pnpm build && pnpm test && pnpm lint` green; DCO
  sign-off; changeset for user-facing changes.
- **Data/ethics review** (medium-risk gate) — for every dataset adapter and every label handoff:
  confirms open license permits derivatives, no identifiable individuals, provenance complete,
  thresholds appropriate. Reviewer must have relevant data/humanitarian-ethics skill.
- **Design review (No-Dark-Patterns)** — every gameplay/monetization-adjacent change is checked
  against the No-Dark-Patterns charter checklist; unresolved findings block release. Human review
  is backed by **machine-enforceable CI invariants** that fail the build, so the charter is not
  merely aspirational:
  - **No FOMO/countdown timers:** static check forbids streak/expiry/"ends in" timer primitives
    in gameplay/UX code.
  - **No IAP/payment code paths:** ban imports of payment/billing SDKs and purchase-flow symbols.
  - **No push-notification APIs:** forbid `Notification`, Push API, and service-worker push
    registration.
  - **No third-party trackers:** bundle/asset scan rejects any non-allowlisted third-party
    network domain (analytics/tracker/ad hosts) in the shipped client.
  These invariants live in CI and are part of the charter's enforcement, not a substitute for the
  human design review.
- **Quality/consensus review** — the ship-gate invariant is enforced by automated tests; any
  change to thresholds or aggregation requires explicit review.
- **Partner sign-off** — a label batch is only "delivered" when the data partner formally accepts
  it.

**Definition of Shipped (project-level):** a playable, deployed, open-source game that has
produced **verified labels formally accepted by a real data partner**, with provenance published,
the no-dark-patterns charter upheld, and the contribution loop demonstrably closed end-to-end.
A merged PR alone is *not* shipped.

## Roadmap & milestones

Each milestone has measurable exit criteria. M0 is a thin, honest foundation; partner-dependent
value lands in M4. **Partner-securing outreach _and_ player-acquisition (Distribution) both run in
parallel starting at M0** (both are long-lead and must not be gated behind the M2 export), tracked
as `TO BE SECURED` until the "secured" criteria in Dependencies are met.

Optimizations folded in from the competitive analysis (woven into the milestones below): adopt the
Game Accessibility Guidelines + no-fail mode (M1); instrument the fun-vs-accuracy frontier and make
contribution consent explicit (M1); treat N = 5 as a telemetry-raised floor with a hard-tile class
and pick one v1 domain (M2); add an asset-provenance manifest + bounded consensus-promoted gold
(M2); adopt freshness SLAs for disaster tiles as first-class (M2/M4); consider adaptive salting to
recover throughput (M2/backlog); write the distribution plan now (M0 onward, above).

**M0 — Foundation & charter (cold-start).**
Goal: prove the spine end-to-end with synthetic data and lock the ethics constraints in writing.
Exit criteria: monorepo + `@beacon/protocol` schemas merged; deterministic core run loop playable
in-browser on a **synthetic** tile set; `@beacon/consensus` library with the hard ship-gate +
passing invariant test; the **No-Dark-Patterns charter** + audit checklist committed; CI green
(build/test/lint). Partner-outreach kickoff begins here in parallel.

**M1 — Real game feel + daily seed.**
Goal: it's actually fun, accessible, anonymous. Exit criteria: combo/juice system; deterministic
**daily global seed** + local leaderboard (no account); **Game Accessibility Guidelines
(Basic + Intermediate) on the core loop — including a no-fail / reduced-speed / pause-anytime mode
and remappable input — plus colorblind-safe judgments and WCAG 2.2 AA on menus/UI**; headless
gameplay tests; **explicit contribution-consent UX** (a "you're helping verify real imagery"
disclosure + impact receipt surfaced at session start/end, resolving the invisible-labeling vs.
informed-contribution tension without harming play feel); **fun-vs-accuracy frontier instrumented**
(`latencyMs` × combo-state correlated with gold accuracy to detect rushed judgments, and a playtest
arm measuring label accuracy as a function of game intensity); playtest feedback logged with the
survey wording fixed up front, **including the "labeling is invisible to play feel" check — ≥ 80%
of blind playtesters report it felt like pure play (hard M1 exit gate).**

**M2 — Labeling backend + one real adapter.**
Goal: real openly-licensed imagery flows through the loop. Exit criteria: **one v1 domain chosen
and its consensus + gold + threshold design fully specified** (binary disaster is the default
statistical fit; the other domain deferred to `beacon-adapter-021`); `@beacon/backend` ingests
judgments; one real `@beacon/adapter-*` against an openly-licensed dataset (license + provenance
verified, **license/provenance review signed off**, **asset-provenance manifest + CI asset
license-check in place**); calibration salting live with **expert-seeded gold + a hard cap on
consensus-promoted gold**; consensus produces `LabelRecord`s with **N treated as a telemetry-raised
floor and a higher-N hard-tile class**; **freshness SLAs for time-sensitive disaster tiles**
(per-tile deadline + priority queue + "unverified in window" return path); **no label exported
below threshold** (verified by test); **anti-cheat/Sybil defenses gate passes before any export**
(calibration burn-in, new-contributor weight discount, coordinated-voting detection; optional
adaptive salting to recover throughput while keeping Sybil cost high).

**M3 — Roguelite meta + co-op Blackout (ethical).**
Goal: depth + cooperation without dark patterns. Exit criteria: earned-only meta-progression
(biomes/cosmetics/modifiers, zero purchase paths); co-op "Blackout event" mode that focuses
players on a region; **No-Dark-Patterns audit passes** on all new systems; impact receipt MVP.

**M4 — Partner handoff + closed loop (the deed).**
Goal: deliver real value. Exit criteria: **data partner secured** (data-use agreement); a real
verified-label batch **formally accepted** by the partner; provenance + license published;
impact receipt reflects the accepted batch; Definition of Shipped met. (Hard dependency:
partner — `TO BE SECURED`; if outreach fails, the self-publish CC-BY-SA + external-spot-check
fallback in Dependencies delivers a weaker public good but does **not** meet Definition of
Shipped.)

**M5 — Sustain & scale (post-delivery).**
Goal: keep it healthy. Exit criteria: second dataset/partner or scaled batches; ops runbook;
outcome-tracking dashboard; maintenance rotation in place.

## Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`** — ~22 tasks across M0–M5, each
mappable to an Elyos Task JSON, with per-milestone task tables, acceptance criteria for the most
important tasks, a Definition of Done per milestone, a future backlog, and a complete example
Task JSON for the first M0 task. This section is intentionally just the pointer.

## Governance, roles & stakeholders

- **Maintainer / Owner:** TBD — owns the repo, roadmap, and final merge.
- **Reviewers (rotation):** TBD — code review pool; at least one reviewer with game-design
  sensibility for the dark-pattern checks.
- **Data/ethics reviewer:** TBD — required for dataset enablement and label handoff (medium-risk
  gate); needs data-licensing + humanitarian/biodiversity-ethics literacy.
- **Steward (last-mile owner):** TBD — owns the partner relationship and the actual label
  handoff; accountable for "delivered, not merged."
- **Partner / requestor:** `TO BE SECURED` — disaster-response org and/or biodiversity research
  group that accepts labels and tunes thresholds.
- **Expert reviewers:** domain experts (humanitarian mapping / conservation) consulted for
  dataset suitability and adjudication of contentious tiles.

## Dependencies & integrations

- **External datasets:** openly-licensed post-disaster satellite imagery and/or open biodiversity
  camera-trap sets — specific source `TO BE SECURED` with the partner.
- **Data partner:** `TO BE SECURED` (critical path for M4). A partner counts as **"secured"**
  only when **all** of: (a) a signed **Data-Use Agreement** (DUA template — covers license,
  permitted derivatives, sensitive-imagery handling, attribution, retraction terms) is executed;
  (b) a **named accountable contact** at the partner exists; and (c) an **acceptance test
  passes** — the partner formally accepts a sample verified-label batch against agreed thresholds.
  Until all three hold, the relationship stays `TO BE SECURED`.
- **Fallback if no partner is secured:** self-publish the verified labels openly under **CC-BY-SA
  4.0** with full provenance, and commission an **independent external spot-check** (domain
  volunteer/expert) of a sample to substantiate the quality claim. This still delivers a public
  open-data good and exercises the full loop, though it is explicitly weaker than a partner-
  accepted handoff and does **not** by itself satisfy the project Definition of Shipped.
- **Hosting:** static CDN/Pages for the client; small container/serverless host for the backend
  (vendor-neutral; TBD).
- **Datastore:** SQLite (M0–M2) → Postgres (scale).
- **Upstream libs:** TypeScript/ESM toolchain, Vite, a Canvas/WebGL renderer (e.g. PixiJS), zod
  or ajv for schema validation — all permissively licensed.
- **Elyos pieces:** Task JSON schema (`packages/schema`), the donated-lane CLI workflow
  (prepares workspaces/PRs), governance/proposal process, good-deed definition + risk tiers.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| No data partner secured → loop never closes (demo, not deed) | High | High | Run partner-securing **in parallel from M0** (not gated behind M2 export); define explicit "secured" criteria (DUA + named contact + acceptance test, see Dependencies); design adapter interface to a generic open dataset so progress isn't blocked; fallback to self-publish CC-BY-SA + external spot-check; label as `TO BE SECURED` everywhere | Steward |
| Low-quality labels shipped to responders | Medium | Critical | Hard ship-gate invariant (consensus + calibration thresholds) enforced by automated test; partner sign-off before handoff; adjudication queue for splits | Data/ethics reviewer |
| Imagery contains identifiable individuals / non-open license | Medium | Critical | License + provenance review gate per dataset; reject sets with possible bystanders; partner guidance on sensitive imagery | Data/ethics reviewer |
| "Addictive" goal drifts into dark patterns | Medium | High | No-Dark-Patterns charter + CI/review checklist; refuse engagement-maximizing metrics as goals; design review on every relevant change | Design reviewer |
| Adversarial/bot labeling poisons consensus | Medium | High | Calibration salting + reliability weighting; rate limiting; anomaly detection; minimum independent contributors per tile | Backend maintainer |
| Disaster imagery is distressing to players | Medium | Medium | Content warnings, opt-in for disaster biomes, abstraction in the game art layer, partner guidance | Design reviewer |
| No player-acquisition channel → throughput never materializes (greenfield game with no audience, retention deliberately un-juiced) | High | High | Treat **Distribution & player acquisition** as a first-class, M0-parallel workstream (embedded-mode + institutional volunteer networks + grant backing); accept slower delivery as the cost of the ethics stance and size partner windows accordingly | Steward / Maintainer |
| Insufficient player throughput to clear the 10k-tile target in a usable window | Medium | High | Minimum-viable-throughput analysis (below) sizes the player base required; recruit/seed players (see Distribution) before partner-time-sensitive batches; degrade gracefully by prioritizing freshest tiles; treat N=5 as a floor when sizing | Maintainer |
| Time-sensitive imagery goes stale before consensus completes | Medium | High | Per-tile freshness deadline + priority queue; expire/return un-cleared time-critical tiles to the partner labeled "unverified in window" rather than shipping late as if fresh | Steward |
| Scope creep into a general crowdsourcing platform | Medium | Medium | Explicit non-goal; v1 limited to 1–2 vetted datasets | Maintainer |
| Accessibility neglected under "juice" pressure | Medium | Medium | Game Accessibility Guidelines (Basic+Intermediate) exit criterion in M1 incl. no-fail/reduced-speed mode + remappable input; WCAG 2.2 AA on menus; colorblind-safe judgments; audits in CI | Maintainer |
| Maintainer burnout / abandonment post-M4 | Medium | High | Maintenance rotation, ops runbook, outcome dashboard before declaring done | Maintainer |
| Sensitive disaster data handling missteps | Low | High | Defer to partner data-use agreement; minimize/transform; no persistence of raw sensitive imagery beyond task lifetime | Data/ethics reviewer |

## Security & privacy

- **Threat surface:** the backend (judgment ingestion, consensus, export) and the dataset
  adapters. The client is static and untrusted by design.
- **Abuse/misuse prevention:** never trust client-reported correctness; calibration is
  server-side; consensus is reliability-weighted; rate-limit by ephemeral IP; detect coordinated
  voting; quarantine suspect contributors from the consensus weight pool.
- **Anti-cheat / Sybil threat model (blocking pre-M2-export gate):** because contributors are
  anonymous and accountless, a single actor can spin up many identities to bias consensus. The
  threat model and its concrete defenses are a **blocking gate that must pass before any label is
  exported in M2**:
  - *Threats:* Sybil flooding (many cheap identities), calibration gaming, coordinated
    block-voting on targeted tiles, and weight farming via easy gold tiles.
  - *Calibration burn-in:* a new contributor's judgments **do not count toward consensus weight**
    until they clear a minimum number of gold tiles at a minimum accuracy (burn-in), so fresh
    Sybils carry zero influence until they have demonstrably labeled correctly.
  - *New-contributor weight discount:* even post-burn-in, weight ramps up with a discount factor
    over sustained accurate volume, capping the damage any single new identity can do.
  - *Coordinated-voting detection:* monitor for tiles whose minority flips correlate with
    near-simultaneous, same-shard, or same-network clusters; flag and route to adjudication,
    and quarantine implicated contributors from the weight pool.
  - These defenses are exercised by the M2 red-team checks (see TASKS `beacon-anticheat-*`) and
    the gate must be green before `beacon-export-*` may ship a label.
- **Secrets handling:** no secrets in client. Any partner/export credentials live only in the
  backend env/secret store — never in logs, receipts, committed files, or label packages (per
  Elyos rule).
- **PII:** none collected. Anonymous rotating contributor tokens; no accounts required; no
  third-party trackers; IPs ephemeral and never joined to label data.
- **Data integrity & export provenance:** server-authoritative gold answers; signed/append-only
  export log of shipped labels for audit; thresholds recorded per label. Every exported
  `LabelRecord`/`ProvenanceRecord` carries a `contentHash` + backend `signature`, so the partner
  can verify nothing was altered in transit or at rest. Corrections/retractions are issued as
  signed, append-only `RetractionRecord`s. A **re-export reconciliation flow** lets the partner
  re-pull a batch and diff by `contentHash`: unchanged labels verify, superseded labels resolve
  to their latest correction, and retracted labels are flagged for removal — no silent in-place
  edits ever.
- **Supply chain:** pinned deps, lockfile, license-check in CI to keep all deps permissive.

## Sustainability & maintenance

- **After delivery:** maintenance handled by the maintainer + reviewer rotation; a steward keeps
  the partner relationship and periodic label handoffs alive.
- **Outcome tracking:** an outcome dashboard records verified labels shipped, batches accepted,
  audited agreement rate, and dark-pattern audit status — these (not DAU) define health.
- **Cost:** static client is near-zero; backend is small. No monetization, so sustainability is
  about keeping hosting + maintenance lightweight and donation/grant-friendly.
- **Continuity:** ops runbook, reproducible builds, and documented adapter interface so new
  datasets/partners can be added by future contributors.

## Adjacent opportunities

Reusable artifacts Beacon naturally produces (noted, not committed; none expand the v1 scope or the
"general crowdsourcing platform" non-goal):

- **Ties to existing Elyos repos.** Two-way synergy with **`a11y-alttext-commons`** (Beacon's
  imagery + Claude alt-text pipeline can feed the commons; Beacon reuses its a11y standards);
  **`open-data-datasheets`** (emit a Datasheet-for-Datasets per shipped label batch from
  `ProvenanceRecord`s); **`vital-info-translations`** (share the localization pipeline for strings +
  content warnings); and disaster-prep repos (the disaster lane's outputs/content warnings
  cross-pollinate preparedness education).
- **A reusable "GWAP-kit."** Extract `@beacon/engine` + `@beacon/consensus` + the adapter interface
  + the no-dark-patterns lint pack into a generic kit any Elyos project can use to wrap an
  openly-licensed dataset in a quality-gated game — *without Beacon itself* becoming the general
  crowdsourcing platform it forswears.
- **Perpendicular libraries.** The **no-dark-patterns CI lint package** (FOMO-timer/IAP/push/tracker
  bans) usable across all Elyos user-facing apps; and the **provenance + tamper-evidence library**
  (`contentHash` + `signature` + `RetractionRecord` + re-export reconciliation) for any Elyos
  open-data export.
- **A "consensus-labeling MCP" server.** Read/verify + submit-judgment tools
  (`fetch_tile_task`, `submit_judgment`, `get_consensus_status`,
  `verify_label_signature(contentHash)`, `pull_verified_batch`) so an agent or partner can check
  status, cryptographically verify an exported label, or pull a verified batch — **strictly never
  exposing gold answers or ship decisions.**

## Open questions

- **Minimum-viable-throughput (optimistic best case):** clearing the **10k-tile** target requires
  ~10k × N judgments (≈ 50k judgments at N = 5), plus ~15% gold overhead (≈ 57.5k total). At ~120
  real judgments per active player-day (a few short runs), that is ~**420 player-days** of
  throughput — e.g. ~**60 daily-active players over a week**, or ~**30 over two weeks**. **Treat
  this as a floor/best case:** N = 5 is itself a floor (splits raise effective N; correlated error
  and the Serengeti ~27-votes/image benchmark argue real N is likely higher), the 15% gold is pure
  overhead producing no labels, and "120 judgments/player-day" presumes sustained engagement the
  project **explicitly declines to optimize**. This sizes the seed audience we must recruit (see
  Distribution) before promising a partner a delivery window; re-baseline against M1 telemetry and
  the actual N chosen.
- **Throughput-vs-ethics tension (explicit resolution needed):** delivery depends on throughput,
  throughput depends on retention, and the project *refuses* to optimize retention. The honest
  resolution is to **accept slower delivery as the cost of the ethics stance and size partner
  windows accordingly** — to be confirmed with the first partner, not buried as a footnote.
- **Label freshness for time-sensitive imagery:** post-disaster tiles have a useful-life window;
  how do we set per-tile freshness deadlines, prioritize the freshest/most-urgent tiles, and
  decide when an un-cleared time-critical tile is returned to the partner as "unverified in
  window" rather than shipped late?
- Which dataset/partner is first — disaster (satellite) or biodiversity (camera-trap)? Each has
  different sensitivity and threshold needs.
- Exact threshold defaults per dataset (min judgments, min weighted agreement, min calibration
  weight) — must be set *with* the partner.
- Consensus algorithm progression: weighted majority for v1; when (and whether) to move to
  Dawid–Skene/EM, given cold-start data sparsity.
- How much of meta-progression can be earned offline vs. needs the server, given anonymous play.
- Renderer choice (PixiJS vs. hand-rolled Canvas2D) and its accessibility implications.
- How to present distressing disaster imagery responsibly within a "fun" frame.
- Is CC-BY-SA the right label license for every partner, or do some require CC-BY/CC0?
- **Distribution channel:** what is the concrete acquisition channel given the deliberate refusal
  of engagement mechanics — embed-in-an-existing-game, institutional volunteer networks, or both?
- **Why Beacon over MapSwipe** for prospective disaster partners who *already run MapSwipe*? What is
  the labels' marginal advantage (quality, tamper-evident provenance, long-tail coverage)?
- **N for hard/rare classes:** what redundancy actually hits 95% accuracy under *correlated* error,
  and can the throughput model survive the real (likely > 5) N?
- **Does combo/juice measurably degrade gold accuracy?** If so, where is the fun-vs-quality
  frontier, and which side does Beacon choose?
- **Multi-class consensus:** if biodiversity is v1, what replaces the binary binomial design
  (per-class thresholds, multi-class Dawid–Skene, rare-class redundancy)?
- **Informed-contribution UX:** can honest labor disclosure and the ≥ 80% "felt like play" gate
  actually coexist, or do they trade off?
- **Sustainability without a host audience or monetization:** what grant/institutional backing keeps
  Beacon alive past M4 (every long-lived GWAP had an institution behind it)?

## References

- Proposal: `governance/proposals/beacon-game.md`
- Elyos work rules: `CLAUDE.md`
- Good-deed definition + risk tiers: `docs/good-deed-definition.md`
- Task JSON schema: `packages/schema/src/schemas.ts`
- Backlog: `planning/projects/beacon-game/TASKS.md`
- Competitive & improvement analysis (cited sources): `COMPETITIVE-ANALYSIS.md`
- Prior art / competitors (context, not dependencies): MapSwipe, Snapshot Serengeti / Zooniverse,
  EVE Project Discovery, Borderlands Science, Sea Hero Quest, Tomnod (cautionary), Foldit, EyeWire,
  von Ahn's ESP Game.

## Changelog

### v0.2 (analysis merged)

Merged `COMPETITIVE-ANALYSIS.md` (2026-06-29 analyst pass) into the plan. Surgical/additive; no
guardrails weakened, no facts invented.

- **Applied fixes.** (1) Added a **Distribution & player acquisition** section — the biggest
  unaddressed risk: every successful GWAP rode an existing audience while Beacon is greenfield with
  un-juiced growth; the throughput math assumed players exist. (2) Recast **N = 5 as a
  telemetry-raised floor**, not a fixed value — the binomial assumes independent errors, but errors
  correlate on hard tiles (Snapshot Serengeti needed ~27 votes/image for 96.6%); added a hard-tile
  class, latency×combo instrumentation, and the agreement-vs-accuracy distinction; **extended the
  consensus design to the multi-class biodiversity case** and made v1 pick one domain. (3) Switched
  the primary accessibility standard from **WCAG 2.2 AA to the Game Accessibility Guidelines**
  (WCAG kept for menus/UI) and added a **no-fail / reduced-speed / pause-anytime mode**.
- **Integrated strategy.** Added **Competitive landscape & differentiation** (MapSwipe, EVE/
  Borderlands embedded distribution, Zooniverse/Serengeti redundancy, Tomnod cautionary tale;
  differentiator = a roguelite people want to play + cryptographically auditable label trust + a
  no-dark-patterns ship-gate); folded **Claude API leverage** into architecture (PII/sensitivity
  pre-screen, i18n, edu narrative — Claude never sets gold answers or makes the ship decision);
  folded **optimizations** into the Roadmap (asset-provenance manifest, bounded consensus-promoted
  gold, explicit contribution consent, freshness SLAs, metric owners/instruments, adaptive salting);
  added an **Adjacent opportunities** section (GWAP-kit, no-dark-patterns lint package, provenance
  library, consensus-labeling MCP, ties to sibling Elyos repos); merged the analysis's **open
  questions** and added a distribution/throughput risk row.
- **Preserved** all guardrails, vision, structure, and valid content; bumped v0.1 → v0.2.

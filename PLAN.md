# Beacon — PLAN.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

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
| Genuine engagement (not maximized) | D1 return of players who *chose* to come back | 0 | report honestly; no target that incentivizes manipulation |
| Accessibility | WCAG 2.2 AA conformance on core loop | none | AA on core loop + colorblind-safe judgments |
| Openness | Reproducible build + published label provenance | none | 100% of shipped labels carry provenance + license |

Note: we deliberately do **not** set targets on DAU, session length, or retention as primary
goals — optimizing those is how games-with-a-purpose drift into dark patterns. We measure them
only to detect *manipulation drift*, not to maximize them.

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
- Accessibility (WCAG 2.2 AA on the core loop), i18n-ready strings.
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
  `nAgreeing`, `weightedAgreement`, `shipped` (bool), `shippedAt?`, `provenance`, `licenseId`.
- **`ProvenanceRecord`**: dataset source, license, acquisition method, partner, transformation
  log — travels with every exported label.

### Consensus aggregation (the trust core)

1. **Calibration salting:** the backend invisibly mixes known-answer gold tiles into each run.
   The client cannot distinguish them. Misjudging a gold tile ends the run (gameplay) *and*
   updates the contributor's rolling reliability weight (quality).
2. **Weighted consensus:** each real tile is judged by N independent contributors. Aggregate by
   reliability-weighted vote (start with weighted majority; design allows upgrade to a
   Dawid–Skene / EM estimator as data grows). Compute `weightedAgreement` and `confidence`.
3. **Hard ship gate:** a label is exported **only if** it meets *all* configured thresholds —
   minimum independent judgments (e.g. ≥ 5), minimum weighted agreement (e.g. ≥ 0.8), and the
   contributors clearing a minimum calibration weight. Thresholds are partner-tunable per
   dataset and recorded with the label. **Below threshold ⇒ never shipped** (enforced invariant
   with a test that fails the build if violated).
4. **Adjudication queue:** contentious tiles (split consensus) route to an extended judgment pool
   and, if still split, to partner/expert review — never auto-shipped.

### Key decisions

- **Local-first, anonymous-by-default.** Daily seed and meta-progression work without an account;
  the server is needed only for global consensus and leaderboards. This is both a privacy stance
  and a dark-pattern guard (no account funnel).
- **Deterministic, headless-testable simulation** separated from rendering — lets AI sessions and
  CI validate gameplay and consensus logic without a browser.
- **Pluggable adapters** keep dataset/partner specifics out of the engine and backend core,
  mirroring Elyos's `adapters/` discipline.
- **Thresholds are data, not code** — partner-tunable and recorded per label for auditability.

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
  responder org on what may be shown and how.

**Labels / output license**
- Verified labels are released **CC-BY-SA 4.0** (as the proposal states), contributed back to the
  data partner and published openly. Every label carries its `ProvenanceRecord`.
- **Code license: MIT.** Content/strings/art: CC-BY (or CC-BY-SA where derived from SA imagery).

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
  against the No-Dark-Patterns charter checklist; unresolved findings block release.
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
value lands in M4.

**M0 — Foundation & charter (cold-start).**
Goal: prove the spine end-to-end with synthetic data and lock the ethics constraints in writing.
Exit criteria: monorepo + `@beacon/protocol` schemas merged; deterministic core run loop playable
in-browser on a **synthetic** tile set; `@beacon/consensus` library with the hard ship-gate +
passing invariant test; the **No-Dark-Patterns charter** + audit checklist committed; CI green
(build/test/lint).

**M1 — Real game feel + daily seed.**
Goal: it's actually fun, accessible, anonymous. Exit criteria: combo/juice system; deterministic
**daily global seed** + local leaderboard (no account); WCAG 2.2 AA on core loop incl.
colorblind-safe judgments; headless gameplay tests; playtest feedback logged.

**M2 — Labeling backend + one real adapter.**
Goal: real openly-licensed imagery flows through the loop. Exit criteria: `@beacon/backend`
ingests judgments; one real `@beacon/adapter-*` against an openly-licensed dataset (license +
provenance verified, **license/provenance review signed off**); calibration salting live;
consensus produces `LabelRecord`s; **no label exported below threshold** (verified by test).

**M3 — Roguelite meta + co-op Blackout (ethical).**
Goal: depth + cooperation without dark patterns. Exit criteria: earned-only meta-progression
(biomes/cosmetics/modifiers, zero purchase paths); co-op "Blackout event" mode that focuses
players on a region; **No-Dark-Patterns audit passes** on all new systems; impact receipt MVP.

**M4 — Partner handoff + closed loop (the deed).**
Goal: deliver real value. Exit criteria: **data partner secured** (data-use agreement); a real
verified-label batch **formally accepted** by the partner; provenance + license published;
impact receipt reflects the accepted batch; Definition of Shipped met. (Hard dependency:
partner — `TO BE SECURED`.)

**M5 — Sustain & scale (post-delivery).**
Goal: keep it healthy. Exit criteria: second dataset/partner or scaled batches; ops runbook;
outcome-tracking dashboard; maintenance rotation in place.

## Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`** — ~16 tasks across M0–M5, each
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
- **Data partner:** `TO BE SECURED` (critical path for M4).
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
| No data partner secured → loop never closes (demo, not deed) | High | High | Treat partner outreach as M0-parallel critical path; design adapter interface to a generic open dataset so progress isn't blocked; label as `TO BE SECURED` everywhere | Steward |
| Low-quality labels shipped to responders | Medium | Critical | Hard ship-gate invariant (consensus + calibration thresholds) enforced by automated test; partner sign-off before handoff; adjudication queue for splits | Data/ethics reviewer |
| Imagery contains identifiable individuals / non-open license | Medium | Critical | License + provenance review gate per dataset; reject sets with possible bystanders; partner guidance on sensitive imagery | Data/ethics reviewer |
| "Addictive" goal drifts into dark patterns | Medium | High | No-Dark-Patterns charter + CI/review checklist; refuse engagement-maximizing metrics as goals; design review on every relevant change | Design reviewer |
| Adversarial/bot labeling poisons consensus | Medium | High | Calibration salting + reliability weighting; rate limiting; anomaly detection; minimum independent contributors per tile | Backend maintainer |
| Disaster imagery is distressing to players | Medium | Medium | Content warnings, opt-in for disaster biomes, abstraction in the game art layer, partner guidance | Design reviewer |
| Scope creep into a general crowdsourcing platform | Medium | Medium | Explicit non-goal; v1 limited to 1–2 vetted datasets | Maintainer |
| Accessibility neglected under "juice" pressure | Medium | Medium | WCAG 2.2 AA exit criterion in M1; colorblind-safe judgments; audits in CI | Maintainer |
| Maintainer burnout / abandonment post-M4 | Medium | High | Maintenance rotation, ops runbook, outcome dashboard before declaring done | Maintainer |
| Sensitive disaster data handling missteps | Low | High | Defer to partner data-use agreement; minimize/transform; no persistence of raw sensitive imagery beyond task lifetime | Data/ethics reviewer |

## Security & privacy

- **Threat surface:** the backend (judgment ingestion, consensus, export) and the dataset
  adapters. The client is static and untrusted by design.
- **Abuse/misuse prevention:** never trust client-reported correctness; calibration is
  server-side; consensus is reliability-weighted; rate-limit by ephemeral IP; detect coordinated
  voting; quarantine suspect contributors from the consensus weight pool.
- **Secrets handling:** no secrets in client. Any partner/export credentials live only in the
  backend env/secret store — never in logs, receipts, committed files, or label packages (per
  Elyos rule).
- **PII:** none collected. Anonymous rotating contributor tokens; no accounts required; no
  third-party trackers; IPs ephemeral and never joined to label data.
- **Data integrity:** server-authoritative gold answers; signed/append-only export log of shipped
  labels for audit; thresholds recorded per label.
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

## Open questions

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

## References

- Proposal: `governance/proposals/beacon-game.md`
- Elyos work rules: `CLAUDE.md`
- Good-deed definition + risk tiers: `docs/good-deed-definition.md`
- Task JSON schema: `packages/schema/src/schemas.ts`
- Backlog: `planning/projects/beacon-game/TASKS.md`
- Prior art (context, not dependencies): Tomnod, FoldIt, EyeWire, Zooniverse.

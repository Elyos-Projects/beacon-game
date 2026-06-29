# Beacon — TASKS.md

> Status: Draft · Version: 0.2 · Last updated: 2026-06-29 · Owner: TBD (maintainer) · Lane: donated

Backlog for **Beacon** (slug: `beacon-game`), an open-source cooperative signal-tracing roguelite
that produces consensus-verified labels on openly-licensed imagery for disaster-response and
biodiversity partners. See `PLAN.md` for full context.

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- **id** — stable slug `beacon-<area>-NNN` (e.g. `beacon-engine-001`).
- **title** — the task title in the milestone table.
- **project** — `beacon-game`.
- **type** — one of `code | research | writing | data | design-spec | maintenance`.
- **lane** — `donated` (default for this project; no funded tasks planned. Any `funded` task must
  add `fundedBudgetUsd`).
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["software","games","humanitarian","open-science"]`.
- **riskTier** — `low | medium | high`. Dataset/handoff/consensus tasks are `medium`; pure
  engine/UI tasks are `low`.
- **urgent** — boolean (true only for live-disaster Blackout response work).
- **deliverable** — `pr | dataset | document | translation`.
- **tokenEstimate** — `small | medium | large` (maps to the Size column).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — partner/steward; `TO BE SECURED` where unknown.
- **verifiedNeed** — `true` only once a specific partner/need is confirmed; otherwise `false`.
- **outputLicense** — `MIT` for code, `CC-BY-SA-4.0` for labels/datasets, `CC-BY-4.0` for docs.

Size legend: small ≈ tokenEstimate `small`, med ≈ `medium`, large ≈ `large`.

---

## Milestone M0 — Foundation & charter (cold-start)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| beacon-repo-001 | Monorepo + pnpm workspaces + CI (build/test/lint) | code | small | low | pr | — | Maintainer |
| beacon-protocol-002 | `@beacon/protocol` shared types + schemas (TileTask/Judgment/LabelRecord/Provenance + contentHash/signature + RetractionRecord) | code | medium | low | pr | beacon-repo-001 | Maintainer |
| beacon-engine-003 | Deterministic core run loop + grid on synthetic tiles (headless-testable) | code | large | low | pr | beacon-protocol-002 | Maintainer |
| beacon-consensus-004 | `@beacon/consensus` library + hard ship-gate invariant test | code | medium | medium | pr | beacon-protocol-002 | Data/ethics reviewer |
| beacon-charter-005 | No-Dark-Patterns design charter + audit checklist | writing | small | low | document | beacon-repo-001 | Design reviewer |

**Acceptance criteria — key tasks**

- **beacon-engine-003** (core run loop)
  - Simulation runs headless (no DOM) and is fully deterministic given a seed.
  - One-tap/swipe judgment chains a combo; a wrong calibration judgment ends the run.
  - Renders in-browser on a synthetic tile set; no real imagery yet.
  - Unit tests cover combo state machine + run-end on calibration miss; CI green.
- **beacon-consensus-004** (consensus + ship gate)
  - Pure functions aggregate `Judgment[]` → `LabelRecord` with confidence + weighted agreement.
  - Reliability weighting from calibration outcomes is applied.
  - A label is marked `shipped:true` **only** when all configured thresholds pass.
  - An invariant test **fails the build** if any below-threshold label is ever marked shipped.
- **beacon-charter-005** (No-Dark-Patterns charter)
  - Enumerates banned patterns (loot boxes, pay-to-win, streak-anxiety, FOMO, engagement-
    maximizing harvesting) and the allowed reward model (mastery + visible impact).
  - Provides a reviewable checklist applied to every gameplay/UX PR.

**M0 Definition of Done:** monorepo + CI green; protocol schemas + consensus library merged with
the ship-gate invariant test passing; core loop playable on synthetic data; charter + checklist
committed and wired into the review process.

---

## Milestone M1 — Real game feel + daily seed

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| beacon-engine-006 | Combo "juice" + audiovisual feedback (no manipulative timers) | code | medium | low | pr | beacon-engine-003 | Design reviewer |
| beacon-seed-007 | Deterministic daily global seed + local leaderboard (no account) | code | medium | low | pr | beacon-engine-003 | Maintainer |
| beacon-a11y-008 | Game Accessibility Guidelines (Basic+Intermediate) on core loop incl. no-fail/reduced-speed mode + remappable input + colorblind-safe judgments; WCAG 2.2 AA on menus | code | medium | low | pr | beacon-engine-006 | Maintainer |
| beacon-playtest-009 | Playtest protocol + feedback log (fun without dark patterns) | research | small | low | document | beacon-engine-006 | Design reviewer |

**Acceptance criteria — key tasks**

- **beacon-seed-007** (daily seed + leaderboard)
  - Same daily seed yields the identical board for all players (deterministic PRNG).
  - Local leaderboard persists without any account or server identity.
  - No streak/FOMO mechanics; missing a day carries no penalty.
- **beacon-a11y-008** (accessibility)
  - Core loop meets the **Game Accessibility Guidelines** (Basic + Intermediate); surrounding
    UI/menus meet WCAG 2.2 AA; judgments distinguishable without color alone.
  - A **no-fail / reduced-speed / pause-anytime mode** exists so the calibration-miss run-end never
    gates contribution behind reaction time; input is **remappable**.
  - Keyboard + pointer + touch input all supported; motion-reduction and flashing limits respected.

**M1 Definition of Done:** game is demonstrably fun and accessible; daily seed + local
leaderboard work anonymously; **Game Accessibility Guidelines (Basic+Intermediate) on the core loop
incl. no-fail mode + WCAG 2.2 AA on menus** verified; **explicit contribution-consent UX** present
and the **fun-vs-accuracy frontier instrumented** (latency×combo vs. gold accuracy); playtest
feedback logged with survey wording fixed up front; charter checklist passed on all M1 PRs.

---

## Milestone M2 — Labeling backend + one real adapter

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| beacon-backend-010 | `@beacon/backend` judgment ingestion service + per-contributor tile sharding + rate limiting | code | medium | medium | pr | beacon-consensus-004 | Data/ethics reviewer |
| beacon-calibration-026 | Server-side gold pool + calibration salting & lifecycle (anti-memorization across shared seed) | code | medium | medium | pr | beacon-backend-010, beacon-consensus-004 | Data/ethics reviewer |
| beacon-anticheat-027 | Anti-cheat/Sybil defenses + pre-export red-team gate (blocking) | code | medium | medium | pr | beacon-backend-010, beacon-calibration-026 | Data/ethics reviewer |
| beacon-dataset-011 | Source & license/provenance vetting for first openly-licensed dataset | data | medium | medium | dataset | beacon-protocol-002 | Data/ethics reviewer |
| beacon-adapter-012 | First real `@beacon/adapter-*` — fetch + normalize to `TileTask` | code | medium | medium | pr | beacon-dataset-011, beacon-protocol-002 | Data/ethics reviewer |
| beacon-adapter-028 | Adapter provenance attachment + export-back-to-partner path | code | medium | medium | pr | beacon-adapter-012, beacon-export-013 | Data/ethics reviewer |
| beacon-export-013 | Label export w/ provenance, content-hash + signature, CC-BY-SA packaging (gated by anti-cheat) | code | medium | medium | dataset | beacon-adapter-012, beacon-calibration-026, beacon-anticheat-027 | Data/ethics reviewer |

**Acceptance criteria — key tasks**

- **beacon-backend-010** (ingestion service)
  - Ingests judgments via REST/WS; persists to datastore; never trusts client-reported correctness.
  - **Per-contributor tile sharding:** assigns each contributor an independent server-side subset
    of real tiles; only the cosmetic daily seed is shared, the labeling assignment is not.
  - **Freshness SLAs for time-sensitive (disaster) tiles:** per-tile deadline + priority queue;
    un-cleared time-critical tiles are returned to the partner as "unverified in window" rather
    than shipped late as if fresh.
  - Ephemeral-IP rate-limiting; no PII persisted.
- **beacon-calibration-026** (gold pool + calibration salting)
  - Gold answers held **server-side only**, never sent to the client; salting rate ~15% with
    ~2–4 gold tiles per run; each gold tile records its answer provenance **and promotion path**.
  - **Initial gold is expert-seeded**; consensus-promoted gold is capped at a small bounded
    fraction of the pool (prevents a systematic error self-promoting into gold and enforcing
    itself); gold answers are **never** generated/set by an LLM.
  - **Anti-memorization:** gold drawn from a large rotating pool, assigned per-contributor
    independent of the shared daily seed, never repeated within a window, retired on exposure.
  - Updates per-contributor rolling reliability weight from gold outcomes.
- **beacon-anticheat-027** (Sybil/anti-cheat gate — blocking)
  - **Calibration burn-in:** a new contributor carries **zero** consensus weight until clearing a
    minimum gold count at a minimum accuracy.
  - **New-contributor weight discount** ramps weight up with sustained accurate volume.
  - **Coordinated-voting detection** flags correlated/same-shard/same-network clusters → routes to
    adjudication and quarantines implicated contributors from the weight pool.
  - Red-team test suite passes; **gate must be green before `beacon-export-013` ships any label.**
- **beacon-dataset-011** (dataset vetting)
  - License confirmed to permit reuse **and derivative labeling**; recorded in repo.
  - Verified free of identifiable private individuals; provenance documented; **sensitive-content
    failsafe** applied (mass-grave/casualty/refugee-identifying tiles quarantined, not surfaced).
  - **Asset-provenance manifest** for bundled fonts/sounds/sprites/shaders in place and the CI
    license-check extended beyond npm deps to bundled assets.
  - Data/ethics reviewer sign-off recorded before the dataset is enabled.
  - **One v1 domain chosen** (binary disaster is the default statistical fit); multi-class
    biodiversity deferred to `beacon-adapter-021` unless its consensus design is specified first.
- **beacon-export-013** (label export)
  - Only threshold-passing labels exported; each carries complete provenance + CC-BY-SA license +
    **`contentHash` + `signature`** for tamper-evidence.
  - Supports signed, append-only **`RetractionRecord`** (correction/retraction) and a
    **re-export reconciliation** flow that diffs a re-pulled batch by `contentHash`.
  - Append-only audit log of shipped labels with the consensus parameters used.

**M2 Definition of Done:** real openly-licensed imagery flows end-to-end; calibration + consensus
live; **anti-cheat/Sybil gate green before export**; dataset license/provenance review signed off;
**no label exported below threshold** (test-verified); exportable label package produced (with
integrity hashes/signatures; not yet partner-accepted).

---

## Milestone M3 — Roguelite meta + co-op Blackout (ethical)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| beacon-meta-014 | Earned-only meta-progression (biomes/cosmetics/modifiers; zero purchase paths) | code | medium | low | pr | beacon-seed-007 | Design reviewer |
| beacon-blackout-015 | Co-op "Blackout event" mode (region focus; opt-in; content warnings) | code | medium | medium | pr | beacon-backend-010, beacon-meta-014 | Design reviewer |
| beacon-impact-016 | Impact receipt MVP (honest, what your labels verified) | code | small | low | pr | beacon-export-013 | Maintainer |

**Acceptance criteria — key tasks**

- **beacon-meta-014** (meta-progression)
  - All unlocks earned through play; no purchase/IAP/paid-power path exists anywhere.
  - Passes the No-Dark-Patterns audit checklist.
- **beacon-blackout-015** (Blackout co-op)
  - Focuses players on a real region's tiles; disaster biomes are opt-in with content warnings.
  - Cooperative, not competitive-pressure; no manipulative urgency framing beyond genuine need.

**M3 Definition of Done:** depth + co-op shipped with zero monetization mechanics; No-Dark-Patterns
audit passes on all new systems; impact receipt MVP reflects real exported labels.

---

## Milestone M4 — Partner handoff + closed loop (the deed)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| beacon-distribution-029 | Player-acquisition plan + seed channel (embedded-mode + institutional volunteer networks + grant backing) | research | medium | medium | document | — (run in parallel from M0) | Steward + Maintainer |
| beacon-partner-017 | Secure data partner + data-use agreement + threshold tuning | research | medium | medium | document | — (run in parallel from M0) | Steward |
| beacon-handoff-018 | First verified-label batch formally accepted by partner | data | medium | medium | dataset | beacon-partner-017, beacon-impact-016 | Steward + Data/ethics reviewer |

**Acceptance criteria — key tasks**

- **beacon-distribution-029** (player acquisition — **runs in parallel from M0**, the biggest
  unaddressed strategic risk; greenfield game with no audience and deliberately un-juiced growth)
  - Names a concrete acquisition channel: an **embedded-mode** path (existing open game / itch.io /
    science-museum / classroom) and/or **institutional volunteer networks** (HOT / Zooniverse /
    conservation pipelines), co-marketed with the secured partner.
  - Sizes the seed audience against the throughput model (treating N = 5 as a floor) and the
    explicit "accept slower delivery as the cost of the ethics stance" resolution.
  - Identifies grant/institutional backing for post-M4 sustainability (no monetization).
- **beacon-partner-017** (secure partner — **runs in parallel from M0**, not gated behind M2)
  - "Secured" requires **all three**: signed **DUA** (from the DUA template), a **named
    accountable partner contact**, and a passing **acceptance test** on a sample label batch.
  - Per-dataset thresholds (min judgments, min weighted agreement, min calibration weight) set
    jointly and recorded; provisional defaults (**N ≥ 5 as a telemetry-raised floor**, weighted
    agreement ≥ 0.8) are partner-overridable, with a higher-N hard-tile/rare-class band and N
    re-baselined from M1 telemetry under correlated error (cf. Serengeti ~27 votes/image).
  - **Fallback** if no partner is secured: self-publish labels CC-BY-SA + commission an external
    spot-check (does not satisfy Definition of Shipped).
- **beacon-handoff-018** (closed loop)
  - A verified-label batch is delivered and **formally accepted** by the partner.
  - Provenance + CC-BY-SA license published; impact receipt reflects the accepted batch.
  - Spot-check agreement vs. partner/expert sample ≥ target; below-threshold labels: zero.

**M4 Definition of Done:** Project-level **Definition of Shipped** met — deployed open-source game
has produced verified labels formally accepted by a real partner, provenance published, loop
demonstrably closed, charter upheld.

---

## Milestone M5 — Sustain & scale (post-delivery)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| beacon-ops-019 | Ops runbook + outcome-tracking dashboard + maintenance rotation | maintenance | medium | low | document | beacon-handoff-018 | Maintainer |

**Acceptance criteria — beacon-ops-019**
- Runbook covers deploy, incident response, dataset enablement, and partner handoff.
- Dashboard tracks verified labels shipped, batches accepted, audited agreement rate, and
  dark-pattern audit status (not DAU).
- Named maintenance rotation in place.

**M5 Definition of Done:** project is sustainably maintained with outcomes tracked and a rotation
owning it after the first delivery.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| beacon-consensus-020 | Upgrade consensus to Dawid–Skene / EM estimator | code | large | medium | pr | When data volume supports it; sparsity-aware |
| beacon-adapter-021 | Second dataset adapter (other domain than first) | code | large | medium | pr | Proves pluggability across domains; if biodiversity, must specify the **multi-class consensus + gold + per-class threshold** design before export |
| beacon-salting-031 | Adaptive salting (higher gold rate during burn-in, lower for proven contributors) | code | medium | medium | pr | Recovers labeling throughput while keeping Sybil cost high; publish labels-per-judgment efficiency as an ops metric |
| beacon-i18n-022 | Internationalization + community translations | writing | medium | low | translation | i18n-ready strings already in engine |
| beacon-pwa-023 | PWA / offline play packaging | code | medium | low | pr | Web-first; no native app |
| beacon-adversarial-024 | Adversarial-labeling red-team + detection hardening | research | medium | medium | document | Post-M2 hardening that extends the M2 anti-cheat gate (beacon-anticheat-027); stress consensus + calibration defenses |
| beacon-edu-025 | Educational "behind the labels" mode (open-science literacy) | code | small | low | pr | Secondary education benefit |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task (`beacon-repo-001`):

```json
{
  "id": "beacon-repo-001",
  "title": "Monorepo + pnpm workspaces + CI (build/test/lint)",
  "project": "beacon-game",
  "type": "code",
  "lane": "donated",
  "priority": "high",
  "domain": ["software", "games", "open-science"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "pr",
  "tokenEstimate": "small",
  "status": "open",
  "context": "Beacon is a cooperative signal-tracing roguelite whose one-tap judgments are secretly consensus-verified micro-labels on openly-licensed imagery for disaster-response and biodiversity partners. This is the cold-start foundation task: there is no repo scaffold yet. The codebase must follow Elyos conventions: TypeScript, ESM, pnpm workspaces, with a strict separation between the agent-neutral game/backend core and dataset-specific adapters.",
  "objective": "Scaffold the pnpm-workspace monorepo with placeholder packages (@beacon/protocol, @beacon/engine, @beacon/consensus, @beacon/backend, @beacon/adapter-*) and a CI pipeline running build, test, and lint, so all later Beacon tasks have a green, reproducible baseline.",
  "acceptanceCriteria": [
    "pnpm workspace with packages: @beacon/protocol, @beacon/engine, @beacon/consensus, @beacon/backend, and an example @beacon/adapter-* stub",
    "TypeScript + ESM configured consistently across packages; shared tsconfig base",
    "pnpm build && pnpm test && pnpm lint all pass on a clean checkout",
    "CI workflow runs build/test/lint on push and PR and is green",
    "MIT LICENSE for code; repo README states the No-Dark-Patterns and open-license constraints; DCO sign-off configured"
  ],
  "resources": [
    "planning/projects/beacon-game/PLAN.md",
    "governance/proposals/beacon-game.md",
    "CLAUDE.md",
    "packages/schema/src/schemas.ts"
  ],
  "output": "A PR adding the Beacon monorepo scaffold (pnpm workspaces, package stubs, shared TS/ESM config, lint setup) and a green CI workflow for build/test/lint.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "MIT"
}
```

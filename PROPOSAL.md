# Proposal: Beacon — an original "game with a purpose"

**Proposer:** jdev1977 (drafted by Claude Code)
**Date:** 2026-06-28
**Domain(s):** software, games, humanitarian, open-science, education
**Lane:** donated
**Requestor / beneficiary:** disaster-response orgs & biodiversity researchers (data); the public (a free, great game)  ·  **Verified need:** yes (crowd image-labeling demand is real and ongoing)

## The idea (the "most addictive" part, done ethically)
**Beacon** is a fast, beautiful, cooperative **signal-tracing roguelite**. Each run, you fly a
trail of light across a dark grid, lighting **beacons** by making one-tap pattern judgments. Every
judgment is actually a micro-label on real openly-licensed imagery (e.g., "flood-damaged vs intact"
on post-disaster satellite tiles, or "species A vs B" on biodiversity camera-trap photos). When
enough players independently agree on a tile, it becomes a verified label that ships to responders
and scientists.

### Core loop (10 seconds, endlessly repeatable)
1. A tile lights up; you make a quick judgment (swipe/tap).
2. Correct, confident, fast judgments **chain a combo** → your light-trail grows and speeds up.
3. The run ends when you misjudge a "calibration" tile (known-answer, salted in invisibly).
4. Score posts to the daily seed leaderboard; you bank XP toward meta-unlocks.

### Why it's compelling — without dark patterns
- **Daily global seed** (everyone plays the same board → water-cooler comparison, à la a daily puzzle).
- **Combo "juice"**: satisfying audiovisual feedback, near-miss tension, "one more run."
- **Roguelite meta-progression**: unlock new biomes/regions, light-trail cosmetics, modifiers.
- **Co-op "Blackout events"**: when a real disaster hits, the community races to light that region
  first — engagement is pointed at genuine urgency, and players *see* their impact.
- **Ethics line we hold:** no loot boxes, no pay-to-win, no manipulative streak-anxiety, no
  engagement-maximizing data harvesting. The reward loop is **mastery + visible real-world good**,
  and the repo states this as a design constraint. (Prior art exists — FoldIt, EyeWire, Tomnod —
  the originality is the roguelite + daily-seed + combo-juice synthesis, fully open-source, and the
  pluggable "any humanitarian labeling task" backend.)

## Why it qualifies as a good deed
- **Tangible public benefit:** crowd-verified labels for disaster response and biodiversity science.
- **Freely available:** MIT engine + CC-BY-SA labels contributed back to the data partners.
- **Not primarily for-profit:** free, ad-free; output goes to responders/researchers.
- **No harm/misinformation:** calibration tiles + multi-player consensus guard label quality; no
  imagery of identifiable private individuals; partner-vetted datasets only.
- **Executable by AI sessions:** decomposes cleanly into engine modules, content packs, and the
  labeling-task backend.

## Scope / first tasks
- (medium) Core run engine (grid, combo system, calibration salting) as an open web game.
- (small) Daily-seed + local leaderboard (no account required to play).
- (medium) Pluggable labeling-task backend + one real dataset adapter (e.g., open post-disaster
  imagery or an open camera-trap set) with consensus aggregation.
- (small) "Impact receipt": show players what their verified labels contributed.

## Definition of shipped
A playable, deployed open-source game that has produced **verified labels accepted by a real data
partner** (a response org or research group), with the contribution loop demonstrably closed.

## Risks / review needs
**Risk tier: medium.** **Data/ethics review:** imagery must be openly licensed and free of
identifiable private individuals; sensitive disaster imagery handled per partner guidance.
**Quality:** never ship a label to a partner without consensus + calibration thresholds. **Design
review:** uphold the no-dark-patterns constraint; "addictive" here means *well-designed*, not
*exploitative*.

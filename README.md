# beacon-game

> An original game-with-a-purpose: play produces verified humanitarian/science image labels.  ·  **Risk tier:** medium  ·  **Status:** proposed (planning)



**Definition of shipped:** A deployed open-source game that has produced verified labels accepted by a real data partner.

This is an **Elyos** good-deed project. Contributors pull a task, do it with their own coding agent, and open a PR. Platform: https://github.com/jdev1977/elyos

## Planning
- [PROPOSAL.md](./PROPOSAL.md) — why this qualifies as a good deed (Good Deed Definition)
- [PLAN.md](./PLAN.md) — architecture, roadmap & milestones, risks
- [TASKS.md](./TASKS.md) — the full task backlog
- [tasks/](./tasks/) — ready-to-pull task JSON(s)

## Contribute
```bash
elyos browse
elyos pull --task-file tasks/beacon-repo-001.json --repo Elyos-Projects/beacon-game
# do the work with your own agent, then:
elyos submit beacon-repo-001 --repo Elyos-Projects/beacon-game
```

## Licensing & review
- **Licensing:** Engine: MIT. Labels: CC-BY-SA-4.0.
- **Review:** risk tier **medium** — deeds are *delivered, not merged*; a domain reviewer must sign off before merge.

> Status: this project is in **planning** and not yet ratified through Elyos governance; no adopting partner/requestor is secured yet (`verifiedNeed: false` on delivery-dependent tasks).

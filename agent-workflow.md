# Agent workflow

- gw-plan is the owner's planning and review contact. The owner does not read implementation or review agents' reports;
  they relay messages between agents and gw-plan. gw-plan raises decisions and actionable findings with the owner.
- gw-build handles bounded implementation. gw-engineer handles complex implementation and debugging. gw-research and
  gw-review are read-only. The planner gives the owner paste-ready briefs rather than invoking implementation agents.
- Every agent handoff to gw-plan must be **one self-contained fenced Markdown code block**, addressed to gw-plan, so the
  owner can copy only that block. It must contain the complete result, evidence, deviations, risks, blockers and any
  questions needing the owner's decision. Do not rely on progress updates outside the block.
- Only one agent writes to games-web at a time. Implementation work ends with an entry in games-web docs/log.md and a
  commit; do not push without authorization. Changes to saves, storage, migrations, transfer behavior and telemetry
  require explicit scope and careful review, never a silent side effect.

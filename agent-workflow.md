# Agent workflow

- OPENCODE (DeepSeek on the owner's laptop) does most work: well defined, mechanical tasks.
- LOCAL CLAUDE (on the laptop) does hard work, debugging, and anything touching saved data, storage, migrations,
  transfer codes or telemetry.
- The cloud planner plans and reviews. It codes only when the owner asks.
- Only one agent writes to the repo at a time.
- Every task ends with an entry in docs/log.md.

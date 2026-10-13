# Sun and moon game

Working concept for a fourth game. Name and public branding are not settled. Inspired by LinkedIn Tango; do not assume
permission to use LinkedIn's name or assets.

## Agreed direction, 2026-10-13

- Start with one mode using sun and moon symbols and the LinkedIn-style rules. Some pairs of adjacent cells have an `=`
  or `×` clue between them: `=` means the two cells hold the same symbol, `×` means they hold different symbols.
- A Takuzu mode may come later. Do not build it into the first version or make the first mode depend on it.
- Provide a generated bank of challenges and a separate daily mode, following the app's existing navigation, settings,
  save and update conventions. The fourth-game integration checklist is in games-web `docs/decisions.md`, under
  "Adding a game".

## Open before implementation

- Confirm the complete first-mode rules and board size, including the row and column balance and adjacency rules,
  whether duplicate rows or columns are forbidden, and the placement and frequency of `=` and `×` clues.
- Decide how the challenge bank is presented. A long ordered list, as in Jam, is the current recommendation; difficulty
  tiers need evidence from a human-style solver and generated puzzle distribution. Once published, a level number must
  continue to name the same puzzle so saved progress stays meaningful.
- Specify generation and validation: unique solution, playable clue distribution, avoidance of repeated or disguised
  puzzles, and difficulty assessment. Decide daily selection and launch date before freezing shipped dailies.
- Settle the game's own name and visuals before public release. Sun and moon symbols are the intended pieces; do not
  copy LinkedIn's branding or assets.

No implementation is authorized by this outline alone.

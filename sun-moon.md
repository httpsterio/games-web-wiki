# Sun and moon game

Working concept for a fourth game named Soluna. Inspired by LinkedIn Tango; do not assume permission to use LinkedIn's
name or assets.

## Agreed direction, 2026-10-13

- Start with one 6×6 mode using sun and moon symbols and the LinkedIn-style rules. Some pairs of adjacent cells have an
  `=` or `×` clue between them: `=` means the two cells hold the same symbol, `×` means they hold different symbols.
- Every row and column contains exactly three suns and three moons. No horizontal or vertical run may have three of
  the same symbol.
- A puzzle may start with fixed symbols as well as links. A fixed symbol can be one endpoint of a link, but usually is
  not. Do not ship link-only puzzles with no fixed symbols: swapping every sun and moon gives another solution. Every
  shipped puzzle must have exactly one solution under the full rules.
- Identical completed rows or columns are allowed in this first mode. Do not apply Takuzu's distinct-line rule. A
  separate Takuzu mode may come later; do not build it into the first version or make the first mode depend on it.
- Tap an editable cell to cycle empty → sun → moon → empty. No double tap, long press, timing settings or Queens-style
  mark key in the first mode. Fixed symbols do not cycle.
- Do not show immediate mistake feedback while a player cycles a cell. After a short delay of roughly 0.5 to 1 second
  without another tap, highlight an invalid completed row or column in red. A run of three identical symbols is an
  exception: highlight just the three offending cells in red even before its row or column is full. Do not reject the
  player's mark. Evaluate visible conflicts from the public balance and no-three rules, not against the hidden solution.
- Do not highlight an incorrect `=` or `×` link during play, even after both cells are filled. The owner does not want
  link feedback to reveal which symbol to place. A complete board must still satisfy every link before it is solved.
- Provide a generated bank of challenges and a separate daily mode, following the app's existing navigation, settings,
  save and update conventions. The fourth-game integration checklist is in games-web `docs/decisions.md`, under
  "Adding a game".

## Open before implementation

- Decide the placement and frequency of fixed symbols and `=` and `×` clues. Identical rows and columns are explicitly
  permitted; do not reject an otherwise valid board for matching lines.
- Decide keyboard input and whether dragging across cells is useful. Do not add gesture settings just because Queens has
  them.
- Settle the exact delay and whether it resets after every tap. Decide how to respond when all cells are filled but
  a link is violated without revealing which link or cell is wrong.
- Decide how the challenge bank is presented. A long ordered list, as in Jam, is the current recommendation; difficulty
  tiers need evidence from a human-style solver and generated puzzle distribution. Once published, a level number must
  continue to name the same puzzle so saved progress stays meaningful.
- Specify generation and validation: unique solution, playable clue distribution, avoidance of repeated or disguised
  puzzles, and difficulty assessment. Decide daily selection and launch date before freezing shipped dailies.
- Soluna is the chosen name and sun and moon are the intended pieces; do not copy LinkedIn's branding or assets.

No implementation is authorized by this outline alone.

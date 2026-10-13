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
- Support configurable double tap and long press interaction like Queens. The exact action mapping and which Queens
  input options belong in Soluna still need a decision.
- Provide a generated bank of challenges and a separate daily mode, following the app's existing navigation, settings,
  save and update conventions. The fourth-game integration checklist is in games-web `docs/decisions.md`, under
  "Adding a game".

## Open before implementation

- Decide the placement and frequency of fixed symbols and `=` and `×` clues. Identical rows and columns are explicitly
  permitted; do not reject an otherwise valid board for matching lines.
- Decide how a plain tap, double tap and long press place or clear the two symbols, and whether Soluna includes Queens'
  mark-key alternatives, timing options, drag behavior and practice control.
- Decide how the challenge bank is presented. A long ordered list, as in Jam, is the current recommendation; difficulty
  tiers need evidence from a human-style solver and generated puzzle distribution. Once published, a level number must
  continue to name the same puzzle so saved progress stays meaningful.
- Specify generation and validation: unique solution, playable clue distribution, avoidance of repeated or disguised
  puzzles, and difficulty assessment. Decide daily selection and launch date before freezing shipped dailies.
- Soluna is the chosen name and sun and moon are the intended pieces; do not copy LinkedIn's branding or assets.

No implementation is authorized by this outline alone.

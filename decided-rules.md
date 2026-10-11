# Decided rules

Rules for all games, agreed in planning. Rules the code must follow also belong in games-web docs/decisions.md.

- Restart outside a daily is a full fresh try: board, hearts, clock, hints and mistakes all reset.
- Restart in a daily only clears the board. The clock, hearts and hints carry on.
- Each daily is played once. A solved daily opens read only with its first result.
- The solve clock only runs while the puzzle is on screen. No idle cap. A long think still counts.
- Flow and Pure use different puzzles. Flowdoku's Pure pool is disguised versions of the Flow puzzles. Queens has a
  separately generated Pure pool.
- Key look: 50 ms press snap. Disabled keys are flat with a dim face, keyline and label.
- Raised keys for playing. Flat rows for settings. Not everything is raised.
- Pure hearts: plain heart icons, full, half, or a dim outline when lost. No boxes around them. Done.
- New games are built inside games-web, not as standalone repos. A game in progress can be hidden from the menu and
  reached only by URL.
- Telemetry, feedback and analytics stay off in games-web until there is one consent rule for all games and a decision
  on where data goes.

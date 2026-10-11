# History

How things used to be.

## Separate repos

- Each game started in its own repo: httpsterio/sudoku (Flowdoku), httpsterio/game-queens, httpsterio/game-jam.
- Flowdoku shipped from httpsterio/sudoku to flowdoku.netlify.app, with a test build at dev--flowdoku.netlify.app.
- The games moved into httpsterio/games-web from 2026-10-09 with git subtree, then got shared packages in packages/.
- Since 2026-10-11 games-web is the only source. The three old repos are retired. No edits or subtree pulls unless the
  owner asks for a specific fix. docs/sync.md in games-web is retired.

## Key look

- Planning once leaned towards raised keys for everything, settings included. Dropped. Raised keys are for playing,
  flat rows for settings.

## Monetization

- An earlier plan put the lifetime unlock first. Now subscriptions are the main business and the lifetime unlock sits
  alongside.

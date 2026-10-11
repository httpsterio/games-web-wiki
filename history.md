# History

How things used to be.

## Separate repos

- Each game started in its own repo: httpsterio/sudoku (Flowdoku), httpsterio/game-queens, httpsterio/game-jam.
- Flowdoku shipped from httpsterio/sudoku to flowdoku.netlify.app, with a test build at dev--flowdoku.netlify.app.
- The games moved into httpsterio/games-web from 2026-10-09 with git subtree, then got shared packages in packages/.
- Since 2026-10-11 games-web is the only source. The three old repos are retired. No edits or subtree pulls unless the
  owner asks for a specific fix.

## Subtree import

The games came in with `git subtree add`, full history, no squash, from each source repo's dev branch. The old sync file
in games-web (docs/sync.md, now deleted) listed these pull commands. Kept here so the history is not lost.

```sh
git subtree pull --prefix=games/game-sudoku https://github.com/httpsterio/sudoku dev -m "Pull Flowdoku from httpsterio/sudoku dev"
git subtree pull --prefix=games/game-queens https://github.com/httpsterio/game-queens dev -m "Pull Queens from httpsterio/game-queens dev"
git subtree pull --prefix=games/game-jam https://github.com/httpsterio/game-jam dev -m "Pull Jam from httpsterio/game-jam dev"
```

## Key look

- Planning once leaned towards raised keys for everything, settings included. Dropped. Raised keys are for playing,
  flat rows for settings.

## Monetization

- An earlier plan put the lifetime unlock first. Now subscriptions are the main business and the lifetime unlock sits
  alongside.

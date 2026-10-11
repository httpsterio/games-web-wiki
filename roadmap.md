# Roadmap

## Next refactor steps

1. Shared platform package: storage helpers, viewport, haptics, back navigation, theme and the Animations rule.
   Navigation uses the Queens and Jam model, which restores the screen after a reload. Flowdoku adopts it.
   Theme gets a System option in every game.
2. Unify behaviour: settings sheet, toasts, transfer core.
3. Storage upgrade.
   - Move saves and logs to IndexedDB. Keep only small settings in localStorage.
   - Ask the browser for persistent storage.
   - Add an Export everything backup, as a file only, in the game selector.
   - Per game transfer codes stay as they are.
   - Reason: all games share one origin and roughly 5 MB of localStorage. Queens keeps a board for every finished
     daily with no cap. Safari deletes site storage after 7 days without a visit unless the app was added to the home
     screen.
   - iPhone players get a gentle add to home screen nudge.
   - Touches saves, so it is LOCAL CLAUDE work.

## Recommended, waiting for the owner

- **Open.** Settings as a full height sheet over the current screen in every game. The same component from menus and
  mid game, closed by X or one back press. Built once in the shared packages, starting from Flowdoku's sheet.
- **Open.** One toast style and one transfer code core for all games, with a schema per game.
- **Open.** Daily hubs (Flowdoku's Daily page, Queens' and Jam's Daily page) are not safe points for silent updates.
  They hold nothing that a reload could lose, so they could be added.

## Before going public

- Buy the domain. Then set VITE_PUBLIC_URL per game.
- Old sites get no move notices for now. Flowdoku's old site has one already. Queens and Jam don't. If that changes,
  the owner adds one later.
- Player data doesn't move between sites by itself. Players need transfer codes or files.
- Telemetry on the new site needs one consent rule for all games, the Netlify functions and env vars, and a Metabase
  connection to prod.
- Flowdoku's debug menu must be off in production builds.
- Reset Queens and Jam daily numbering and archive start to the public launch day. Both start on 1 September 2026 now.
- Queens probably needs a new name or theme. See [Queens](queens.md).
- Font: PP Supply Mono will be swapped for another font, so its web licence is not a concern.

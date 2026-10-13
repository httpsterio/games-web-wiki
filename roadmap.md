# Roadmap

## Next refactor steps

1. Shared platform package: storage helpers, viewport and haptics are in place. System theme is available in every game
   (2026-10-13), with an expandable Theme row on all four settings pages. Back navigation and the Animations rule still
   need a focused status check. Flowdoku still returns to its main menu on reload, unlike Queens and Jam.
2. Unify behaviour: settings work and the one save file replaced per-game transfer codes. Queens, Jam and Flowdoku's
   daily copy use the small bottom toast. Flowdoku's brief puzzle feedback appears over the board. The longer feedback
   form result still uses the top bar; broader toast unification is not done.
3. Storage upgrade: steps 1 to 3 are live in IndexedDB. Step 4 was replaced by the one save file and is done. Step 5,
   a gentle iPhone add-to-home-screen notice, is built and awaits a real iPhone check. See
   [Storage upgrade](storage-upgrade.md) and [App settings and save file](app-settings-and-save-file.md).

## Recommended, waiting for the owner

- **Partial, 2026-10-13.** The short messages have their intended placements: general copy status at the bottom and
  Flowdoku puzzle feedback over the upper board. The longer Flowdoku feedback-form result still replaces the top-bar
  title in standalone mode; do not move it without a separate placement decision.
- Daily hubs are safe points for silent updates in all three games (2026-10-11).

## Before going public

- Buy the domain. Then set VITE_PUBLIC_URL per game.
- Old sites get no move notices for now. Flowdoku's old site has one already. Queens and Jam don't. If that changes,
  the owner adds one later.
- flowdoku.netlify.app (old sudoku repo) has a move notice that tells players to use Settings, Transfer progress in the new app. That page is gone since the one save file replaced the transfer codes (2026-10-12). Before launch, reword it to point at Settings, Import progress, or remove it. The old repo needs the owner's go ahead.
- Player data doesn't move between sites by itself. Players need transfer codes or files.
- Telemetry on the new site needs one consent rule for all games, the Netlify functions and env vars, and a Metabase
  connection to prod.
- Flowdoku's debug menu must be off in production builds.
- Reset Queens and Jam daily numbering and archive start to the public launch day. Both start on 1 September 2026 now.
- Queens probably needs a new name or theme. See [Queens](queens.md).
- Font: Geist Mono (OFL) is the default for text and Flowdoku's grid digits since 2026-10-12. Players can pick one of six fonts in Settings: Geist Mono, PP Supply Mono, Azeret Mono, IBM Plex Sans, Inter, Vollkorn. **Open.** PP Supply Mono's web licence was never checked. Settle it before launch or drop that option.

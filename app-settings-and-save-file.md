# App settings and save file

Design for one settings page for the whole app and one save file for all games. Owner's calls 2026-10-12. Replaces
storage step 4 (the backup file) and the per-game transfer codes. Claude designs and reviews, OPENCODE builds.

## Owner's calls

- One app settings page with the settings the games share: theme, animations, sound, haptics, the play stats question,
  and Your progress (export and import).
- Theme, animations, sound and haptics are app defaults. Each game can override them. A game setting that was never
  set follows the app default.
- One save file for all games, easy to parse, able to hold any number of games, not readable or editable as plain text.
- An old file that lacks a game still imports. A game missing from the file keeps this device's data.
- The per-game transfer pages and codes (FD, QN, JM) go.

## Settings

- App settings live in the store under `app:settings`, in IndexedDB with the game keys. Fields: `theme` (system, light,
  dark), `animations`, `sound`, `haptics`, `stats` (unanswered, yes, no).
- Animations follows the device's reduce motion setting until the player sets it, as the games do now.
- Each game's own theme, animations, sound and haptics take a new value, `app`, which means follow the app default. It
  is the default for every game. Flowdoku has sound, all three have the other three.
- The games read the effective value: their own unless it is `app`, then the app's. The shell exposes the app settings
  through the bridge. A game built on its own has no shell and uses built-in defaults.
- The selection menu follows the app theme. It no longer takes the theme of the game played last.
- The play stats question is asked once for all games. Nothing is sent until there is a decision on where the data
  goes, as now.
- The page opens from the selection menu (a gear in its top bar) and from the overlay inside any game.

## Save file

### Container

A text file, `games-save-yyyymmddhhmm.txt`, three lines:

```
GAMES-SAVE 1
<base64 of gzip of the JSON payload>
check <16 hex digits>
```

- Line 1 names the format and the container version. A newer container version is refused with a note to update.
- The check is a 64 bit FNV-1a hash of a fixed string plus line 2. Any edit to line 2 breaks it and the file is
  refused. The fixed string is in the code, so this stops casual editing, not a determined developer. Real
  protection needs a server, which comes with accounts.
- Size limits as the transfer codes have now: 5 MB of text, 20 MB of JSON.

### Payload

```
{
  "made": "2026-10-12T09:30:00.000Z",
  "build": "202610120930",
  "app": { "v": 1, "data": { ...app settings } },
  "games": {
    "flowdoku": { "v": 1, "data": { ... } },
    "queens": { "v": 1, "data": { ... } },
    "jam": { "v": 1, "data": { ... } }
  }
}
```

- One section per game, each with its own version. A new game adds a section. Unknown top level fields are ignored.
- Each game owns its section: what goes in, how it is checked, how older versions are upgraded, how it is applied.
  This is what each game's transfer module does today, moved into a pure section module.

### Sections

Each game gets `src/save/section.ts`, imported by the shell. It may read and write only through the store, and never
imports the game's state modules (settings, progress), which read storage and change the page when they load. That
is what lets the selection menu export and import every game without running any game.

```
id: 'queens'
version: 1
collect(): unknown                       the game's data, from the store
check(v, data): data | null              valid, upgraded to the current version, or null
apply(data): boolean                     replaces the game's keys, puts everything back if a write fails
describe(data): string                   a summary line for the import screen
```

What a section carries: the same as the transfer code does now, progress, games in progress, dailies, settings. Not
what belongs to the device: play stats ids, queued telemetry and feedback, debug and admin keys.

### Import

1. Read the file. A damaged, foreign or newer file is refused with a plain message.
2. Check each section. A game the app doesn't know is skipped. A section newer than the game is refused for that game.
3. Show a summary: which games the file has and what each holds, and which games stay as they are.
4. On confirm, apply each game, then the app settings, then reload.

### Export

Collects every game's section and the app settings, writes the file, then the share sheet or a download, as the
transfer files do now.

## Build order

1. The container, in packages/platform. Pure code and unit tests.
2. One section module per game, from its transfer module, with unit tests.
3. App settings: the store key, the bridge, the per-game `app` value, the page.
4. Export and import on the app settings page. Then the per-game transfer pages, codes and modules go.

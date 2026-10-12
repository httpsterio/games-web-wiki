# Storage upgrade

Plan for moving the games' saves from localStorage to IndexedDB. Steps 1, 2 and 3 are built (2026-10-12). Step 3 waits
for its browser test before the push. How it works now is in games-web docs/decisions.md, Storage. Touches saves, so it
is LOCAL CLAUDE work.

## Why

- All games share one origin and about 5 MB of localStorage. If one game fills it, saves fail in all three.
- Queens and Jam keep the end board of every finished daily, with no cap. Two modes make about 730 boards a year.
- IndexedDB gets far more room: hundreds of MB, or a share of the disk.
- Safari's 7 day rule (site data cleared when the site goes unvisited, unless it is on the home screen) applies to
  IndexedDB too. The fix for that is persistent storage (step 2) and the home screen nudge (step 5), not IndexedDB.

## Done

1. One shared store, `@games/platform/storage`, in packages/platform. `prefixed('queens:')` gives load, save, remove
   and keys for one prefix. The three games' load and save wrappers go through it. Same keys, same JSON, no behaviour
   change. Unit tests in packages/platform.
2. `keepStorage()` in `@games/platform/persist` asks the browser to keep the site's storage. Only in the installed
   app, where the browser decides without asking the player. In a normal tab Firefox would show a prompt.

## Step 3 design

Built as designed, without the migration (see No migration). Transfer modules use raw text reads and writes
(`loadRaw`, `saveRaw`), so transfer codes see exactly the bytes they saw in localStorage.

### The store stays synchronous

Every game reads and writes storage synchronously, many times, from module code and components. Making all of that
async would rewrite every game. So the store keeps its synchronous API and IndexedDB sits behind it:

- At launch, before any game code runs, the shell opens the database and reads every record into memory.
- Reads come from memory. Writes go to memory at once and to IndexedDB right after, in the background.
- When the page hides, pending writes are flushed.

The data is small (a few MB at most), so holding it in memory is fine.

### Game code runs only after the store is open

The games read storage the moment their modules load. Queens and Jam read progress, the settings of all three are read,
and Queens runs a one time pool migration. Module code runs before main.ts's own lines, so the shell's await in main.ts
is too late. The build plugin will instead give each page a small shell entry that opens the store, then imports the
game's main.ts. A game built or tested on its own has no shell entry, and its store falls back to plain localStorage, as
now. Its tests don't change.

### What moves

Everything under the games' prefixes moves, settings included (owner, 2026-10-12). One store and one rule is simpler
than a list of keys that stay behind. Session storage keys stay where they are. The shell's theme key stays in localStorage, because it is
read before the store opens.

One database, `games`, one object store, `kv`. Key: the full key as now (`queens:progress`). Value: the same JSON
text as now. Keeping the text, not objects, means transfer codes compare and write exactly the same bytes as today.

### No migration

Old localStorage saves are not moved: there are no players yet, so they can be wiped (owner, 2026-10-12). IndexedDB is
the store from the first launch. Game keys left in localStorage are removed at launch, so nothing reads them. The copy,
check and marker design that was here is dropped.

### When IndexedDB fails

The database won't open (blocked, missing, a Safari bug, no answer within about 3 seconds). Using localStorage then
would split saves between two places. So the store runs from memory only for that session: play works and nothing is written. It never uses localStorage
for game keys in the app (owner, 2026-10-12).

The player is told twice: a toast at launch, and a line that stays at the top of settings. Both say: "Progress can't be
saved right now. Reopen the app to try again."

### Code that goes straight to localStorage

These skip the wrappers and must move onto the store:

- Queens and Jam transfer.ts: read and write raw values and list keys. They need `loadRaw` and `saveRaw` for exact
  bytes, plus `keys`.
- Flowdoku saves.ts (game cap), migrate.ts, telemetry.ts and feedback.ts (quota handling), analytics.ts (the
  `umami.disabled` flag, which stays in localStorage because Umami reads it), and gameStore's save check.
- Queens' and Jam's session storage use stays as it is.

### Several tabs

Each tab holds its own copy in memory. Two tabs writing the same key: the last write wins, as with localStorage now. A
BroadcastChannel tells other tabs which keys changed, so their copies stay current for keys they have not changed
themselves.

### Tests

- Unit tests for the store with a fake IndexedDB (fake-indexeddb as a dev dependency, owner, 2026-10-12): read,
  write, remove, flush, old localStorage game keys wiped, a failed or silent database, no IndexedDB, a game on its own,
  two tabs.
- A browser test on a real build: old localStorage game keys wiped and not read, saves in the database read by all
  three games, play writes to the database only, a stuck database shows the toast and the settings line and changes
  nothing, the next launch has everything back.
- The long runs go to opencode.

### Owner's calls, 2026-10-12

- When the database fails, the session runs without saving. A toast at launch and a lasting line at the
  top of settings say "Progress can't be saved right now. Reopen the app to try again." Never fall back to localStorage.
- fake-indexeddb is added as a dev dependency for the tests.
- Settings move too.

## Later steps

4. Export everything as a file, from the selector. Uses the store's keys across all prefixes.
5. A gentle add to home screen nudge for iPhone players. Wording and placement are the owner's call.

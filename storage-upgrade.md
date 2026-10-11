# Storage upgrade

Plan for moving the games' saves from localStorage to IndexedDB. Steps 1 and 2 are done. This page is the design for
step 3, waiting for the owner. Touches saves, so it is LOCAL CLAUDE work.

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

Everything under the games' prefixes moves, settings included. One store and one rule is simpler than a list of keys
that stay behind. Session storage keys stay where they are. The shell's theme key stays in localStorage, because it is
read before the store opens.

One database, `games`, one object store, `kv`. Key: the full key as now (`queens:progress`). Value: the same JSON
text as now. Keeping the text, not objects, means transfer codes compare and write exactly the same bytes as today.

### Migration, once per device

1. First launch with this version: copy every prefixed localStorage key into IndexedDB in one transaction.
2. When the transaction completes, read every key back and compare it with localStorage.
3. Only if all match, write the marker `shell:storage = idb` to localStorage. From then on the store uses IndexedDB.
4. The localStorage copies stay for one more launch. At the next launch that opens the database and finds the marker,
   they are removed. A broken first run can't lose anything.

If any step fails, the marker is not written, localStorage stays the store, and the next launch tries again.

### When IndexedDB fails after the move

The marker says the data is in IndexedDB, but the database won't open (blocked, a Safari bug, a timeout of about 3
seconds). Falling back to localStorage would show empty saves, and anything played would be lost or would overwrite
real data later. So the store runs from memory only for that session: play works, nothing is written, and the player
is told saving is off. **Open:** what the player is told, and where.

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

- Unit tests for the store with a fake IndexedDB (fake-indexeddb as a dev dependency): open, read, write, flush,
  migration, failed migration, the marker without a database, two tabs.
- A browser test on a real build: old localStorage saves for all three games, upgrade, every save still there, transfer
  codes the same before and after, a second launch removes the localStorage copies.
- The long runs go to opencode.

### Open for the owner

- **Open.** The message when saving is off for a session (IndexedDB failed after the move).
- **Open.** fake-indexeddb as a new dev dependency, for the tests.
- **Open.** Settings move too (recommended), or stay in localStorage as the roadmap first said.

## Later steps

4. Export everything as a file, from the selector. Uses the store's keys across all prefixes.
5. A gentle add to home screen nudge for iPhone players. Wording and placement are the owner's call.

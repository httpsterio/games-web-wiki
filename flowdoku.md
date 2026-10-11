# Flowdoku

## Hint wording rewrite

**Done** 2026-10-11 (games-web 886a52c). Steps go by letter (A to P) and the hint header reads "NAME, STEP B". No cell
counts. A digit never sits next to a row, column or step number. Digit first sentences in fish, pointing and claiming.
"must be" in chains, "can only be ... or ..." in naked subsets, "two solutions" in rectangles. The live words are in
games/game-sudoku/docs/hint-copy.md.

Hint text size, same day (games-web 1f520d9): the sentence is 1rem with line height 1.5, the label above it 0.8rem and a
little thicker. Drawn by games/game-sudoku/src/components/HintText.svelte, used by the hint panel and the technique
walk-through.

## Techniques

- **Open.** Naked Triple is never the easiest step, so it has no examples. Recommended: drop it from the technique
  table.
- Technique pages, next step: try it yourself. The player makes the move on the example. Wrong taps get a nudge. Next
  example cycles through the 5 to 8 stored examples per technique. Each technique shows progress like 3 of 6 done.

## Pool

- Grader fix and pool regeneration are planned in games-web games/game-sudoku/docs/pool-regen.md.

## Small open items

- **Open.** Daily file tests only check every 4th daily.
- **Open.** The Expert daily score band leaves 600 to 1000 unused.
- **Open.** Some Fridays can be easier than Thursdays.
- **Open.** The daily file needs regenerating before summer 2027.

## Before going public

- The debug menu must be off in production builds.

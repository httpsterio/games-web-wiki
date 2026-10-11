# Flowdoku

## Hint wording rewrite

**Open.** Waiting for the owner's picks. Proposed changes to src/copy/hints.ts, shown as filled examples.

- Walk through Result
  - "So 6 goes in this cell." becomes "Place 6 here."
  - "So 2 comes out of the notes of this cell." becomes "Remove 2 from this cell's notes."
  - Same pattern for the plural versions.
- Full House: "Every other cell in row 6 is filled, so this is 6." becomes "This is the last empty cell in row 6. Only
  6 is missing."
- Hidden Single: "The 2s leave only this cell for 2 in the bottom right box, so this is 2." becomes "The only place
  left for 2 in the bottom right box is here. The other 2s block the rest." The step variants follow the same shape.
- Naked Single: "Its row, column and box hold every digit but 3, so this is 3." becomes "Its row, column and box
  already have every digit except 3."
- Naked subsets: "only hold" becomes "can only be".
- Hidden subsets: "4 and 7 only fit these 2 cells of column 5, so those cells hold nothing else." becomes "In column
  5, 4 and 7 only fit these 2 cells, so remove their other notes."
- Chains and wings: "is 7" becomes "must be 7". For example "One end of this chain must be 7, so this cell, seeing
  both ends, can't be 7."
- Rectangles: "could swap and give 2 answers, so it isn't." becomes "would allow 2 solutions. So it isn't 1."
  "answers" becomes "solutions" everywhere.
- Finned fish: "Any way they fill" becomes "However they're placed".
- Empty Rectangle
  - "3 is on this row or column" becomes "3 must sit on this row or column".
  - "Either one, with this pair, rules out 3 in this cell." becomes "Either way, with this pair, 3 can't go in this
    cell."

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

# Stage 7: the string menu, by hand

**Goal:** give each string its own scale, key, notes or chords, and octave, chosen in mid-air

**File:** [`index.html`](../index.html), sections 10, 18, 20 and 21

## Answer to the last question

- A hand "clicks" by pinching and letting go without moving
- Or by holding still over a choice until a timer fills, which interface designers call **dwell**
- This stage uses both

## What you'll learn

**Settings that belong to each string**
- Every string now carries its own `key`, `scale`, `mode` (notes or chords) and `oct` (octave)
- A new string copies whatever the top bar shows
- The top bar changes every string at once, and the menu changes one
- `degree` now takes the string as an input, so each string plays in its own key

**More jobs for a pinch**
- When a pinch closes, the code works through this list, in order:
  1. A menu is open: choose the option under the pinch, or close the menu if it misses
  2. Over a top-bar button: press it when the pinch lets go
  3. Near a peg: drag it
  4. On a string's middle dot: open the menu when the pinch lets go
  5. Anywhere else: free air, for making strings
- The order matters
  - A peg beats a dot, so a short string's ends stay grabbable
- **Like a click:** a dot or button only counts if the pinch stays put
  - If it moved more than 50 pixels, it was a drag, not a click

**Two ways to choose**
- **Pinch** on an option: chosen at once
  - Fast and precise
- **Hover** your pinch point over an option: a ring fills, and it's chosen after 0.7 seconds
  - No pinch needed, which is easier on tired hands
  - After it's chosen, move off the option and back to choose it again
- The mouse just clicks

**Closing the menu**
- Press **Done**, pinch anywhere outside the menu, or leave it alone for 10 seconds
- While a menu is open, nothing plucks, so choosing can't play notes by accident

**Hands on page buttons**
- The canvas and the buttons sit at the same screen positions
- `document.elementFromPoint` asks the page what's under a point
- If a button is under a hand's pinch point, it gets a ring, and a still pinch presses it

**Aiming rings**
- Pegs and middle dots both light up when your pinch point is close enough to grab them

## Read the code in this order

1. Section 10: `pinchStart`, the list of jobs
2. Section 20: `openMenu`, `layoutMenu`, `chooseItem`
3. Section 20: `updateMenu`, the dwell timer
4. Section 21: `buttonAt`, the hand-on-button check
5. Section 18: `degree`, which now uses the string's own settings

## Test it

**With the mouse**
1. Click the small dot in the middle of a string
2. Choose **Minor**, a key, **Chords** and **High**
   - Each choice plays the string quietly
3. Pluck it, then pluck another string
   - They sound different
4. Press **Key** in the top bar
   - Every string changes

**With your hands**
1. Hover near a string's middle dot until it gets a ring
2. Pinch and let go without moving
   - The menu opens
3. Hold your pinch point over an option until the small ring fills
4. Pinch an option to choose it at once
5. Pinch outside the menu to close it
6. Hover over **Key** in the top bar, then pinch and let go

## How I tested it

- **Mouse:** clicking the dot opened the menu
  - Choosing Minor, D, Chords and High changed only that string
  - Its first zone played D minor (D-F-A) an octave up
  - **Done** closed the menu
- **Top bar:** pressing **Key** changed every string
- **Hands:**
  - A still pinch on the dot opened the menu for the right string
  - Hovering for 0.3 seconds chose nothing, and 0.8 seconds chose **Blues**
  - A pinch on **F♯** chose it straight away
  - A pinch outside the menu closed it
  - A pinch on the dot that dragged away didn't open the menu
  - A hand over a top-bar button gave it a ring, and a pinch pressed it
- **A bug I fixed:** the menu read two different clocks, so in testing it closed the moment it opened
  - It now uses the same clock as everything else
- **Earlier stages:** every earlier test still passes

**Can't test from here:** how it feels to choose with your own hands

## Experiments

- a) Change `DWELL = 0.7` to `0.3`, then `1.5`
- b) Change `IDLE_CLOSE = 10` to `3`
- c) Change `dotRadius: 28` in section 3 to `60`, then try to grab a short string's peg
- d) Add an **Octave** option for two octaves up: add `[2, 'Higher']` to `OCTAVES`

## Question before Stage 8

- Every string still sounds like the same guitar
- His version has 24 instruments, including drums, all played the same way
- How might a drum be "plucked" along a string?

## Fix after testing: point with your fingertip

**What happened**
- The menu only reacted when the ring landed on an option, not your fingertip
- The ring was the pinch point, halfway between thumb and index tip
  - With an open hand, that's empty space between your fingers, not where you're pointing

**What changed**
- Each hand now has one **pointer** (`h.cursor` in section 9)
  - Open hand: the index fingertip
  - Closing into a pinch: it slides smoothly to the pinch point
  - At the moment of the pinch they're nearly the same spot, so nothing jumps
- The pointer drives the menu, the top-bar buttons, and the aiming rings on pegs and dots
- While a pinch is held, a dragged peg follows the pinch point itself
  - Before, it drifted about 30 px toward your fingertip as you let go

**Tested**
- Fingertip on **E** with the thumb far away chose **E**
  - The old in-between point was over a different option
- The old in-between point on **A**, with the fingertip elsewhere, chose nothing
- Pointing at **G** and bringing the thumb up to pinch chose **G**
- A dragged peg now lands exactly where you let go
- Every earlier test still passes

**Setting:** `POINTER_OPEN` in section 2 is how open the hand must be for the pointer to sit fully on the fingertip

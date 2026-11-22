# Stage 4: a precise pinch, and strings you make

**Goal:** a pinch that holds steady when you aim it, and does the right job for what it's aimed at

**File:** [`index.html`](../index.html), sections 8, 10, 11 and 13

## What you'll learn

**Why Stage 2's smoothing wasn't enough**
- Stage 2 moved every point halfway to its new spot, every frame
- That's a fixed trade between two problems:
  - Smooth a lot, and fast moves lag behind your hand
  - Smooth a little, and a still hand shakes

**Smoothing that adapts to speed**
- The new smoother measures how fast each point is moving
- Still or slow: heavy smoothing, so a pinch you're aiming holds steady
- Fast: almost none, so a swing keeps up with your hand
- The method is the **One Euro Filter**, published by Casiez, Roussel and Vogel in 2012
  - Cursor, VR and motion-capture software use it for the same reason
- **Cutoff** is in vibrations per second
  - Wobble faster than the cutoff gets smoothed away
  - Movement slower than the cutoff passes through
  - The cutoff rises as the point speeds up

**A pinch with a job**
- When a pinch closes, the code checks what's under it, then sticks with that job until you let go
  - Near a peg: **drag** that peg
  - Anywhere else: **free**
  - Two free pinches at once: **make** a new string between them
- The code decides once, at the moment of the pinch
  - So a drag can't turn into a new string halfway through
- Later stages add more jobs here:
  - The middle of a string: open its menu
  - The string itself: bend it
  - Empty air with one hand: effects

**Aiming**
- When an open hand's pinch point is near a peg, the peg gets a ring
- You see what a pinch will grab before you close it

**Pinches don't pluck**
- A hand can only pluck when all three are true:
  - It isn't pinching
  - It's clearly open, above the "opens above" mark
  - It let go of its last pinch more than a quarter of a second ago
- A brand-new string ignores plucks for 0.4 seconds, while your hands are still on it

**Rules for strings**
- A new string has to be at least 110 pixels long, or you get a message instead
- Drag a peg to within 60 pixels of the other peg and the string turns red
  - Let go and it's deleted
- 12 strings at most

## Read the code in this order

1. Section 8: `Smoother`, the adaptive smoothing (about 20 lines)
2. Section 10: `pinchStart`, `updateGrabs`, `pinchEnd`, the jobs
3. Section 11: `addString`, `removeString`, `nearestPeg`
4. Section 13: `plucking`, which decides who may pluck right now
5. Section 17: the mouse, which uses the same pinch code
   - Pressing the button is a pinch

## Test it

**Mouse**
1. Press **Use the mouse instead**
2. Press in empty space and drag
   - A dashed line follows you: dim while it's too short, bright once it's long enough
3. Let go: a new string
4. Press on a peg and drag it somewhere else
5. Drag one peg onto the other: the string turns red, and letting go deletes it
6. Move without pressing to pluck

**Camera**
1. Pinch with both hands in empty air and pull apart
2. Let go: a new string
3. Hover an open hand near a peg: a ring appears on it
4. Pinch to grab the peg, then move it
5. Pinch near the string and sweep across it: silence
6. Let go and sweep again: it plays

## How I tested it

- **The smoother** on a still point with 4 pixels of wobble:
  - Error dropped from 1.1 px to 0.6 px compared with Stage 2
- **The smoother** on a fast 1,500 px/s swing:
  - Stage 2 lagged 50 px behind, the new one 9 px
- **Scripted hands**, moving smoothly, checked every job:
  - Stretching made a string
  - A short stretch showed the message
  - A peg dragged
  - Pegs pushed together deleted the string
  - A pinched hand crossing a string stayed silent
  - A hand that had just let go stayed silent, and 0.4 seconds later it plucked
  - Hands that vanished mid-pinch cancelled cleanly
- **The real mouse** made a string by dragging and plucked it
- **The hands photo** still tracks in the real tracker
- I can't test your actual hands from here, so tell me how it feels

## Experiments

- a) Set `steadyCutoff: 0.5`, hold a pinch still, then swing your hand
  - Then try `steadyCutoff: 5`
- b) Set `speedBoost: 0` and swing your hand fast
  - That's Stage 2's lag coming back
- c) Change `grabRadius: 40` to `80`
  - Grabbing gets easier, until two pegs sit close together
- d) Change `minLength: 110` to `300`

## Question before Stage 5

- Pluck the middle of a string, then near one end
- In Stage 6 each spot along a string will play its own note
- How would you split a string into notes, so that a longer string holds more of them?

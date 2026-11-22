# Stage 9: hold, roll, bend and snap

**Goal:** play a string the way you'd play a real one: hold notes, roll drums, bend the pitch

**File:** [`index.html`](../index.html), sections 12c, 23 and 24

## Answer to the last question

- Speed and time tell a resting finger from a sweeping one
- A finger is **resting** when it stays within 12 px of a string, moving slower than 250 px a second, for 0.15 seconds
- A sweep crosses the string far faster than that, so it plucks instead

## What you'll learn

**Notes became "voices"**
- Until now, a note was fire-and-forget: it played and nobody could touch it again
- Now `playNote` returns a **voice**, a handle on that one note, with two controls:
  - `bend(cents)` changes the pitch of every sound source in the note at once
  - `release(fade)` fades the note out and stops everything in it
- To make that possible, every oscillator and noise source a note creates is collected into a list as it's built
- Every note also gets a **gate**: one volume knob for the whole note, so releasing it is a single move

**Holding**
- Each instrument has a `hold` setting in `INSTRUMENTS`:
  - `'sustain'`: violin, cello, strings, flute, trumpet, voices, organ, synth
    - The note keeps going until you lift off
  - `'roll'`: drums, hand drums, marimba, steel pan, oud, qanun
    - Fast repeated hits, about 12 a second, each a little softer or louder than the last, like real hands
  - none: guitar, piano, harp, bells and the rest ring out once, the way they do for real
- Slide a held finger along the string and the note follows, zone by zone
- The middle dot is left out: resting there opens the menu instead

**Bending**
- Pinch anywhere along a string, away from the pegs and the middle dot, and pull sideways
  - The string bends into a point at your pinch, and the note rises
- **Detune** is measured in **cents**: 100 cents is one semitone
  - Pulling 140 px gives the full bend, 200 cents, which is two semitones, like a big guitar bend
- Let go and the pitch snaps back, and a fresh pluck adds the twang
  - The further you pulled, the louder the twang

**One list of targets, grown again**
- `targetAt` now has five kinds, checked in order:
  1. Menu option
  2. Button
  3. Peg
  4. Middle dot
  5. **String body**, for bending
- A pinch on the body starts a bend
- The aiming ring shows on the string when a pinch would grab it

## Read the code in this order

1. Section 12c: `makeVoice`, the handle with `bend` and `release`
2. Section 12c: `playNote` and `buildNote`, which collect the sources and add the gate
3. Section 23: `checkHold`, which tells resting from sweeping
4. Section 23: `startHold`, `moveHold`, `endHold`, `updateHolds`, the life of a hold
5. Section 24: `startBend`, `moveBend`, `endBend`

## Test it

1. Make a string and set it to **Violin**
   - Rest your fingertip on it: the note keeps going until you lift off
   - Slide along: the note follows
2. Set a string to **Drums** and rest a fingertip on the snare zone: a drumroll
3. Set one to **Guitar**, pinch the string and pull sideways slowly
   - Listen to the pitch rise
   - Let go for the twang
4. Pinch a violin string and pull: the held note bends, and letting go ends it

## How I tested it

- **Violin hold**
  - Resting for 2 seconds kept it sounding (a fixed note ends at 0.9 s)
  - Sliding along moved it to zone 5
  - Lifting off took the output to silence
- **Drum roll:** about 10 hits a second in my test browser
  - It draws fewer frames per second than a real screen, so expect about 12 on your Mac
- **No accidental holds**
  - A sweep across the violin plucked instead of holding
  - Resting on the middle dot started no hold
  - Resting on a guitar string started no hold, as intended
- **Bend:** pulling the guitar 140 px measured exactly 200 cents, and the violin at 70 px exactly 100 cents
- **Snap:** letting go added one twang pluck and straightened the string
- **Release:** the bent violin went silent once its twang note finished
- **Earlier stages:** every earlier test still passes

**Can't test from here:** how holding and bending feel with your own hands

## Experiments

- a) Change `rollGap: 0.085` to `0.04`, then `0.2`
- b) Change `bendMax: 200` to `1200`: a full octave
- c) Give the guitar `hold: 'roll'` in `INSTRUMENTS`, for tremolo picking
- d) Give the piano `hold: 'sustain'`. What happens, and why?
  - Hint: look at what `L` does in the `piano` case
- e) Change `holdAfter: 0.15` to `0.5`, then try playing fast

## Question before Stage 10

- Every effect so far belongs to one string
- His version also has an "air pad": pinch empty air with one hand and drag, and the whole mix gets darker, thinner or echoey
- How could one drag control three different effects at once?

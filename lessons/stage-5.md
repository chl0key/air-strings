# Stage 5: notes, scales and chords

**Goal:** every spot along a string plays its own note, and every note fits the key

**File:** [`index.html`](../index.html), sections 18 and 19, plus `pluck` in section 13

## What you'll learn

**Notes as numbers**
- Every note has a **MIDI number**
  - Middle C is 60
  - One step up (one piano key, called a semitone) adds 1
  - 12 steps make an octave, which doubles the pitch
- `midiToPitch` turns a MIDI number into vibrations per second
  - A4 is 69, which is 440 vibrations per second

**Scales**
- A scale is a list of steps up from the key note
  - Major: 0, 2, 4, 5, 7, 9, 11
- Past the end of the list, it wraps into the next octave
- Any note from the scale sounds good with any other, so you can't play a wrong note
- Change the key and every note shifts up or down together

**Zones along a string**
- Each string is cut into equal zones, one note per 70 pixels
  - At least 3 notes and at most 15
- So a longer string holds more notes, and a short one is a quick 3-note riff
- Notes climb from the **low end** to the **high end**
  - On a flat string, the low end is the left peg
  - On an upright string, it's the bottom peg
  - The low peg is a filled dot and the high peg a ring

**Chords**
- A basic chord (a triad) is a scale step, plus the step two above it, plus the step two above that
  - In C major, step 0 gives C-E-G, which is C major
  - Step 1 gives D-F-A, which is D minor
- Every chord built this way fits the key
- Pentatonic and blues scales have gaps, so they borrow chords from their full parent scale
- The name comes from the gaps between the notes:
  - 4 then 3 semitones: major
  - 3 then 4: minor (m)
  - 3 then 3: diminished (dim)

**Strumming**
- The notes of a chord don't start together
- Each one starts 18 thousandths of a second after the last, like a pick crossing guitar strings
- Sweep down and the strum goes one way, sweep up and it goes the other

## Read the code in this order

1. Section 18: `SCALES`, then `degree`
   - `degree` is the one function that turns a step number into a note
2. Section 18: `zoneCount`, `lowEnd`, `zoneAt`
3. Section 19: `zoneNotes` and `chordName`
4. Section 13: `pluck`, where the strum happens

## Test it

1. Press **Use the mouse instead**
2. Sweep across a string from left to right, slowly
   - The notes climb, and each name floats up as it plays
3. Press **Key: C** a few times, then sweep again
   - Same tune shape, higher
4. Press the scale button until you reach **Blues**, then **Hijaz**
5. Press **Single notes** to switch to **Chords**, and sweep up and down across one spot
   - Listen for the strum changing direction
6. Stretch a short string and a long one, and compare how many notes they hold
7. Try all of it with the camera
   - The buttons still need the mouse for now
   - Stage 7 lets your hands press them

## How I tested it

- **Note counts:** a 150 px string holds 3 notes, 500 px holds 7, and 1,200 px holds 15
- **Spelling:** each scale came out right
  - C major: C D E F G A B C
  - A minor: A B C D E F G A
  - C blues: C E♭ F F♯ G B♭ C E♭
  - D hijaz: D E♭ F♯ G A B♭ C D
- **Chords:** C major gives C Dm Em F G Am Bdim C, the textbook set
- **Low end:** correct for flat and upright strings, whichever way the pegs were placed
- **Real pitch:** a sound meter heard 130.5 Hz for C3 (should be 130.8) and 196.5 Hz for G3 (should be 196.0)
  - The small gaps come from the meter's resolution
- **Earlier stages:** the Stage 4 gesture tests and the camera test still pass

## Experiments

- a) Change `ZONE_SIZE = 70` to `40`, then `120`
- b) Change `BASE_NOTE = 48` to `36`
- c) Change `STRUM_GAP = 0.018` to `0.08`, then `0`
- d) Add your own scale to `SCALES`
  - For example, Japanese in-sen: `insen: { name: 'In-sen', steps: [0, 1, 5, 7, 10] }`
  - Pentatonic scales need a `parent` for chords, so give it `parent: 'minor'`
- e) In `zoneNotes`, change `[0, 2, 4]` to `[0, 2, 4, 6]` for 4-note chords
  - `chordName` will show a `?`, because it only knows 3-note chords

## Question before Stage 6

- Every instrument still plays the same plain beep
- Pluck a real guitar string and the sound is bright at first, then turns softer as it fades
- What could make a computer sound do that?

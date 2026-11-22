# Stage 12: the looper

**Goal:** record what you play over the beat and stack it up in layers

**File:** [`index.html`](../index.html), section 27

## Answer to the last question

- To replay a sound later, a loop needs five things:
  - **Instrument:** which one
  - **Notes:** which ones, or for a drum string, which drum
  - **Loudness**
  - **Position:** where in the loop it happened
  - **Length:** for a held note, how long it lasted
- Position and length are counted in **beat steps** (sixteenth notes), not seconds
  - So if you change the tempo, a layer speeds up or slows down with the beat and stays in time

## What you'll learn

**Recording on the bar line**
- Press **Record layer**, and the looper waits for the start of the next bar
  - That's your count-in
- It records for exactly 1, 2 or 4 bars, then stops on a bar line too
- Starting and stopping on bar lines keeps every layer lined up with the beat and with each other
- The scheduler from Stage 11 handles this: as it books each step, it checks whether recording should start or stop there

**Capturing sounds**
- Every pluck, chord, drum roll and held note goes through `captureEvent`
- That works out the sound's position: (time now − time recording started) ÷ length of a step
- A held note gets its length when you lift off, in `endSustain`

**Snap to the beat**
- With snap on, each position is rounded to the nearest whole step
  - A pluck that lands slightly late moves onto the beat
- With snap off, your exact timing is kept, swing and all

**Playing it back**
- Each layer files its sounds under the step they happened on, in a list called `buckets`
  - When the scheduler books a step, a layer only looks in that step's bucket
- A brand-new layer skips its first time round, because you just played it live
- Held notes replay as held notes, released at exactly their recorded length (`releaseAt`)

**Follow chords**
- With it on, every string is retuned to the beat's chord for the current bar
- Each zone plays a note of that chord: root, third, fifth, then up an octave and round again
- In chords mode, each zone plays the whole chord, a little higher each zone
- Anywhere you sweep fits the beat, so you can play by feel without thinking about notes

**A fix that came out of testing**
- A finger resting on a violin string that then moved away *through* the string counted as a pluck
- Lifting off shouldn't twang it, so a hold that ends now counts the side the finger left by as its starting side

## Read the code in this order

1. Section 27: `armRecording`, `loopBoundary`, `finishRecording`, the life of a recording
2. Section 27: `captureEvent` and `endSustain`
3. Section 27: `playLayers`
4. Section 27a: `followNotes` and `currentBar`
5. Section 26: `planAhead`, which now calls the looper on every step

## Test it

1. Press **Beat**, switch on **Kick** and **Hats**
2. Press **Loop**, choose **2 bars**, then **Record layer**
   - The panel closes, and the top left says "Get ready"
3. When it says "Recording", play a few notes on a guitar string
   - It loops them when the 2 bars are up
4. Record a second layer on a violin string, holding notes
5. Switch on **Follow chords** and sweep anywhere: it all fits
6. Mute a layer, then delete it

## How I tested it

- **Arming** closed the panel, started the beat, and waited for the bar line
- **Four plucks** in a 1-bar loop landed on steps 0, 4, 9 and 13, snapped to whole steps
  - Over the next two loops they replayed 8 times, as expected
- **A held violin note**, with snap off, was recorded as one note 6.7 steps long
  - That matches how long it was held after the hold kicked in
- **A tempo change** from 120 to 90 BPM kept the layers in time
- **Follow chords** tracked the progression bar by bar: F, C, G, which is IV, I and V in C major
- **Mute, Undo last and Delete** all worked
- **Earlier stages:** every earlier test still passes, and the top bar still doesn't jump with the new **Loop** button

**Can't test from here:** how it feels to build a loop with your own hands

## Experiments

- a) Set `MAX_LAYERS = 12`
- b) In `captureEvent`, change `Math.round(pos)` to `Math.round(pos / 2) * 2`
  - That snaps to eighth notes instead of sixteenths
- c) In `followNotes`, change `[0, 2, 4]` to `[0, 2, 4, 6]` for seventh chords
- d) Record a layer with snap off, then one with snap on, and listen to the difference

## Question before Stage 13

- You've made a loop, and when the page closes, it's gone
- The browser can record its own sound and picture
- What would it need to capture to save a video of you playing, with sound?

# Stage 6: a real string sound

**Goal:** the beep becomes a plucked string, played in a room

**File:** [`index.html`](../index.html), section 12 (12, 12a, 12b)

## Answer to the last question

- A real string starts bright and turns mellow as it fades
- The high sounds die away faster than the low ones
- A steady oscillator can't do that, because its tone never changes
- **Karplus-Strong** does it with a loop and an average

## What you'll learn

**Karplus-Strong (1983)**
1. Fill a short loop with random noise
   - That's the pluck: a burst of every pitch at once, like a pick snapping off a string
2. Play the loop round and round
   - A loop that repeats 131 times a second is heard as the note C3
   - So the loop's length sets the note
3. Each trip round, replace every sample with the average of itself and its neighbour
   - Averaging smooths out fast wiggles first, so high sounds fade first
   - That's the bright-to-mellow change of a real string
4. Shrink everything a tiny bit each trip, so the note fades out

**Two details that keep it in tune**
- A loop length is usually not a whole number of samples
  - At 48,000 samples a second, C3 needs a loop 367.0 samples long, but A4 needs 109.1
- An **all-pass filter** makes up the leftover fraction
  - It delays sound by part of a sample without changing the tone
- Without it, high notes drift out of tune

**Where the string is plucked**
- Pluck a guitar near the bridge and it twangs, pluck it in the middle and it sounds round
- `pickAt` copies that effect by subtracting a shifted copy of the starting noise

**Saving work**
- Each note is built once, in about 3 thousandths of a second, then saved
- Every later pluck of that note replays the saved copy

**Reverb**
- A real room answers every sound with thousands of tiny echoes that blur into a fading tail
- Random noise that fades away sounds a lot like that tail
- A **convolver** plays the tail after every note
- About a quarter of each note goes to the reverb, and the rest goes straight out

**Harder plucks sound brighter**
- A **lowpass filter** on each note lets low sounds through and trims the highs
- Soft plucks get a low cutoff and sound darker, hard plucks a high one

**The limiter**
- 12 notes ringing at once add up, and anything over the maximum level distorts
- The **limiter** squashes the sound when it gets close to that maximum

## Read the code in this order

1. Section 12b: `stringNote`, steps 1 to 4 marked in the comments
2. Section 12b: `playString`, one note through the tone filter to the speakers
3. Section 12a: `makeRoom`, the made-up room
4. Section 12: `startAudio`, the whole path from note to speakers

## Test it

1. Turn the sound up and press **Use the mouse instead**
2. Sweep slowly along a string
   - Each note should ring for a couple of seconds, and the low notes ring longest
3. Sweep fast, then slow
   - Fast is louder and brighter
4. Switch to **Chords** and strum up and down
5. Strum hard across several strings, as fast as you can
   - It should get loud without crackling

## How I tested it

- **Pitch** on 10 notes from low E (82 Hz) to high G (1,568 Hz):
  - All within 0.5 cents of perfect
  - A cent is a hundredth of a semitone, and most people can't hear an error under 5
- **Fade:** every note drops to under a tenth of its starting loudness within a second
- **Bright to mellow:** brightness falls about tenfold between the start of a note and one second in
- **Speed:** each new note takes about 3 ms to build, and a repeat comes from the cache instantly
- **Distortion:** the first version peaked at 1.37, and anything over 1.00 crackles
  - I lowered the gain and tightened the limiter
  - 4 loud chords at once (12 notes) now peak at 0.75
- **Through the speakers:** the played pitch matched the note
- **Earlier stages:** the gesture, chord and camera tests all still pass

**Can't test from here:** how it sounds to your ears

## Experiments

All in `STRING_SOUND` at the top of section 12b:
- a) `bright: 0.2`, then `bright: 0.95`
- b) `pickAt: 0.5`, then `pickAt: 0.03`
- c) `ring: 1`, then `ring: 8`
- d) In section 12a, set `REVERB_AMOUNT = 0` for a dry sound, then `0.8` for a cathedral
- e) Set `ROOM_SECONDS = 6`

## Question before Stage 7

- So far, every choice is made with the mouse on the top bar
- His version lets you pick each string's instrument and scale with your hand, in mid-air
- How could a hand "click" something without a mouse?

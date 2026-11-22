# Stage 8: 24 instruments

**Goal:** every string can be any of 24 instruments, drums included, all built from scratch

**File:** [`index.html`](../index.html), sections 12b, 12c and 12d

## Answer to the last question

- A drum can't play notes, but a string still has zones
- So on a drum string, each zone is a different drum instead of a note
- Low drums sit at the low end and high ones at the high end, the same as notes

## What you'll learn

**Six building blocks make every instrument**
- **Karplus-Strong strings** (Stage 6), for anything plucked
- **Oscillators:** steady tones in four shapes: sine (pure), triangle (soft), sawtooth (buzzy), square (hollow)
- **Partials:** several sine waves at chosen multiples of the note's pitch, each fading at its own speed
  - A bell, a marimba and a piano differ mostly in which multiples ring, how loud, and for how long
- **FM (frequency modulation):** one oscillator wobbles another's pitch very fast
  - That adds bright, metallic overtones, like the electric piano's bell-like attack
- **Filtered noise:** breath, bows, hammers and drums
- **Envelopes:** the shape of the volume over time

**How each family is made**

| Instruments | Recipe |
| --- | --- |
| Guitar, harp, oud, qanun, sitar, bass | Karplus-Strong with different brightness, pluck spot and ring time |
| Oud, qanun | Two or three strings per note, tuned a few cents apart, for shimmer |
| Sitar | A flat bridge flattens one side of each swing, which makes the buzz |
| Electric guitar | Karplus-Strong through a "drive" that rounds off loud peaks, like a pushed amplifier |
| Piano | 10 partials, each slightly sharp (stiff strings), plus a felt-hammer thump |
| Electric piano | FM, with a strong wobble at the start that fades |
| Organ | 7 sines held flat, plus a slow wobble like a rotating speaker |
| Marimba, kalimba, steel pan | A few partials each, with fast-fading bright ones on top |
| Bells | 9 partials at non-whole multiples, from Jean-Claude Risset's 1960s bell |
| Violin, cello | A sawtooth (the bow's stick-and-slip), a body filter, vibrato and bow noise |
| Strings | Three sawtooths slightly out of tune with each other, slow to rise |
| Flute | A pure tone plus breath noise tuned near the note |
| Trumpet | A sawtooth through a filter that opens as the note starts, with a small slide up into pitch |
| Voices | A buzzy tone through three narrow filters at the shape of an open mouth ("aah") |
| Synth | Two buzzy oscillators through a filter that sweeps closed |
| 808 bass | A sine that drops in pitch, plus gentle distortion so laptop speakers can play it |
| Drums | A tone that drops in pitch (the skin) and bursts of filtered noise |

**Octaves that suit each instrument**
- `shift` moves an instrument up or down from the string's notes
- The cello plays an octave below, and the violin and flute an octave above

**Even loudness**
- `level` trims each instrument so they all sound about as loud as each other
- Each instrument also has its own reverb amount, `room`
  - The harp and voices get a big room, and the 808 almost none

**Held notes, for now**
- Violin, organ, voices and other held instruments last `NOTE_HOLD` (0.9 seconds)
- Stage 9 lets you hold a note for as long as your finger rests on the string

**The menu adapts**
- The string menu gains an **Instrument** section: 4 rows of 6, one family per row
- On a drum string, scale, key, chords and octave don't apply, so the menu hides them
- Each string shows its instrument's name beside its middle dot

## Read the code in this order

1. Section 12c: `INSTRUMENTS`, the table of all 24
2. Section 12c: `playNote`, one `case` per family
   - Start with `marimba`, then `piano`, then `bowed`
3. Section 12c: `partials`, `env` and `swell`, the small helpers
4. Section 12d: `DRUM_KITS` and `playDrum`
5. Section 12b: `addPluck`, now shared by every plucked instrument, with the sitar's bridge

## Test it

1. Hold your fingertip on a string's middle dot to open its menu
2. Try one instrument from each row, plucking after each choice
3. Choose **Drums** and sweep along the string
   - Kick at the low end, crash at the high end
4. Choose **Hand drums** and do the same
5. Make three strings: **Bass**, **Piano** and **Drums**, and play them together

## How I tested it

- **Tuning:** all 22 pitched instruments played their notes within 4 cents
  - Most ears can't hear an error under 5
  - Bells are left out: their overtones aren't whole multiples, which is the point of a bell
- **Loudness:** measured each instrument's peak and steady loudness, then trimmed them
  - Before trimming, the flute's steady loudness was about 5 times the guitar's, the bells peaked at 0.51, and the clap barely registered
  - After: plucked and struck sounds peak between 0.20 and 0.38, and held sounds sit between 0.046 and 0.064
- **Drums:** a sweep along a hand-drum string played all 7 drums in order
- **Menu:**
  - Fits on screen at 1280×800, 1440×760 and 1024×700
  - Shrinks from 50 options to 26 on a drum string, and grows back
- **Earlier stages:** every earlier test still passes

**Can't test from here:** whether each one sounds like its name, which only your ears can judge

## Experiments

- a) In `PLUCKED`, give the guitar `courses: [-4, 4]`, which makes a 12-string guitar
- b) Set the sitar's `bridge: 0.1`, then `bridge: 0.5`
- c) In the `bells` case, change `0.56` to `1`
- d) Change `NOTE_HOLD = 0.9` to `3`, then play the violin
- e) In the `voices` case, change the first formant from `800` to `400` and the second from `1150` to `2000`
  - That's roughly "ee" instead of "aah"
- f) Add your own drum to `DRUM_KITS.drums`, with a matching `case` in `playDrum`

## Question before Stage 9

- Rest a finger on a violin string and it keeps singing
- How could the app tell a finger resting on a string from one sweeping across it?

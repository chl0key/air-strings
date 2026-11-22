# Stage 11: beats

**Goal:** a backing beat that stays perfectly on time, built up one part at a time

**File:** [`index.html`](../index.html), section 26

## Answer to the last question

- Browser timers can fire late when the page is busy, so the beat never relies on them for timing
- The **audio clock** never wobbles
- Every 25 ms, a timer books every step due in the next 0.12 seconds, each at its exact time on the audio clock
- If a timer fires late, it doesn't matter: the notes it books were already booked by an earlier run, with time to spare
- This is called a **look-ahead scheduler**, and it's the standard way to keep time in a browser

## What you'll learn

**A pattern is text**
- Each part is a line of 16 characters, one per step, and a step is a sixteenth note
  - `x` is a hit, `X` a louder hit, `o` an open hi-hat, `-` a rest
  - Bass lines use numbers: `0` is the chord's root, `3` its third, `5` its fifth, `8` the root an octave up
- A part with two lines alternates them, one bar each, so the groove doesn't sound like a stuck loop

```
kick:  X-----x---x-----
snare: ----X-------X---
hats:  x-x-x-x-x-x-x-x-
```

**Tempo**
- BPM (beats per minute) sets the speed
- One beat lasts 60 ÷ BPM seconds, and a step is a quarter of a beat
  - At 90 BPM, a step lasts 0.167 seconds

**Swing**
- Every second step is pushed a little late
- A stiff, even beat starts to sound laid-back and human
- Boom bap and lo-fi use it, and house and trap don't

**Bass and chords follow your key**
- Each pattern plays one chord per bar, from a 4-chord progression
  - Major: I, V, vi, IV, the chords behind a huge number of pop songs
- The chords are built from the top bar's key and scale, the same way the strings' chord mode works
- So whatever you play on the strings fits the beat

**The six patterns**

| Pattern | Tempo | Feel |
| --- | --- | --- |
| Boom bap | 90 | 90s hip hop, swung |
| House | 122 | four-on-the-floor kick, offbeat open hats |
| Lo-fi | 78 | slow, heavily swung |
| Trap | 140 | rolling hi-hats, 808 bass |
| Reggaeton | 96 | the dembow rhythm |
| Maqsum | 100 | Arabic rhythm on hand drums: doum, tek, tek, doum, tek |

**Lights that follow the bookings**
- The 16 lights in the top left show the step that's playing now
- They read the list of booked steps rather than working it out from the tempo
- So they stay right when you change tempo mid-beat

## Read the code in this order

1. Section 26: `BEATS`, the patterns as text
2. Section 26: `startBeat` and `planAhead`, the look-ahead scheduler
3. Section 26: `playStep`, one step of every part
4. Section 26: `chordForBar` and `beatNote`, how bass and chords follow the key
5. Section 26: `buildBeatRows`, the panel

## Test it

1. Press **Beat** in the top bar
2. Press **Kick**: the beat starts
3. Add **Snare**, **Hats**, **Bass**, **Chords**, one at a time
4. Change **Pattern** while it plays
5. Play along on a string in the same key
6. Change **Key** in the top bar: the bass and chords move with it
7. With a keyboard, the space bar starts and stops the beat

## How I tested it

- **Starting:** switching on **Kick** started the beat, and all five parts switched on one at a time
- **Freezes:** I froze the page for 90 ms, like a slow frame
  - No steps were skipped, and the scheduler was still booked 0.23 seconds ahead
- **Loudness:** all five parts together peaked at 0.66, safely under the 1.00 where it would distort
- **Changes:** pattern and tempo changes applied straight away, and the tempo label updated
- **Space bar:** started and stopped the beat
- **The top bar:** still doesn't jump with the new **Beat** button
- **Earlier stages:** every earlier test still passes

**Can't test from here:** whether the grooves feel good

## Experiments

- a) Write your own kick line in `BEATS.boombap.kick`
  - Try `'X--x--X---x--x--'`
- b) Set `swing: 0.3` on lo-fi, then `swing: 0`
- c) Change `PROGRESSIONS.major` to `[0, 5, 3, 4]`, the "50s" progression: I, vi, IV, V
- d) Swap house's chords from `'organ'` to `'piano'`, or trap's bass from `'bass808'` to `'synth'`
- e) Add a seventh pattern of your own to `BEATS`
  - It shows up in the panel by itself

## Question before Stage 12

- The beat loops, but what you play on the strings vanishes as soon as it's played
- A looper records what you play and plays it back on every loop, so you can stack layers
- What does it need to remember about each pluck, so it can replay it later?

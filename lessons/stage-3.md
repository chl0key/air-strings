# Stage 3: one string, one sound

**Goal:** sweep a fingertip across a string and hear it

**File:** [`index.html`](../index.html), sections 13 to 16

## What you'll learn

**The browser's sound engine**
- The **Web Audio API** is a small sound studio built into every modern browser
- You build sound by connecting parts in a chain:
  - A source, such as an **oscillator**, which hums at a set pitch
  - A volume control, called a **gain**
  - The speakers, called the **destination**
- Browsers only let sound start after a click
  - So the camera and mouse buttons switch the sound on as well

**Pitch**
- Pitch is how many times a second the sound vibrates
- This string plays 330 vibrations a second, the note E above middle C
- Double the number and you go up one octave

**Envelopes**
- An **envelope** is the volume shape of one note over time
- A pluck rises in 5 ms, then fades over 0.8 seconds
- The fade is **exponential**, quick at first and then slower
  - Real strings fade the same way, so it sounds plucked
- Change the envelope and the same tone can sound like a pluck, an organ or a swell

**Which side of a line?**
- Draw an arrow along the string, and another from the string's start to your fingertip
- The **cross product** of the two arrows gives one number
  - Positive on one side, negative on the other, zero on the line
- Divide it by the string's length and the number is your distance from the string in pixels
- A pluck is a fingertip that was on one side and is now on the other

**Three guards against false plucks**
- **Dead band:** within 6 pixels of the string, a fingertip counts as on neither side
  - Tracker wobble can't flip it back and forth
- **Between the ends:** a crossing past either peg doesn't count
- **Cooldown:** one fingertip can't pluck twice within a tenth of a second

**Where it was plucked, and how hard**
- The code finds the exact spot where your fingertip crossed, from 0 at one peg to 1 at the other
  - For now that spot only moves the wobble, and in Stage 6 it picks the note
- Speed sets loudness: pixels moved divided by the time taken

**One code path for mouse and fingertips**
- The mouse pointer is fed into the same `checkPluck` as each fingertip
- So testing with the mouse tests the real pluck code

## Read the code in this order

1. Section 13: `startAudio` and `beep`, the sound
2. Section 15: `sideOf`, the cross product, which is three lines long
3. Section 15: `checkPluck`, the three guards
4. Section 10: the loop, where each open hand's index fingertip goes to `checkPluck`
5. Section 14: `drawString`, the wobble after a pluck

## Test it

**With the mouse first**
1. Press **Use the mouse instead**
2. Sweep the pointer across the string
   - One beep, and the string shakes most where you crossed it
3. Sweep fast, then slow
   - Fast is louder
4. Wiggle the pointer right on the string
   - Silence
5. Sweep across past either peg
   - Silence

**Then with the camera**
1. Reload, press **Turn on camera**
2. Sweep your index fingertip across the string
3. Pinch, then move the pinch across the string
   - Silence, because a pinching hand can't pluck
   - In Stage 5 a pinch on a string will grab it instead

## How I tested it

- Made-up fingertip paths, fed straight into the pluck code:
  - A fast sweep plucked once, at the right spot
  - A slow sweep that paused on the line still plucked once
  - Wobbling inside the dead band never plucked
  - Crossing past either peg never plucked
  - Back and forth within 0.03 seconds plucked once, and at normal speed twice
  - A slow sweep played quieter than a fast one
- The real mouse, through the real button, plucked once per sweep
- A sound meter on the output confirmed the beep reached the speakers
- The camera and the pinch panel still work on the hands photo
- I can't hear it from here, so tell me how it sounds

## Experiments

- a) Change `osc.type = 'triangle'` to `'sine'`, then `'square'`, then `'sawtooth'`
- b) Change `osc.frequency.value = 330` to `165`, then to `660`
- c) Change the fade in `beep` from `t + 0.8` to `t + 3`, then to `t + 0.1`
- d) Change the attack from `t + 0.005` to `t + 0.3`
  - The pluck turns into a swell
- e) Set `DEAD_BAND = 0`, wiggle the mouse on the string, and listen
- f) Move the string by changing the numbers in `STRING`, such as a vertical line from `{ x: 0.5, y: 0.2 }` to `{ x: 0.5, y: 0.8 }`

## Question before Stage 4

- Experiment a gives you 4 tones, and none of them sounds much like a guitar
- What does a real string do after it's plucked that a steady tone doesn't?

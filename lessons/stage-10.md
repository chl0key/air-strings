# Stage 10: the air pad

**Goal:** shape the sound of the whole mix with one hand while the other plays

**File:** [`index.html`](../index.html), section 25, plus `updateGrabs` in section 10

## Answer to the last question

- One drag has two directions you can measure separately:
  - How far down or up
  - How far sideways
- Down controls one effect, up another, and sideways a third
- A diagonal drag moves both measures at once, so it mixes two effects

## What you'll learn

**Effects on the whole mix**
- Until now, every sound went from the master volume straight to the limiter
- Now three effects sit in between, and they act on everything at once:

```
master ──► thin (highpass) ──► dark (lowpass) ──► limiter ──► speakers
   └──► echo send ──► delay ◄─► feedback ──┘ (into thin)
```

**Darker: a lowpass filter**
- Lets low sounds through and cuts the high ones
- Drag down and its cutoff falls from 20,000 Hz (everything passes) to 360 Hz (muffled, like music through a wall)

**Thinner: a highpass filter**
- The opposite: cuts the lows
- Drag up and its cutoff rises from 20 Hz to 1,200 Hz (tinny, like an old radio)

**Echo: a delay with feedback**
- A **delay** plays the sound again 0.375 seconds later
- **Feedback** sends the delayed sound back into the delay, so it repeats again and again
- Each repeat goes through a softening filter and comes back quieter, so the echoes fade like real ones
  - With feedback at 1 or more, the echoes would never fade, and they'd get louder each time round
- Drag sideways to send more of the mix into the echo

**Why the filters move on a curve**
- Our ears hear pitch in ratios: 200 Hz to 400 Hz sounds like the same step as 2,000 Hz to 4,000 Hz
- So the cutoff is set with `Math.pow`, which makes each bit of drag move it by the same ratio
- A straight-line mapping would put all the audible change in the first few pixels

**When a pinch becomes the air pad**
- A pinch in free air starts as `'free'`
- If the other hand isn't pinching and this one drags more than 45 px, it becomes `'fx'`
- If the other hand pinches too, the pad lets go and the two hands stretch a string instead
- With the mouse, hold **Shift** and drag, because a plain drag already makes strings

**Glide**
- `setTargetAtTime` moves each effect smoothly toward its new value instead of jumping
- Jumps would click
- Letting go glides everything back to normal over about a third of a second

## Read the code in this order

1. Section 25: `buildAirPad`, the three effects and how they connect
2. Section 25: `moveAirPad`, from drag distance to effect amounts
3. Section 25: `setAirPad`, from amounts to filter settings
4. Section 10: `updateGrabs`, where a free pinch becomes the pad
5. Section 25: `drawAirPad`

## Test it

1. Make a string, set it to **Organ** or **Strings**, and rest a finger on it to hold a note
2. With your other hand, pinch empty air and drag down slowly
   - The note goes muffled
3. Drag up past where you started
   - It goes tinny
4. Drag sideways while plucking a **Marimba** string
   - Echoes
5. Let go
   - Everything returns to normal

## How I tested it

- **Each direction** set its filter exactly where intended:
  - Down 250 px: lowpass at 360 Hz
  - Up 250 px: highpass at 1,200 Hz
  - Sideways 270 px: echo send at 0.7
- **Letting go** returned everything to normal
- **On a real sound** (an organ chord):
  - Darker cut the highs by 31 dB
  - Thinner cut the lows by 23 dB
  - A cut of 10 dB already sounds about half as loud
- **Echo:** 1.3 seconds after a marimba note, it was 19 to 29 times louder with echo on, because the repeats were still going
- **Two hands:** a second pinch turned the pad into a string stretch, and the string was made
- **Mouse:** Shift-drag worked the pad, and a plain drag still made a string
- **A fix along the way:** the live percentages first sat at the end of the cross, right under your hand
  - They now sit beside the starting point
- **Earlier stages:** every earlier test still passes

**Can't test from here:** how it sounds and feels to perform with

## Experiments

- a) In `buildAirPad`, change `delay.delayTime.value = 0.375` to `0.12`, a slapback echo
- b) Change the feedback range in `setAirPad` from `0.35 + 0.25 * echo` to `0.35 + 0.55 * echo`
  - Careful: near 1, the echoes stop fading
- c) Change `padReach: 220` to `120` for a more sensitive pad
- d) Change `0.018` in `setAirPad` to `0.1`, so darker doesn't go as dark
- e) In `moveAirPad`, swap the effects for up and down

## Question before Stage 11

- Everything so far happens when you touch something
- A beat keeps playing on its own, perfectly on time, for minutes
- Browsers are busy drawing the screen and tracking your hands, so their timers can run late
- How could a beat stay on time anyway?

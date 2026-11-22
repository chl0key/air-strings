# Stage 2: detect a pinch

**Goal:** the app knows when your thumb and index finger touch, on each hand separately

**File:** [`index.html`](../index.html)

## What you'll learn

**Measuring a pinch**
- A pinch is the gap between the thumb tip (point 4) and the index tip (point 8)
- In pixels, that gap shrinks as you lean back from the camera
  - So a fixed pixel gap would pinch at one distance and fail at another
- The fix: divide the gap by the size of the hand
  - Lean back and both shrink together, so the ratio stays the same
- Hand size is the bigger of two palm measures, length and width
  - Tilting your hand shortens one of them on camera, but rarely both

**Two thresholds**
- The pinch closes when the ratio drops below 0.22
- It only opens again when the ratio rises above 0.32
- Between the two, nothing changes
- With a single line, a hand hovering on it would flicker between pinched and open
  - Engineers call this **hysteresis**, and your home thermostat uses the same trick

**Waiting two frames**
- The tracker sometimes misreads a single frame
- A change only counts once it lasts two frames in a row
  - That's about 1/30 of a second, too short for you to notice

**Smoothing**
- The tracked points wobble a few pixels even when your hand is still
- Each frame, every point moves halfway to its new position instead of all the way
  - Less wobble, at the cost of a tiny lag

**Keeping track of which hand is which**
- In Stage 1 the colors could swap, because MediaPipe doesn't keep hands in the same order
- Now each hand has a slot, and the app remembers where each wrist was last frame
- Each new reading goes to the slot whose wrist was closest
- A slot is cleared once its hand has been missing for a quarter of a second

## Read the code in this order

1. Section 3: the settings, with the numbers you can tune
2. Section 9: `updatePinch`, the heart of this stage
3. Section 8: smoothing, which is three lines long
4. Section 7: `assignHands`, which keeps each hand in its own slot
5. Section 11: drawing the rings and the dashed line

## Test it

1. Open the page and turn on the camera
2. Hold up one hand with your fingers relaxed
   - Watch its meter in the bottom-left panel
   - The number should sit above the right-hand white mark
3. Touch your thumb tip to your index tip
   - The bar fills with the hand's color
   - The ring around the pinch point turns thick, with a quick burst
4. Pinch with both hands
   - A dashed line joins them and follows your hands as you move
5. Lean back from the camera and pinch again
   - It should still work

## Tune it to your hands

- Watch the numbers as you pinch and relax
- If pinches don't register, move **Pinch closes below** to the right
- If a relaxed hand counts as a pinch, move it to the left
- If a pinch won't let go, move **Pinch opens above** to the left
- Tell me the numbers that feel right, and I'll make them the defaults

## How I tested it

- **The real tracker** read a photo of two open hands at 0.34 and 0.40, both correctly open
  - My first defaults (0.30 and 0.45) were too close to a relaxed hand, so I lowered them
- **Made-up hands** fed straight into the code checked each rule:
  - One frame of closing is ignored, and two frames count
  - 0.27 sits between the marks and keeps the pinch closed
  - Swapping the order of the hands leaves each one in its slot
  - A half-size hand, as if you'd leaned back, gives the same ratio
  - A hand that vanishes frees its slot after a quarter of a second
- I can't test with a moving hand in front of a real camera, so your test is the real one

## Experiments

- a) Set `smoothing: 0.15`, then `smoothing: 1`, and wave your hand
  - Which one feels laggy, and which one feels jittery?
- b) Set `holdFrames: 1`, then `holdFrames: 6`, and pinch quickly
- c) Make the two thresholds equal by dragging the sliders together (they stop 0.05 apart), then hover your pinch near the line
- d) Change `PALM_WIDTH_SCALE` to `1`, then tilt your hand toward the camera and pinch

## Question before Stage 3

- The dashed line between two pinches is nearly a string
- Your fingertip will pluck it by crossing from one side to the other
- How could the code tell which side of a line a point is on?

# Stage 1: camera on, dots on your fingertips

**Goal:** see your hands tracked live, with a dot on every fingertip

**File:** [`index.html`](../index.html)

## What you'll learn

**Web page anatomy**
- One HTML file holds three languages
  - HTML: what's on the page (a button, a canvas)
  - CSS: how it looks (colors, fonts, sizes)
  - JavaScript: what it does
- There's no build step
  - Open the file in Chrome and it runs

**The canvas**
- A `<canvas>` is a blank picture the code draws on, pixel by pixel
- Every frame, the code wipes it and redraws everything
  - The same trick as a flip book

**The camera**
- `getUserMedia` is the browser's way of asking for the camera
- Chrome only allows it on secure pages
  - Secure pages are https addresses, or a file opened from your own computer
- The `<video>` element receives the camera feed but stays hidden
  - The code copies each frame onto the canvas

**The hand tracker**
- **MediaPipe Hand Landmarker** is a small AI model from Google
- It runs inside the browser tab, so the camera picture never leaves your Mac
- You give it a picture and get back 21 points per hand
  - Point 0 is the wrist
  - Points 4, 8, 12, 16 and 20 are the fingertips, thumb to pinky
- Each point is a fraction of the picture, from 0 to 1
  - The code turns those fractions into screen positions

**The mirror**
- A webcam sees you the way another person would
- Without a flip, moving your right hand right would move it left on screen
- The code flips both the picture and the points, so the screen works like a mirror

**The loop**
- `requestAnimationFrame` runs a function once per screen refresh, about 60 times a second
- Each pass checks for a new camera frame, tracks it, and then draws

## Read the code in this order

1. Section 2 in the script: the 21 points and which ones get joined with lines
2. Section 5: the button, the camera and the tracker loading
3. Section 6: `toScreen`, which turns fractions into screen positions and does the mirror flip
4. Section 7: `loop` and `draw`

## Test it

1. Open the page in Chrome on your Mac
2. Press **Turn on camera** and allow the camera
3. Wait a few seconds while the tracker downloads, about 8 MB the first time
4. Hold up both hands

**Should happen**
- A faint mirrored picture of you
- A thin skeleton over each hand
- Amber dots on one hand's fingertips and coral dots on the other's
- A readout in the bottom left, showing hands found and tracking speed
  - Expect 20 to 60 times a second on a Mac

**Known quirk**
- The two colors can swap between hands when a hand leaves the frame and comes back
  - MediaPipe doesn't promise to keep hands in the same order
  - Stage 2 fixes this, because a pinch needs to remember which hand started it

## Experiments

Change one thing, save, reload the page:
- a) Change `--lamp` in the CSS to another color
- b) Change `globalAlpha = 0.35` in `draw()` to `1`, then to `0`
  - At `0` you're down to dots on a dark stage, which is closer to the finished look
- c) Change `numHands: 2` to `1`
- d) Make the fingertip dots bigger: change `dot(pts[k], 9)` to `dot(pts[k], 20)`
- e) Remove the mirror flip in `toScreen` by changing `innerWidth - (...)` to just `(...)`
  - Then wave your right hand and see which way the dots go

## Questions to answer before Stage 2

- Which point numbers would you use to measure a pinch?
- Why would a fixed pinch distance in pixels go wrong as you step back from the camera?

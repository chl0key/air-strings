# Air Strings

**Status:** 🛠 in progress
**Started:** 2026-11-22

## What is it?

An instrument you play in the air with a webcam.
Pinch with both hands and pull them apart to stretch a string, then sweep a fingertip across it to play it.

## Why?

To learn how hand tracking and browser sound work by building one step by step, with Claude teaching each stage.
Goal: everything Hashem Al-Ghaili's Music Maker does, then my own additions on top.
His repo has no license, so this matches what it does, in my own code, name and look.

## Next step

Test Stage 11: press Beat, switch on the parts one at a time, try each pattern, play along on a string.
Then Stage 12: the looper, follow chords, snap to the grid.

## Stages

| Stage | What it adds | Lesson |
| ----- | ------------ | ------ |
| 1 | Camera on, dots on your fingertips | [stage-1.md](lessons/stage-1.md) |
| 2 | Detect a pinch | [stage-2.md](lessons/stage-2.md) |
| 3 | One string that beeps when a fingertip crosses it | [stage-3.md](lessons/stage-3.md) |
| 4 | Precise pinch, pinch jobs, make, drag and delete strings | [stage-4.md](lessons/stage-4.md) |
| 5 | Notes and scales along each string, longer strings hold more notes, chord mode | [stage-5.md](lessons/stage-5.md) |
| 6 | Real string sound (Karplus-Strong) and reverb | [stage-6.md](lessons/stage-6.md) |
| 7 | The string menu: scale, key, notes or chords, octave, by pinching or hovering | [stage-7.md](lessons/stage-7.md) |
| 8 | All 24 instruments, drums included, chosen from the string menu | [stage-8.md](lessons/stage-8.md) |
| 9 | Hold to sustain or roll, pinch-and-pull to bend the pitch | [stage-9.md](lessons/stage-9.md) |
| 10 | Air pad: one-hand pinch and drag sweeps the filter and echo | [stage-10.md](lessons/stage-10.md) |
| 11 | Beats: 6 patterns, one part at a time, on a steady clock | [stage-11.md](lessons/stage-11.md) |
| 12 | Looper layers, snap to the grid, "follow chords" | - |
| 13 | Recording: video or sound, MP4/WebM, MP3, WAV, stems | - |
| 14 | Saved setups and ready-made layouts | - |
| 15+ | My additions | - |

## Later

- **Air pad: latch it.** Drag to set the sound, let go, and it stays while you play with both hands. Pinch the pad's dot to turn it off
- **Air pad: make it visible.** Dim the screen when the sound goes darker, give strings trails when there's echo

## Notes

- Live at https://chl0key.github.io/air-strings/ (GitHub Pages, from `main`)
- Each stage is pushed straight to `main` and goes live about a minute later
- Reference: https://github.com/Hashem-Al-Ghaili/music-maker
- Needs Chrome or Edge, good light, and hands about 40-80 cm from the camera
- Hand tracking: MediaPipe Hand Landmarker 0.10.14, loaded from a CDN
- One HTML file, no build step

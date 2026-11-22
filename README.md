# Air Strings

**Status:** 🛠 in progress
**Started:** 2026-11-22

## What is it?

An instrument you play in the air with a webcam.
Pinch with both hands and pull them apart to stretch a string, then sweep a fingertip across it to play it.

## Why?

To learn how hand tracking and browser sound work by building one step by step, with Claude teaching each stage.
Inspired by Hashem Al-Ghaili's Music Maker, but this is my own version with my own code.

## Next step

Test Stage 2: pinch with each hand, then both, and tune the two sliders.
Then start Stage 3: one string that beeps when a fingertip crosses it.

## Stages

| Stage | What it adds | Lesson |
| ----- | ------------ | ------ |
| 1 | Camera on, dots on your fingertips | [stage-1.md](lessons/stage-1.md) |
| 2 | Detect a pinch | [stage-2.md](lessons/stage-2.md) |
| 3 | One fixed string that beeps when you cross it | - |
| 4 | Karplus-Strong, so the beep sounds like a plucked string | - |
| 5 | Pinch with both hands to create strings | - |
| 6 | Notes and scales along each string | - |
| 7+ | My twist: to be decided | - |

## Notes

- Live at https://chl0key.github.io/air-strings/ (GitHub Pages, from `main`)
- Each stage arrives on a branch, and I merge it into `main` to put it live
- Reference: https://github.com/Hashem-Al-Ghaili/music-maker
  - The repo has no license, so we learn from its ideas and write our own code
- Needs Chrome or Edge, good light, and hands about 40-80 cm from the camera
- Hand tracking: MediaPipe Hand Landmarker 0.10.14, loaded from a CDN
- One HTML file, no build step

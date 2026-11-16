# Monarch Training Console

A phone-only, offline workout logger with a starship-console look. The app ships **empty**: it runs whatever training program you load from a private program file. Programs, progress and logs stay on the device.

Application created by S. Scott Larsen · MONOC Engineering

## Features

- **Command:** a status ring for weeks and days (complete, in progress, on duty, missed, scheduled), plus the selected day's targets and Engage.
- **Training:** a timeline session view with a pace clock (start/pause), one-tap set logging, 5-lb weight dial, rep steppers, and inline "impulse burst" cardio timers.
- **Logs:** per-exercise history with progress charts.
- **Engineering:** import and export for the **program**, the **logs**, or **everything**. Log imports merge and ask before overwriting anything. Every export can be saved as a file or copied as text.
- Works fully offline after the first load (service worker). Data lives in localStorage with an IndexedDB mirror.

## Install on iPhone

1. Serve this folder over HTTPS (for example, GitHub Pages from `main`).
2. Open the URL in **Safari** → **Share** → **Add to Home Screen**.
3. Open it once from the icon while online. After that it runs offline.
4. In the app, tap **Load training file** and choose your program file.

## Program files

Program files (`*.program.json`) are **not** stored in this repo. A program file is JSON with:

| Field | Meaning |
|---|---|
| `format` | always `"monarch-program"` |
| `id`, `name`, `subtitle` | identity; progress is tracked per `id` |
| `weeks`, `sessionMinutes` | program length and the session time budget |
| `cycles[]` | `{key, name, weeks:[start,end], deloadLast}` |
| `tiers`, `tierStep` | rep ranges per week-in-cycle for each exercise kind (last entry = recovery week) |
| `weekNotes`, `techniques`, `safetyRule` | banner text per week-in-cycle |
| `days[]` | `{code, name, armDay, warmup:[...]}` |
| `plan` | per cycle key: one `{core:[ids], add:[ids]}` per day |
| `exercises` | `id → {name, kind, load, sets, start, inc, fixed, unit, cue, how:[steps]}` |
| `burstMoves`, `standingOrders`, `sickbay` | optional text lists |

Logs are keyed by exercise `id`, so history and suggested weights carry across programs that reuse the same ids.

## Updating

Edit the files, bump `VERSION` in `sw.js`, then push. Data on the phone is not affected.

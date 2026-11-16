# Measure Stack handoff

The game drops note-value bars into measures, scores full rows, plays a rhythm, pauses on tab hiding, and stores a best score per level. The public helpers include the next-kind value and a note drawing so a student can add a preview without core changes. No feature-menu item is delivered.

## Deviations and added copy

The self-check adds Thai test names and the labels `ตัวโน้ต`, `ระดับ`, `บันทึกได้`, `บันทึกไม่ได้` and `ส่วนเสริม` for the table counts, storage status and extension errors required by the shared rules. The board also has the accessible label `ห้องเพลง` and the HUD has `ปุ่มควบคุม`. CSS values are exposed as custom properties; SVG paths retain their drawing coordinates.

`Core.nextKind` returns `{ state, kind }` to keep the weighted bag immutable; the spec does not prescribe its return shape. It excludes pieces wider than the configured row when refilling the bag, so the proposed 3/4 extension remains playable even while a level includes whole notes. Explicitly spawning an oversized piece still ends the game. Rhythm schedule items add a `rest` boolean to support the required silent-rest fixture.

## Checks run

- `cd tests && node --test measure-stack*.test.cjs`: 11 tests passed, covering core, fixed-seed bag, silent start, pause, row clearing and scoring, a student-added piece, blocked storage, a rest fixture, preview extension, event details and an extension throwing on lock.
- `node tools/measure-browser-check.mjs`: local Chrome checked the self-check, idle and pause loop, level lock, 320 × 640 board/control fit, ghost landing row, controls, silent play to game over, best-score reload, and no console output or network request during that run. It also produced note-shape images for both colour schemes, which were visually inspected.
- `node tools/shot.mjs measure-stack`: generated the six required screenshots; each initial-screen image and a 320 px playing image were inspected for layout and readability.

- `node tools/contrast-check.mjs`: visible text passed contrast checks in sampled playing or selected states in both light and dark schemes. This does not cover every possible student modification.

- `node tools/audio-browser-check.mjs`: both audio-enabled games completed in headless Chrome with zero console messages and network requests. This does not establish audible speaker quality.

## Not verified

Real sound on speakers and subjective timing, touch feel on a real phone, the school lab's Windows PCs, and deployment to GitHub Pages were not tested. A separate private Sites preview was deployed for the instructor; the Sites service reported success. Browser tests simulate pointer and keyboard input rather than the physical feel of holding the buttons. The game has not been exhaustively tested against arbitrary student changes to timing, note widths, and levels. For a 3/4 configuration, the weighted bag skips pieces wider than the row; an explicit attempt to spawn such a piece still ends the game.

## v2 upgrade

Added Marathon, Target Rhythm and 12-Measure Sprint, three meters, a two-piece
queue and hold, note and rest notation, combo and target scoring, a rhythm
lesson after each clear, result statistics, medals and saved preferences.

- The 11 Measure Stack baseline tests passed before editing and after formatting.
- `cd tests && node --test --test-reporter=spec measure-stack*.test.cjs packaging.test.cjs`:
  18 passed. The older score fixtures include the new full-sound bonus and the
  seeded bag replaced a synthetic whole-note-only level to keep target validation
  meaningful. Tests cover v1 migration, hold and queue, the target bonus,
  the zone E result-history probe, and a throwing legacy handler.
- `node tools/measure-v2-browser.mjs`: all three modes reached their end states
  with and without Web Audio in headless Chrome. The target rhythm and counting
  appeared as expected, sprint reached 12 and the target board and its five
  controls fit at 320 × 640. The app made no network requests and logged nothing.
  All 36 screen captures were inspected by contact sheet.
- Unspecified labels include `ไปหน้าแรก`, `เวลา` and the self-check counts.
  Physical touch, audible payoff and Windows lab devices remain unverified.

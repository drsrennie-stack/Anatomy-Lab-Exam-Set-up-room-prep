# Accessibility Compliance Notes

**Project:** BIO 004 Lab Practical toolkit
**Files covered:** bio004-practical-answer-sheet.html, bio004-room-map-timer.html, bio004-course-hub.html, room-map-arrows-new.png, and the station-card-generator.html / exam-builders.html pair in the teaching-resources repo
**Date:** August 30, 2026 (supersedes the June 21, 2026 draft)
**WCAG version / target:** WCAG 2.2, Level AA minimum. AAA on primary text contrast.
**Type / fonts:** Open Sans (display) with Plus Jakarta Sans (body), fallback stack Open Sans, Plus Jakarta Sans, system-ui, sans-serif. No italics, no Lora, no DM Sans.
**Palette as built:** navy #0B1530, navy-deep #060A18, navy-tint #EAECEF, navy-15 rgba(11,21,48,.15), ink-muted #4F576A, terra #8B3A2E, white, off-white #FAFAF9. Gold is retired. No sage, no cream, no off-palette grays.
**System:** the August 30, 2026 house card system. White ground, 0.5px navy-15 hairline borders, no lift shadows, 8px card radius, 4px button radius, 3px badge radius, terra as the only accent.

## 1. Answer sheet (bio004-practical-answer-sheet.html)

Single HTML that works as an app. Planner view lets the instructor pick a system or unit (growable menu), a question stem scoped to that system (growable menu), and an answer key per question. A live balance panel tallies ID vs Beyond-ID (auto-classified from the stem), counts questions per system, and flags any system carrying well above the average. Student view and print output show blank lines only.

Sheet mode setting:

- Combined practical: 26 stations, questions 1 to 52.
- Wet Lab: 13 stations, questions 1 to 26.
- Dry Lab: 13 stations, questions 1 to 26.

Each mode stores its data separately, prints its lab name in a terra band, and feeds card printing and CSV export for its own questions. Stations run down column 1, then down column 2.

An optional bonus section numbers attached and standalone bonus questions B1, B2 and so on, and prints them across the foot of the sheet.

Outputs: one-page student sheet, one-page answer key on the identical grid, half-sheet station cards, and CSV export.

The CSV columns are Lab, Station, Question, System, Stem, Answer, Type, Source, and that order is a contract: the Station Card Generator in the teaching-resources repo reads this export to lay out printable cards. Bonus rows carry System "BONUS" and put the bonus label (B1, B2) in the Station column, because the generator groups cards by Station and prints that value as the card numeral. Source is a trailing column recording where an attached bonus came from; the generator matches headers by name and ignores it.

## 2. Room map and rotation timer (bio004-room-map-timer.html)

Projector tool. Room picker: Built-in Anatomy Room (fixed SVG replica of the lab with terra rotation arrows 1 to 52, START badge on 1 and 2, FINISH badge on 51 and 52, Desk), plus Dry Lab and Wet Lab slots that accept an uploaded PNG or JPG map (downscaled and stored in localStorage, switchable, removable).

Flexible timer: editable interval (minutes and seconds, default 1:20) and optional rotation count (blank loops forever, or stop after N intervals). Start doubles as Pause and Resume; spacebar toggles; Reset returns to the interval; shows "interval X of N." role="timer" on the clock, with no per-second aria-live, which would flood a screen reader.

Chimes: five synthesized options (Songbird trill, Morning birds, Wind chimes, Warm marimba, Lake loon call) via Web Audio, no audio files, on and mute toggle, volume slider, Test button. Audio context created and resumed only on a user gesture. Sound is supplementary; all timer state is shown visually, so nothing depends on audio alone.

## 3. Color contrast audit (each pair, ratio, result)

| Element | Foreground | Background | Ratio | Result |
|---|---|---|---|---|
| Body text, station numbers, answer rules | Navy #0B1530 | White | 18.0:1 | AAA |
| Body text on page ground | Navy #0B1530 | Off-white #FAFAF9 | 17.3:1 | AAA |
| Eyebrow, field labels, question numbers | Terra #8B3A2E | White | 7.7:1 | AAA |
| Lab band text | White | Terra #8B3A2E | 7.7:1 | AAA |
| Button text, Answer Key banner, Bonus band | White | Navy #0B1530 | 18.0:1 | AAA |
| Completed-state text | Navy #0B1530 | Navy-tint #EAECEF | 15.2:1 | AAA |
| De-emphasized text (hints, card footers, brand subline) | Ink-muted #4F576A | White | 7.2:1 | AAA |
| Focus ring | Terra #8B3A2E | White or any panel | 7.7:1 | Pass (1.4.11) |
| Panel hairline border | Navy-15 rgba(11,21,48,.15) | White | decorative only | n/a |

Notes: gold is retired from this toolkit. The Answer Key banner and the Bonus band, which were gold, are now navy with white text, which keeps them distinct from the terra lab-name band. On the printed station cards the bonus placard inverts the regular one, terra border and terra numeral against a navy eyebrow and navy pill, so bonus reads apart without a third color. Terra #8B3A2E at 7.66:1 clears AAA with little headroom and must never be tinted lighter for text. The 0.5px navy-15 hairline is decorative framing only; no information is carried by it alone.

The de-emphasized ink is #4F576A rather than the navy-at-55-percent the card spec names. That value flattens to #797E8D, 4.05:1 on white, which fails the AA floor for the small text it was written for.

## 4. Keyboard, screen reader, motion

All controls are native button, input and select, reachable in source order, with a skip link and a visible terra focus ring (3px outline, 2px offset, plus a soft navy halo). Every generated field carries an accessible name naming its station and question number ("Station 7 question 14 answer key"). The station grid, the balance panel and the save note use aria-live where content updates. The view toggle is a labelled group with aria-pressed on both buttons. Reduced motion is respected (transitions disabled under prefers-reduced-motion). The iframe height-sender (postMessage plus load and resize listeners plus ResizeObserver) is present. The one internal link, the brand bar to the course hub, carries target="_top".

## 5. Print and one-page check

@page Letter portrait, 0.35in by 0.45in margins. Balanced columns (column-fill: balance) so the block ends with its content and the signature line cannot be pushed onto a second sheet.

The student sheet and the answer key print on an identical two-column grid: 13 stations per column for the combined practical, so the sheet folds down the middle, and the key overlays the student sheet station-for-station when grading. Every ruled line is a fixed-height box with its text sitting on the rule, rather than a baseline-aligned flex row, because a line holding typed text otherwise sits about 3.5px taller than an empty one and the two printouts drift apart down the column.

Verified in headless Chromium, with Plus Jakarta Sans and Open Sans installed locally so the measurements use the real faces rather than a fallback, by printing to PDF and counting pages. All one page: combined student, combined key, wet student, wet key, dry student, dry key, combined key with maximum-length answers, and every sheet carrying a bonus section of 4, 6 and 8 questions. Station block height measured identical at 63.5px on the student sheet and the key.

Writing-line heights: 18px (about 4.8mm) on the combined practical, which is the largest that holds 52 answer lines on one page in two columns; 36px (about 9.5mm) on Wet and Dry, which have half the stations and room to spare.

Station cards print two half-sheets per page, 13 pages for a 26-station practical.

Planner fields and the balance panel are hidden in print. The lab name and the "Note" cue label are forced on for the student print.

## 6. Student privacy

No student names, IDs or grades are stored. Only the instructor's systems, stems, answer keys, region notes (per sheet mode), bonus questions, uploaded room images and timer settings persist, in localStorage on the local device. The Name field is a blank print line and is never captured.

## 7. Known limitations and remediation

- localStorage is per-browser and per-device; planning data, room uploads and settings do not sync across machines.
- Answer key entries longer than 62 characters would be clipped by the fixed-height print line. The planner flags any such field with a terra outline and a tooltip while typing, so the truncation cannot happen silently, but the guard is a character count rather than a true width measurement and is deliberately conservative.
- One-page fit is verified in Chromium. Other print engines round differently; confirm in print preview before a class run.
- Live screen-reader pass (VoiceOver or NVDA) not yet run on instructor hardware; recommended before first classroom use.
- Uploaded room maps are the instructor's own images; their internal contrast and legibility are outside this audit.

## 8. House system alignment, August 30, 2026

These files were moved onto the house card system in full:

- Gold #C9A14A retired. Terra took over the labels and rules it held; the Answer Key banner, the Bonus band, the bonus field accents and the bonus card pill went navy, since navy is what keeps bonus distinct from the terra lab band.
- Focus ring changed from a gold outline with a navy-deep halo to a terra outline with a soft navy halo.
- Panels moved from a 1px navy-tint border with a lift shadow to a 0.5px navy-15 hairline with no shadow.
- Radii normalized: 8px on cards and panels, 4px on buttons and inputs, 3px on badges and chips.
- Open Sans loaded for display type (h1, panel headings, the large station numerals on the cards), Plus Jakarta Sans kept for body.
- The brand bar lost its bottom rule, per the standing no-bookend-borders rule.

One item is deliberately left alone. The three-figure mark still carries its own navy, terra and gold fills. Changing a brand mark is a separate decision from a styling pass, and the mark is already on the open list of things to reconcile.

Printed station placards keep a visible 1.5px navy edge rather than the 0.5px hairline. They are physical cut-and-place objects, and a hairline does not survive a laser printer as a usable edge.

## Reviewer

Drafted by Claude (Cowork) for Dr. Sharilyn Rennie. Instructor review pending.

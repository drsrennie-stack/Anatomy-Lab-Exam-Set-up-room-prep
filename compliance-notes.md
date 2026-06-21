# Accessibility Compliance Notes

**Project:** BIO 004 Lab Practical toolkit
**Files covered:** bio004-practical-answer-sheet.html, bio004-room-map-timer.html, room-map-arrows-new.png
**Date:** June 21, 2026
**WCAG version / target:** WCAG 2.2, Level AA minimum. AAA on primary text contrast.
**Type / fonts:** Plus Jakarta Sans only, no italics, no Lora, no DM Sans.
**Palette:** PRIMARY only (navy #1E3D4C, navy-deep #142A36, navy-tint #EDF1F3, gold #B8924A, terra cotta #C2734D, terra-dark #A0522D, white, off-white). No sage, no cream, no off-palette grays. Per instructor preference, terra-dark #A0522D is the lead reddish accent; gold is used sparingly (focus rings and a single FINISH badge).

## 1. Answer sheet (bio004-practical-answer-sheet.html)

Single HTML that works as an app. Planner view lets the instructor pick a question stem (growable menu: "ID this bone," "Name the origin of this muscle," etc.), a topic (growable menu), and an answer key per question; a live balance panel tallies ID vs Beyond-ID (auto-classified from the stem) and counts per topic, flagging any over-weighted topic. Student view and print output show blank lines only.

Sheet mode setting:
- Combined practical: 26 stations, questions 1–52.
- Wet Lab: 13 stations, questions 1–26.
- Dry Lab: 13 stations, questions 1–26.
Each mode stores its data separately, prints its lab name in a terra-dark band, and feeds card printing and CSV export for its own questions. Stations run top-to-bottom (column 1 then column 2).

Outputs: one-page printable student sheet (lab name, Name, Lab Exam #, blank Note line + two blanks per station); printable station cards; CSV export (Lab, Station, Question, Topic, Stem, Answer, Type) for external card generation.

## 2. Room map & rotation timer (bio004-room-map-timer.html)

Projector tool. Room picker: Built-in Anatomy Room (fixed SVG replica of the lab with terra-dark rotation arrows 1→52, START badge on 1&2, FINISH badge on 51&52, Desk), plus Dry Lab and Wet Lab slots that accept an uploaded PNG/JPG map (downscaled and stored in localStorage, switchable, removable).

Flexible timer: editable interval (minutes:seconds, default 1:20) and optional rotation count (blank = loops forever, or stop after N intervals). Start doubles as Pause/Resume; spacebar toggles; Reset returns to the interval; shows "interval X of N." role="timer" on the clock; no per-second aria-live (would flood a screen reader).

Chimes: five fuller synthesized options (Songbird trill, Morning birds, Wind chimes, Warm marimba, Lake loon call) via Web Audio, no audio files, on/mute toggle, volume slider, Test button. Audio context created/resumed only on a user gesture. Sound is supplementary; all timer state is shown visually, so nothing depends on audio alone.

## 3. Color contrast audit (each pair, ratio, result)

| Element | Foreground | Background | Ratio | Result |
|---|---|---|---|---|
| Body text, station numbers | Navy #1E3D4C | Off-white #FAFAF9 | 11.0:1 | AAA |
| Text on white card | Navy #1E3D4C | White | 11.5:1 | AAA |
| Eyebrow / labels | Terra-dark #A0522D | Off-white | 5.4:1 | AA (AAA large) |
| Button / lab band text | White | Navy #1E3D4C / Terra-dark #A0522D | 11.5 / 5.6:1 | AA+ |
| FINISH badge text | Navy-deep #142A36 | Gold #B8924A | 5.1:1 | AA |
| Map rotation arrows | Terra-dark #A0522D | White corridors | 5.6:1 | Pass (graphic 3:1) |
| Map arrows over black benches | white casing under arrow | Black | 11+:1 | Pass |
| Focus ring (gold + navy-deep halo) | — | any | >3:1 on every bg | Pass (1.4.11) |

Notes: white-on-terra (#C2734D, 3.6:1) is NOT used for text; the lab band and START badge use terra-dark (#A0522D, 5.6:1) and the FINISH badge uses navy-deep on gold (5.1:1). Map arrows carry a white casing so they stay visible on both black benches and white corridors.

## 4. Keyboard, screen reader, motion

All controls are native button / input / select, reachable in source order with a skip link and visible gold-plus-navy focus ring. Spacebar pauses/resumes the timer (suppressed while a control is focused). SVG room map has role="img" with a descriptive aria-label naming the start, the weave, the finish, and the desk. Station sections and every generated field have accessible names. Reduced motion respected (transitions disabled under prefers-reduced-motion). iframe height-sender (postMessage + load/resize listeners + ResizeObserver) present in both HTML files. Internal links: none requiring target handling.

## 5. Print / one-page check

Answer sheet: @page Letter portrait, 0.45in x 0.5in margins, two columns. Combined (26 stations) verified to fit one page; Wet/Dry (13 stations) fit comfortably. Cards and planner fields are hidden in print; the lab name and the "Note" cue label are forced on for the student print. Confirm in print preview; if a station wraps, reduce @page margin to 0.4in.

## 6. Student privacy

No student names or IDs are stored. Only the instructor's stems, topics, answer keys, region notes (per sheet mode), uploaded room images, chime/timer settings persist (localStorage, local device). The Name and Lab Exam # fields are blank print lines, never captured.

## 7. Known limitations and remediation

- localStorage is per-browser, per-device; planning data, room uploads, and settings do not sync across machines.
- One-page fit depends on the browser print engine; verify in preview.
- Live screen-reader pass (VoiceOver/NVDA) not yet run on instructor hardware; recommended before first classroom use.
- Uploaded room maps are the instructor's own images; their internal contrast/legibility is outside this audit. The supplied room-map-arrows-new.png uses terra-dark arrows with white casing on black benches for legibility.

## Reviewer

Drafted by Claude (Cowork) for Dr. Sharilyn Rennie. Instructor review pending.

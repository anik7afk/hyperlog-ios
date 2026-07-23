<div align="center">

<img src="art/hero.png" alt="Hyperlog — the hypertrophy training log" width="100%" />

<br/>

**A native iOS training log for people who train in programs, not one-off workouts.**

`Swift` · `SwiftUI` · `SwiftData` · `Swift Charts` · `Supabase` · `iOS 17+`

<sub>This repo is the showcase. The source lives in a private repository.</sub>

</div>

<br/>

## What it does

You author a program as a repeating cycle of days. The app tracks your place in the cycle, so opening it at the gym lands on today's workout with every exercise, target, and rest timer already set. Logging is steppers, not a keyboard: each set arrives pre-filled from the one before, RIR is one tap, and every set carries a tag (warm-up, main, or dropset) so the volume math can ignore warm-ups.

PR detection runs during the session. Estimated 1RM is computed per set as you log it; beat a standing record and the app tells you while you're still on the bench, then files it on the PR board.

<br/>

<p align="center">
<img src="art/card-author.png"  alt="Day editor — sets, reps, rest timers, dropset tags" width="32%" />&nbsp;
<img src="art/card-volume.png"  alt="Weekly hard sets per muscle group" width="32%" />&nbsp;
<img src="art/card-records.png" alt="PR board — standing records per exercise" width="32%" />
</p>

<br/>

## Up close

<img src="art/details.png" alt="e1RM progression with PR markers · set logging steppers with RIR · PR card · per-muscle volume bars" width="100%" />

<div align="center"><sub>The progression chart with PR markers, the set logger, a standing record, weekly sets by muscle.</sub></div>

<br/>

## Body weight and cardio

<img src="art/float-weight.png" align="right" width="285" alt="Body weight trend — weekly averages over raw daily points" />

Daily weigh-ins are noisy, so the chart pushes them back into grey dots and draws the weekly average over them. The trend gets the accent; the noise doesn't.

Cardio sessions have their own timer and their own trend. Live workouts survive force-quits, and an idle reminder catches the session you forgot to end. Everything works offline, and everything exports to JSON or CSV.

<br clear="right"/>

## Design

The app is dark only, and quiet by rule: one full-saturation element per screen, usually the primary button or the hero line on a chart. Everything else is monochrome, and numbers get their hierarchy from size and weight rather than color. Coral has exactly two meanings, cardio and personal records, which is why a coral diamond on a progression line needs no legend.

The rules live in a token layer (spacing scale, radii, type ramp, shared motion), not in per-screen styling.

<img src="art/palette.png" alt="Hyperlog palette — near-black surfaces, ice-blue accent, reserved coral" width="100%" />

## How it's built

The domain layer is plain Swift. `ProgramEngine`, `SessionEngine`, and the analytics engines (progression, muscle volume, PRs, trends) have no UI imports and are tested directly; SwiftUI feature modules sit on top, with the design tokens in a shared layer.

Persistence is SwiftData with versioned schemas. There are thirteen of them so far. Every schema change ships as an explicit migration stage, some with custom data backfill, and each has its own migration tests, because losing a user's training history to a lazy migration is the one unforgivable bug in a logging app.

Sync came later, and the pivot is documented: the app started local-only (ADR-0001), then moved to local-first with a Supabase backend (ADR-0002). SwiftData acts as the on-device cache and write queue, Postgres with row-level security is canonical, and the app stays fully usable offline.

The process is unusually documented for a solo project: architecture decision records, a domain glossary, a written visual-language spec, and about 37 vertical-slice issue docs. Around 223 tests cover domain logic, persistence, migrations, and the sync engine.

The screenshots on this page came from a script. It seeds demo data, deep-links into each screen with launch arguments, and polls the simulator framebuffer until it stops changing before capturing, so twelve app states come out reproducible in one command.

<br/>

<div align="center">
<sub><a href="https://github.com/anik7afk">Mehedi Haque</a></sub>
</div>

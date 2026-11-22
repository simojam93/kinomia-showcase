# kinomia: technical overview

The screens, devices, numbers and architecture behind kinomia. The story of the product and its design is in the [README](README.md). The source code is private.

## What you get

| Screen | What it does |
|---|---|
| Home | Ready, Load and Sleep as three dials, Vitals (how many of your night readings sit in your usual range), daytime stress so far, a daily outlook and today's activities. Arrows move back through past days. |
| Ready | Today against your last 30 days for HRV, resting heart rate, breathing rate and sleep, with one sentence such as "Ready 72, 10 above your usual 62". |
| Load | Time in heart rate zones 1 to 3 and 4 to 5, strength time and steps, with today's Load target. |
| Sleep | Stages, efficiency, consistency, bed and wake times, and the sleep you need tonight. |
| Trends | Every number over 7 days, 4 weeks, 3 months or 1 year. Each weekly bar can be tapped in place. |
| Activity | Every workout from every device, by days or weeks. A workout recorded by two devices shows once, with both recordings merged. Its page has the map, heart rate and the Load it added. |
| Devices | Each device, how it reaches kinomia (through Apple Health or directly over Bluetooth), what it measures and when it last synced. |
| Journal and behaviors | A short daily log, and how each behavior went along with your Ready. |
| Weekly and monthly review | A summary of the last 7 or 30 days. |
| Share | Cards for Ready, Load, Sleep, a week, a workout or your devices, in seven color palettes, sized for a post or a story. |
| Routes | A Komoot tour becomes a course sent to your Garmin. |
| Desk | The iPhone serves a dashboard to a browser on your Mac on the same network, with no server in between. |

## Devices and services

| Device or service | How it reaches kinomia | Status |
|---|---|---|
| Garmin Enduro 2 and other Garmin watches | Through Apple Health from Garmin Connect, and from a Garmin Connect account export for older history | In daily use |
| Garmin Edge 540 | Directly over Bluetooth (Direct mode) | Built, not yet tested on the hardware |
| Amazfit Helio Strap | Through Apple Health from Zepp; directly over Bluetooth | Apple Health in daily use; Direct mode built, not yet tested |
| Whoop | Through Apple Health and from a Whoop export; directly over Bluetooth | Built |
| iPhone | Apple Health | In daily use |
| Strava | Maps for your workouts; sending activities to Strava | Maps in daily use |
| Komoot | Planned tours turned into Garmin courses | Built, waits for tests on the Garmins |

Any other device that writes to Apple Health shows up too, with its name.

## The numbers

| Number | Scale | What it tells you |
|---|---|---|
| Ready | 0 to 100 | How recovered you are this morning |
| Load | 0 to 21 | How much training stress you took on today, with a target for the day |
| Sleep | 0 to 100 | How much of the sleep you needed you got |
| Daytime stress | 0 to 100 | How your day went outside sleep and workouts. Context only, never part of Ready |

Each device counts on its own scale, and kinomia combines them into one number. The morning number is there as soon as one device has synced. Every formula has a version number, and each change shows up in the app.

kinomia shows numbers for training and recovery. It isn't a medical device.

## How your data flows

1. Garmin Connect, Zepp and the other vendor apps write to Apple Health. kinomia reads Apple Health app by app and device by device, after you agree to a health data consent.
2. In Direct mode kinomia talks to the device over Bluetooth, with no vendor app involved.
3. Every reading lands in one SQLite database on the iPhone, one row per signal, source and day. Sources are never merged destructively: each keeps its own rows, and computed values are stored apart with their inputs.
4. The formulas run on the iPhone. The server holds only your email account.
5. Strava maps are fetched when you open a workout and kept for 48 hours at most, as Strava's API rules require.

You can export everything, erase everything, or delete your account from the app.

## Architecture

| Part | Language | What it does |
|---|---|---|
| iPhone app | SwiftUI | The screens, Apple Health reading, the on device database and Bluetooth. |
| `packages/core` | TypeScript | Every formula, written once. The iPhone runs it through JavaScriptCore, so the app and the server always compute the same numbers. |
| `packages/parsers` | TypeScript | Garmin, Zepp and Whoop exports, FIT, GPX and Komoot files. |
| Device protocols | Swift | Garmin, Zepp and Whoop over Bluetooth, written from protocol specifications, with attribution, and tested on Linux. |
| FIT decoder | Swift | Garmin's activity file format. |
| Sync and database | Swift, GRDB | The SQLite database on the iPhone, with the same table shapes as the server. |
| Backend | Supabase (Postgres) | Accounts, and an optional cloud copy that's closed for now. |
| Site | Static | [kinomia.app](https://kinomia.app) |

Shared test files (a "golden day" with known answers) pin the iPhone and the server to the same results, and a check fails the build if the JavaScript bundle on the iPhone drifts from the TypeScript source.

## Status and next steps

kinomia runs on my own iPhone every day with a Garmin Enduro 2, a Garmin Edge 540 and an Amazfit Helio Strap.

1. Test Direct mode on the Garmin Edge 540, so kinomia reads the Garmin without waiting for Garmin Connect.
2. Ship a first launch that asks for your goals and the Apple Health permissions.

Copyright 2026 Simone Lovera. All rights reserved.

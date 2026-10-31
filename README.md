# Kinomia

Kinomia is one hub for all your wearables, and it's easy to use. It's an iPhone app that reads every device you own and gives you three numbers each morning: Ready, Load and Sleep. Your data stays on your iPhone.

<p>
<img src="screenshots/home.png" width="220" alt="Home: Ready 72, Load 13.4, Sleep 95%">
<img src="screenshots/ready-page.png" width="220" alt="Ready 72, with heart rate variability, resting heart rate, breathing rate and sleep">
<img src="screenshots/devices.png" width="220" alt="Your devices: a Garmin Enduro 2, an iPhone, an Amazfit Helio Strap and a Whoop">
</p>

The screenshots run on made up data.

## The problem

People who train with more than one device live with four problems every day.

1. Activities don't reach Strava by themselves. You finish a ride and it isn't there.
2. Garmin Connect doesn't keep up. You finish an activity on your Garmin fēnix and it doesn't show up in the app. In Apple Health, Garmin Connect writes sleep at once, heart rate the next day and heart rate variability (HRV, the variation in time between heartbeats) never.
3. Every device has its own app. A Garmin, an Amazfit Helio Strap and a Whoop mean three apps to open each morning.
4. Two devices that track the same night or the same workout give two different values, and you don't know which one to look at.

## What Kinomia does

Kinomia puts every device in one app and turns their data into one value. Our model combines what each device measures and takes from each device what it measures well. Data is organised by signal (heart rate, HRV, sleep, workouts), and every reading keeps its source.

You get one Ready, one Load and one Sleep each morning. Next to each Kinomia number you can see what every device said. Ready shows up as soon as one device has synced: it can take the night from one device and heart rate and HRV from another.

Your data stays on the iPhone. Kinomia reads Apple Health and does every calculation on the phone, so health data never goes through a server. Strava maps are kept for 48 hours at most.

<p>
<img src="screenshots/load-page.png" width="220" alt="Load 13.4 of 21, with heart rate zones, strength and steps">
<img src="screenshots/trends.png" width="220" alt="Trends: Ready and HRV over 7 days">
<img src="screenshots/activity-detail-strava.png" width="220" alt="A ride with its map from Strava and its heart rate">
</p>

## Where it stands

Kinomia runs on my own iPhone every day with a Garmin Enduro 2, a Garmin Edge 540 and an Amazfit Helio Strap. Whoop support is in progress. Direct mode, which talks to the devices over Bluetooth with no vendor app on the phone, is built and hasn't been tested on the hardware yet.

I started on 4 October 2026. In four weeks the private repository reached 799 commits and about 109,000 lines of Swift, TypeScript and SQL, with 462 test files across 14 Swift packages and 6 TypeScript packages. The formulas live once, in TypeScript, and the iPhone runs them through JavaScriptCore, so the app and the server always compute the same numbers.

## Next

1. Test Direct mode on the Garmin Edge 540, so Kinomia reads the Garmin without waiting for Garmin Connect.
2. Wear Garmin and Whoop together for 14 days to calibrate the Whoop readings.
3. Ship a first launch that asks for your goals and the Apple Health permissions.

## About this repository

This is a public overview. The source code is private. Site: [kinomia.app](https://kinomia.app).

Copyright 2026 Simone Lovera. All rights reserved.

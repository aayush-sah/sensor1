# SirenNet+ — Predictive Emergency Vehicle Tracker (WSN from Phones)

**Purpose:** Evolve the siren-detection WSN into a **predictive tracker** that estimates the **route, speed, and ETA** of an approaching emergency vehicle using **crowdsourced phone detections**.

## What’s new vs SirenNet
- **Centroid tracking**: Aggregates detections from multiple phones into 2‑second **centroids**.
- **Velocity & bearing**: Computes speed and heading from the latest centroids.
- **Prediction**: Projects the next 60 seconds of motion, drawing a **predicted path**.
- **Checkpoints & ETA**: CTRL+Click to drop checkpoints. The dashboard shows **ETA** to each checkpoint.

## Run in minutes (no installs)
1. Open `laptop.html` on your laptop.
2. Open `phone.html` on each phone (Android Chrome recommended).
3. Phone: **Grant Mic & Location** → **Create Offer** → copy JSON.
4. Laptop: paste Offer → **Accept & Create Answer** → copy JSON back → **Set Answer**.
5. Phone: **Start Detection**.
6. On the map: **Ctrl+Click** to add one or more **checkpoints**; ETAs appear when a predicted route is available.

## How prediction works
- Keeps the last **90 seconds** of positive, geotagged detections (confidence > 0.7).
- Bins into **2‑second windows** and computes a **confidence-weighted centroid** per bin.
- Uses the latest two centroids to estimate **speed (m/s)** and **bearing (°)**, clamped to 2–22 m/s (~7–80 km/h).
- Projects positions every 5 seconds for 60 seconds and draws the polyline.
- Computes **ETA to checkpoints** using current speed.

## Notes
- Prototype detector is rule-based; for production use a small **on‑device CNN** (TF.js) trained on siren Mel-spectrograms.
- iOS Safari may require HTTPS for mic; Android Chrome is easiest.

## Files
- `phone.html` — on-device siren detector + WebRTC sender
- `laptop.html` — predictive dashboard (Leaflet map + alerts + ETA + CSV)
- `README.md` — overview & instructions


# Thunder Player — browser TEST build

Minimal tablet test harness for the Ryuboku aquarium storm engine.
Open `index.html` via the local server (not as a `file://` URL — browsers
block audio fetches that way).

```
python3 serve.py 8765
```

Then open `http://127.0.0.1:8765/` on the same machine, or the
trycloudflare.com URL if a Cloudflare quick tunnel is running.

On the tablet:

1. Tap **Tap to start audio** (required on iOS).
2. Tap **Test sound** and check that FLAC and WAV both show OK.
3. Set the storm length (1–50 minutes, default 3) and tap **Test storm**.

Audio is lossless stereo FLAC (plus one WAV copy). See `LICENSES.txt`.

Limitations: the public tunnel URL is temporary; output is stereo only
(not 7.1); an iPad/iPhone silent switch can mute web audio.

## Long-storm rain build (added 17 Nov 2026)

- Storm length slider is now 1–50 minutes (default 3). A **Quick test:
  accelerate ×10** checkbox compresses the timeline 10× (a 30-minute storm
  plays in 3:00); the log shows real vs compressed length.
- Storms of **8 minutes or more** (real length) add a continuous **rain bed**
  for the whole storm: a seamless 32 s loop of a recorded rain-on-leaves
  FLAC (`audio/rain_bed.flac`), through its own lowpass → stereo panner →
  gain. The earlier synthesised pink-noise layer (30% mix) and the rumble-
  chunk bed have been removed, because the noise sounded like hiss/static
  through speakers.
  - Approach (first 32%): gain −46 dB (0.005) → 0 dB on an S-curve in dB,
    lowpass 250 Hz → 11 kHz (log scale), pan ±0.9 on the arrival side → centre,
    playback rate 0.90 → 1.00. Automated with 1 s-resolution value curves.
  - Peak (32–64%): held at full, open and centred. Bed level 0.5.
  - Recession: the reverse, drifting to the exit side, with the last 15%
    fading to silence.
  - Approach/recession clips follow the same distance (gain × envelope^0.6,
    cutoff ≈ the bed's cutoff ×0.8–1.25, pan follows the bed) and swell in
    over 6–20 s (÷10 in quick-test mode, min 0.6 s).
- Storms under 8 minutes are unchanged: no bed, rain/rumble clips swell in
  over 2–8 s, peak cracks 0.02 s.
- Stop fades the bed out over 0.6 s. The phase label shows "rain N% away"
  and the log reports rain distance every ~5% step.
- Backup of the previous page: `index.backup-before-long-rain.html`.

## Crack clip swap (17 Nov 2026)

- `crack_hof.flac` was removed: its first ~10 s is steady rain/wind noise
  (spectrally flat, centroid ~2.5 kHz, ~−21 dBFS) and the actual thunder only
  arrives ~10.5 s in, so peak-phase lightning strikes produced a hiss.
- Replaced by `crack_thunder4.flac` — BigSoundBank "Thunder #4" (CC0), a dry
  close thunderclap with no rain, trimmed so it hits immediately.
- Backups of the previous page/licences and the old clip:
  `backups/2026-11-17-before-crack-swap/` (workspace only, not deployed).

## Rain bed swap (18 Nov 2026)

- The long-storm rain bed no longer uses synthesised noise or sliced rumble
  clips. It loops `audio/rain_bed.flac`, a Public Domain recording of rain
  on leaves (raphael.malfliet, radio aporee / Internet Archive).
- The distant-approach envelope is unchanged: gain −46 dB → 0 dB on an
  S-curve, lowpass 250 Hz → 11 kHz, pan ±0.9 → centre, playback rate
  0.90 → 1.00; short storms still swell rain/rumble clips over 2–8 s.
- Backup of the previous page, licences and README:
  `backups/2026-11-18-before-rain-swap/` (workspace only, not deployed).

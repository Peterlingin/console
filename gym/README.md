# Workout Timers

Two self-contained workout timers for your home network, plus a small menu page.
No build step, no external dependencies. English interface.

## Files

| File | Description |
|------|-------------|
| `index.html` | Menu page linking both timers |
| `emom.html` | EMOM timer (interval, rounds, warm-up) |
| `sets.html` | Sets timer (sets, set duration, rest, total workout time) |
| `manifest.webmanifest`, `icon.svg`, `icon-192.png`, `icon-512.png` | Make the pages installable as an app |
| `sw.js` | Service worker: offline use and automatic updates |

Keep all files in the same folder. `emom.html` and `sets.html` also work on their own; the extra files only add the menu, install and offline features.

## EMOM Timer

- Interval (10 s to 10 min), rounds (1 to 99), warm-up (0 to 60 s), presets of 10, 15, 20 and 30 minutes
- Countdown ring with colour phases, beeps in the last 3 seconds, a longer beep at each round, vibration
- Pause, resume, stop, and a lock button against accidental taps (hold 0.7 s to unlock)

## Sets Timer

- Sets (1 to 30), set duration (5 s to 10 min), rest (5 s to 10 min)
- A 5-second get-ready countdown before set 1
- Each set counts down; **SET DONE** finishes it early. Rest has -15 s, +15 s and SKIP
- Pause/resume and **END** are available at any moment
- Recap shows the total workout time; **ADD EXERCISE** keeps the total running across exercises

## Voice announcements

Both timers can announce phases ("Get ready", "Round 6, halfway", "Last set", "Rest, 1 minute 30 seconds", "Workout complete"). Use the **Voice** switch on the setup screen; the choice is shared by both pages and remembered.

## Hosting

Copy the folder to any machine that serves static files (PC, NAS, Raspberry Pi, router). Quick test with Python:

```bash
cd /path/to/folder
python3 -m http.server 8080
```

Open `http://<machine-ip>:8080/` on your phone.

## HTTPS (screen always on, install, offline)

The browser only allows "keep the screen awake", app install and offline mode over HTTPS (or `localhost`). Over plain HTTP everything still works, except that the screen may turn off.

Easiest option with [Caddy](https://caddyserver.com), using its built-in local certificate authority. Example `Caddyfile`:

```
workout.lan {
    tls internal
    root * /srv/workout
    file_server
}
```

Point `workout.lan` at the machine (router DNS or `hosts` file). Then trust Caddy's root certificate on the phone, otherwise Chrome will show a warning. The root certificate is `root.crt` inside Caddy's data folder (for example `~/.local/share/caddy/pki/authorities/local/` on Linux). On Android: Settings > Security > Encryption & credentials > Install a certificate > CA certificate.

Alternative without HTTPS: in Chrome on Android open `chrome://flags`, search "Insecure origins treated as secure", add your `http://<machine-ip>:8080` address, enable it and relaunch. This enables the wake lock, but not install/offline.

## Installing as an app

Once served over HTTPS, open the menu page in Chrome and choose **Install app** (or **Add to Home screen**). The app then opens full-screen and works offline. Pages are fetched fresh when you are online, so updating a file on the server updates the app.

## Limitations

- Browsers pause pages when the screen locks: keep the screen on during a workout. Beeps and voice stop otherwise.
- Audio and voice start only after you tap START (a browser requirement).
- Settings and the recap live in the browser, per device and per address.

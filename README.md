# Drowsy Watch

A real-time drowsiness detection website. It uses your device's webcam to track eye landmarks in the browser, calculates the **eye aspect ratio (EAR)**, and raises a visual + audio alert when your eyes stay closed too long — all processed locally, with nothing uploaded anywhere.

## Files

- `drowsiness-detection.html` — the full site (structure, styling, and detection logic in one file)

## How it works

1. **Capture** — [face-api.js](https://github.com/justadudewhohacks/face-api.js) locates your face each frame and maps 68 landmark points, including 6 around each eye.
2. **Measure** — those points are reduced to a single number, the eye aspect ratio, which drops sharply when eyelids close.
3. **Decide** — a quick dip is logged as a blink; if the ratio stays low past the alert delay (adjustable on the page), it's logged as a drowsy event and triggers an alarm.

## Why it needs proper hosting

Browsers only allow camera access (`getUserMedia`) on **secure contexts** — `https://` pages or `http://localhost`. Opening the HTML file directly from disk (`file://...`) or through an in-app preview blocks camera access entirely. To use this for real, it needs to be served from an actual URL.

## Running it on desktop

From a terminal in the folder containing the file:

```bash
python3 -m http.server 8000
```

or, with Node installed:

```bash
npx serve
```

Then open `http://localhost:8000/drowsiness-detection.html` in your browser and allow the camera prompt.

## Running it on mobile (no computer needed)

1. Create a free account at [github.com](https://github.com).
2. Create a new **public** repository (e.g. `drowsy-watch`).
3. Upload `drowsiness-detection.html`, renaming it to `index.html`.
4. Go to **Settings → Pages**, set source to `Deploy from a branch`, branch `main`, folder `/ (root)`. Save.
5. After about a minute, GitHub gives you a live link: `https://yourusername.github.io/drowsy-watch/`.
6. Open that link in Chrome (or any mobile browser) and tap **Start monitor** — you'll get a real camera permission prompt.

## Using the site

- **Start monitor** — requests camera access and loads the face-tracking models (first load takes a few seconds).
- **Calibrate** — hold your eyes open normally for 1.5 seconds so the alert threshold adapts to your face and lighting instead of using a fixed number.
- **Alert delay slider** — controls how long your eyes must stay closed before it counts as drowsy rather than a blink.
- The sidebar tracks blink count, drowsy events, session time, and live EAR value, with a rolling chart of the EAR signal.

## Notes and limits

- All processing happens locally in the browser tab; no video or frames leave the device.
- This is a demo/educational tool, not a certified safety device — it shouldn't replace actual rest.
- Detection accuracy depends on lighting and camera angle; use Calibrate after adjusting position.

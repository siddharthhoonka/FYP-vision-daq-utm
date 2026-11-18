# Vision DAQ — Tensile Test Video Analyser

Browser app for the final-year project **"Development of a Vision-Based Data Acquisition System for Legacy UTM Systems"** (Dept. of Metallurgical and Materials Engineering, VNIT Nagpur).

Give it a video of a tensile test that shows the specimen and the machine's load display. It returns the stress–strain curve, the mechanical properties, and the raw data.

Everything runs in the browser. The video is never uploaded anywhere, and there is nothing to install — the whole app is the single file `index.html`.

## What it does

| Step | Method |
|---|---|
| Input | A recorded video file, or a **live camera** (the test can also be recorded to a video file while it runs) |
| Load, P | Reads the digits on the UTM display in every sampled frame (seven-segment reader; Tesseract.js option for printed digits; or a CSV load log). Misread frames can be corrected in a review table |
| Strain, ε | Shi–Tomasi corner detection + pyramidal Lucas–Kanade optical flow on the specimen surface. Gauge strain is the extension between the two ends of the gauge box (a virtual extensometer), so it needs no pixel-to-mm calibration and ignores frame and grip deformation |
| Strain, ε (two-display mode) | For machines that show extension: reads the LOAD and EXTENSION displays together and uses strain = extension ÷ L₀, so the specimen needs no pattern. If the extension read-out is crosshead travel it includes machine and grip deformation, so the modulus from it is not valid |
| Strain map | Full-field local strain drawn over the specimen, with the neck position and peak local strain reported |
| Stress, σ | σ = P / A₀ |
| Properties | Modulus, 0.2 % offset yield / proof strength, tensile strength, uniform and fracture elongation, toughness, resilience, strain-hardening exponent n and K, apparent Poisson's ratio, elastic stress rate |
| Standards | Names and rounding in plain form, ASTM E8/E8M, or IS 1608 / ISO 6892-1, with checks on stress rate, fracture position and gauge length |
| Compare | Overlay up to 6 tests on one graph; compare measured values with typical values for common materials |
| Output | Engineering and true σ–ε curves, load–time, strain–time, load–elongation, PDF report, CSV, PNG, and a saved-test file that restores the results later without the video |

## How to use

1. Open the page. Either drop in the test video (MP4 / WebM / MOV), or choose **Live camera** and start the camera. "Try the built-in demo test" runs a synthetic test with a known answer.
2. Enter the specimen cross-section, gauge length L₀ and the unit shown on the load display.
3. Press **Auto-detect**, or draw the **gauge region** on the specimen and the **load display** box around the digits yourself. Check that the read-out matches the display.
4. Video file: set start/end times if there is dead time, then press **Analyse video**. Live camera: press **Start test**, run the machine, and press **Stop** when the specimen breaks.
5. Fix any misread load values in the review table, pick the standard, then export the PDF report or save the test.

Notes:

- LCD displays (dark digits on a lit window) are supported: set "Digits are" to "Dark on light" if Auto picks wrongly, and draw the box around the whole lit window.
- Two videos of the same test (specimen and control panel) must be combined into one synchronised video first; a clap or the sound of the specimen breaking can be used to line them up.
- Live camera needs the page to be served over https (GitHub Pages is fine) and camera permission.
- Auto-detect of the gauge region works on recorded videos only, because it finds the part of the picture that stretches.
- Rounding rules and rate limits in the standards modes should be checked against the current text of the standard before being quoted in a formal report.

## Filming tips

- The specimen needs visible texture: a sprayed speckle pattern (as in DIC) works best.
- Fix the camera on a tripod, square to the specimen, with steady lighting and no glare on the display.
- Fill as much of the frame as possible with the gauge section — elastic strains are tiny, so Young's modulus is the most resolution-sensitive result.
- The box drawn on the specimen is treated as the gauge length L₀.

## Validation status

Checked against the built-in synthetic test (E = 70 GPa, σy = 250 MPa, UTS = 400 MPa), including after H.264 compression and through a simulated camera feed: UTS and yield strength agree within about 2–4 %; Young's modulus within about 5–20 %. Two-display mode has been run on one real test (18 Nov 2026, LCD panel): the load display was read in 98 % of frames and the extension display in 88 %, with misreads rejected automatically. It has **not yet been validated against an extensometer or a certified specimen** — that is the project's next step (standard SS 304 data).

## Deploy on GitHub Pages

1. Create a repository (for example `vision-daq-utm`) and add `index.html` and `README.md`.
2. Settings → Pages → Source: *Deploy from a branch* → `main` / root → Save.
3. The app is live at `https://<username>.github.io/vision-daq-utm/`.

## Team

Siiddharth Hoonka, Ayush Gandhi, Yatharth Nema — mentors Dr. Abhinav Arya and Dr. Seelam Rajasekhar Reddy.

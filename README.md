# Vision DAQ — Tensile Test Video Analyser

Browser app for the final-year project **"Development of a Vision-Based Data Acquisition System for Legacy UTM Systems"** (Dept. of Metallurgical and Materials Engineering, VNIT Nagpur).

Give it a video of a tensile test that shows the specimen and the machine's load display. It returns the stress–strain curve, the mechanical properties, and the raw data.

Everything runs in the browser. The video is never uploaded anywhere, and there is nothing to install — the whole app is the single file `index.html`.

## What it does

| Step | Method |
|---|---|
| Load, P | Reads the digits on the UTM display in every sampled frame (seven-segment reader; Tesseract.js option for printed digits; or a CSV load log) |
| Strain, ε | Shi–Tomasi corner detection + pyramidal Lucas–Kanade optical flow on the specimen surface; strain is the fitted gradient of displacement along the pull direction, so it needs no pixel-to-mm calibration and ignores frame and grip deformation |
| Stress, σ | σ = P / A₀ |
| Properties | Young's modulus, 0.2 % offset yield strength, UTS, uniform and fracture elongation, toughness, resilience, strain-hardening exponent n and K, apparent Poisson's ratio |
| Output | Engineering and true σ–ε curves, load–time, strain–time, load–elongation, data table, CSV and PNG export, printable report |

## How to use

1. Open the page and drop in the test video (MP4 / WebM / MOV). "Try the built-in demo test" runs a synthetic test with a known answer.
2. Enter the specimen cross-section, gauge length L₀ and the unit shown on the load display.
3. Draw the **gauge region** on the specimen between the grips, and the **load display** box tightly around the digits. Scrub the video and check the read-out matches.
4. Set start/end times if the video has dead time, then press **Analyse video**.

## Filming tips

- The specimen needs visible texture: a sprayed speckle pattern (as in DIC) works best.
- Fix the camera on a tripod, square to the specimen, with steady lighting and no glare on the display.
- Fill as much of the frame as possible with the gauge section — elastic strains are tiny, so Young's modulus is the most resolution-sensitive result.
- The box drawn on the specimen is treated as the gauge length L₀.

## Validation status

Checked against the built-in synthetic test (E = 70 GPa, σy = 250 MPa, UTS = 400 MPa), including after H.264 compression: UTS, yield strength and elongation agree within about 2 %; Young's modulus within about 5–15 %. It has **not yet been validated against a real UTM video or an extensometer** — that is the project's next step (standard SS 304 data).


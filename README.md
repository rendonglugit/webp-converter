# WebP Batch Converter

Drop in a folder or a ZIP of images, get WebP back — with adjustable quality, true lossless mode, and the original directory structure preserved.

Everything runs locally in your browser. **Images are never uploaded**, and the page makes no network requests at all.

**English** | [简体中文](README.zh-CN.md)

## Live

https://rendonglugit.github.io/webp-converter/

## Why this exists

Android Studio has a handy *Converting Images to WebP* dialog, but it only serves Android projects. Everyone else — designers, front-end devs, content folks — still ends up dragging images one at a time into online converters that ask you to upload your assets to someone else's server.

This is that dialog, rebuilt for the browser. Drop files in, pick a quality, download a ZIP. Nothing to install, nothing to upload.

## Features

| | |
|---|---|
| **Input** | Drag a folder, drag a ZIP, or pick files manually |
| **Output** | Individual `.webp` files, or a single ZIP |
| **Lossy** | Quality 0–100, defaults to 75 |
| **Lossless** | True lossless (libwebp VP8L) — pixel-identical output |
| **Compare** | Split slider, side-by-side, or a pixel-difference map — zoom to 100/200/400% to inspect artifacts |
| **Review mode** | Optional per-image confirmation: keep the original or accept the WebP |
| **Skip rules** | Keep original if WebP ends up larger; skip nine-patch `.9.png`; skip images with alpha |
| **Structure** | ZIP paths are preserved — unzip and drop it straight into your project |
| **Non-images** | `.xml` / `.json` / `.txt` etc. are passed through untouched |
| **Formats** | PNG · JPG · JPEG · BMP · GIF (first frame) · AVIF · WebP |

## Usage

1. Drag a folder or ZIP onto the page (or click **Choose folder**)
2. Set quality on the left — 75 is a good default
3. Check the size deltas; click **Compare** (or any row) to inspect before/after
4. Click **Download ZIP**

Inside the ZIP, `assets/icons/a.png` becomes `assets/icons/a.webp`. Everything else keeps its original path.

### Inspecting a conversion

The compare view has three modes and four zoom levels:

- **Split** — drag the divider to wipe between original and WebP
- **Side by side** — both images at once, for judging overall fidelity
- **Difference map** — per-pixel delta as a heatmap, plus average error, peak error and the share of pixels that changed

Zooming to 200% or 400% is the practical way to spot blockiness in gradients and ringing around text. When the image is larger than the viewport, drag to pan.

Keyboard: `←` `→` to step through images, `0` to reset zoom, `Esc` to close.

## Offline use

The whole tool is **one HTML file** with the WebP encoder (libwebp compiled to WebAssembly) embedded inside.

Save `index.html`, double-click it, and it works — no internet required. Sending it to a colleague means sending one file.

## Browser support

Recent versions of Chrome, Edge, Safari and Firefox.

- If the browser can encode WebP natively, it uses that (faster)
- Otherwise it switches to the bundled libwebp engine automatically
- The badge in the top-right always shows which engine is actually in use

## How it works

- **No dependencies** — no npm packages, no CDN, no build step
- **WebP encoding** — Canvas `toBlob('image/webp')` when available; falls back to the embedded libwebp WASM for lossless mode or when native support is missing
- **ZIP reading** — hand-rolled central-directory parser on top of the native `DecompressionStream('deflate-raw')`, handling both UTF-8 and GBK filenames
- **ZIP writing** — hand-rolled STORE-mode writer (WebP is already compressed, so re-compressing would only add cost)
- **Size** — ~470 KB total, of which 375 KB is the base64-encoded WebAssembly encoder

## License

- This tool: MIT — see [LICENSE](LICENSE)
- Bundled WebP encoder: libwebp (BSD-3-Clause, Copyright (c) 2010 Google Inc.), built via [@jsquash/webp](https://github.com/jamsinclair/jSquash) (Apache-2.0)

If you redistribute this file, keep the attribution in [NOTICE](NOTICE).

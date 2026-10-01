# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page static site: a landscape A5 "flipbook" rendering of the New Muslim Centre's Impact Report 2026. There is no build system, package manager, linter, or test suite — the entire site is one hand-authored `index.html` plus an `assets/` folder of images, deployed as-is via GitHub Pages (`.nojekyll` forces raw static serving, bypassing Jekyll).

## Working locally

There is no build step. To preview changes, either open `index.html` directly in a browser or serve the directory so relative asset paths resolve correctly, e.g.:

```
python3 -m http.server 8000
```

There are no lint or test commands — verify changes by opening the page and checking the flipbook navigation, scaling, and print output in-browser.

## Architecture

- `index.html` is a single, largely minified line containing:
  - All CSS inline in a `<style>` block, including a Quicksand variable font embedded as a base64 `data:` URI (no external font requests).
  - All markup for the 23 "pages" of the report, grouped into `.sheet` elements (each sheet = one spread/page-pair shown at a time).
  - One `<script>` block (~1.2KB, vanilla JS, no dependencies) that implements the entire flipbook behavior:
    - Toggles which `.sheet` is visible via `hidden`, driven by `current` index.
    - Prev/Next buttons, arrow keys, and Home/End keys all call `show(index)`.
    - `fit()` scales the whole `.book` to the viewport against a fixed design size of 1588×560, recalculated on `resize`.
    - A print button calls `window.print()` directly — print/PDF export relies on CSS, not a separate print template.
  - Editing a page's content means finding the right `.sheet` block inside this one file; editing navigation/scaling behavior means editing the single `<script>` block.
- `assets/` holds all images referenced by `<img src="assets/...">` in `index.html`. Images are a mix of real project photography, a licensed Unsplash photo, and AI-generated illustrations/concepts — **always check `image-credits.md` before adding, replacing, or redistributing an image**, and update it when the provenance of an image changes (e.g., swapping an AI illustration for real photography, or upscaling/editing an existing photo).
- `image-credits.md` is the source of truth for what each image is, where it came from, and what editing (upscaling, background removal, color grading) has been applied — keep it in sync with `assets/`.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page shipping form built with plain HTML, CSS, and Alpine.js (no build step, no other JS frameworks). The entire application lives in `index.html`.

## Architecture

All logic, styling, and markup are in one file:

- **Alpine.js** (loaded from CDN) drives reactivity via `x-data`, `x-model`, `x-show`, and `x-text` directives — no separate JS files.
- The `x-data` object on the root `.card` div holds `form` (the live input state) and `submitted` (a snapshot set on submit). Submitting copies `form` into `submitted`, which causes the summary panel to appear via `x-show`.
- There is no backend, build tool, bundler, package manager, or test suite.

## Development

Open `index.html` directly in a browser — no server needed. Alpine.js is pulled from the jsDelivr CDN, so an internet connection is required to run it.

## Constraints

- Do not introduce JS frameworks beyond Alpine.js.
- Do not add a build step or package manager unless explicitly requested.

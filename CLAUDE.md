# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page employee-facing shipping form for a retail book/media store with multiple locations. Staff fill it out when processing customer shipments. Built with plain HTML, CSS, and Alpine.js — no build step, no other JS frameworks. The entire application lives in `index.html`.

## Architecture

All logic, styling, and markup are in one file:

- **Alpine.js** (loaded from jsDelivr CDN) drives reactivity via `x-data`, `x-model`, `x-show`, and `x-text` — no separate JS files.
- The `x-data` object on the root `.card` div holds `form` (live input state), `submitted` (snapshot set on submit), `helpOpen` (boolean controlling the Help modal), and `submitAttempted` (boolean set on first print attempt, used to gate the contact-method validation error). Submitting shallow-copies `form` into `submitted`, which makes the summary panel appear via `x-show`.
- The Print button validates that at least one of `email` or `phone` is filled (custom logic in `submit()`) in addition to the standard HTML5 `required` checks. `submitAttempted` is set to `true` on every submit call so the error only appears after the first attempt.
- `date` is initialised to today (`new Date().toLocaleDateString('en-CA')`) and reset to today when the Clear button is used.
- There is no backend, build tool, bundler, package manager, or test suite.

## Form Fields

The `form` state object has these keys (in order of appearance):

| Key | Type | Required |
|-----|------|----------|
| `initials` | text | yes |
| `store` | radio: 112th / Broadway / LIC / Pittsford | yes |
| `date` | date (auto-filled to today) | yes |
| `name` | text (ship-to name) | yes |
| `address` | textarea | yes |
| `customerName` | text (customer name) | yes |
| `email` | text (customer email) | at least one of email/phone |
| `phone` | tel | at least one of email/phone |
| `weightLbs` | number (whole pounds) | yes |
| `weightOz` | number (ounces, 0–15) | only if weightLbs is 0 or empty |
| `method` | radio: USPS Media Mail / USPS Ground Advantage / USPS Priority / UPS Ground | yes |
| `emailTracking` | radio: Yes / No | yes |
| `specialRequests` | textarea | no |
| `sendReceipt` | radio: Yes / No | yes |

## CSS Patterns

Two radio button layouts are used:

- **`.radio-group`** — horizontal flex row, used for short single-word options (Store, Email tracking?, Send receipt?).
- **`.radio-option` + `.radio-option-text`** — vertical stacked layout with a bold label `<span>` and a `<small>` description line, used for Shipping Method where each option has a description.

The Help modal uses `.modal-backdrop` (fixed full-screen overlay, closes on outside click) containing `.modal`. It is toggled via `helpOpen`. The modal body currently holds placeholder text to be replaced with real help content later.

## Print Layout

The Print button (`@submit.prevent`) snapshots the form, then calls `window.print()` via `$nextTick` + 50ms `setTimeout` (the delay is required for Safari to render before the print dialog freezes the pipeline).

Print-specific CSS (`@media print`) hides the form, Help/Clear buttons, and required legend, and shows several print-only elements:

- **`.method-banner`** — large bold all-caps banner at the very top, shown for all methods except USPS Media Mail. USPS Priority adds ⚠️ before and after the method name. Hidden on screen via `@media screen`.
- **`.print-meta`** — one line showing initials and store (e.g. `AB — 112th`), shown above the address block. Hidden on screen.
- **`.print-date`** — date shown right-aligned in the header next to "Shipping Form". Hidden on screen.
- **`.address-block`** — name and address displayed together as a package-label block, using a disambiguating monospace font stack (`Consolas, Menlo, Monaco, 'Lucida Console', 'Courier New', monospace`) with `white-space: pre-wrap` to preserve address line breaks.
- **`.mono-value`** — applied to the email value in the summary; uses the same monospace font stack in print.

The Initials, Store, and Date rows are hidden in the print summary grid (`.initials-row`, `.store-row`, `.date-row`) since they appear elsewhere in the print layout.

## Version Number

The version string in the `<span class="version">` inside the `<h1>` must be updated to `vYYYY.MM.DD` (e.g. `v2026.04.20`) whenever Claude resumes work on this project. Do this as the first change of each session before committing anything else.

## Development

Open `index.html` directly in a browser — no server needed. Alpine.js requires an internet connection (CDN).

## Constraints

- Do not introduce JS frameworks beyond Alpine.js.
- Do not add a build step or package manager unless explicitly requested.
